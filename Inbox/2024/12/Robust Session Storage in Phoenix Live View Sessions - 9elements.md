---
created: 2024-12-04T12:09:49 (UTC -03:00)
tags: []
source: https://9elements.com/blog/robust-session-storage-in-phoenix-live-view-sessions/?utm_medium=email&utm_source=elixir-radar
author: By Daniel Hoelzgen
---

# Robust Session Storage in Phoenix Live View Sessions - 9elements

> ## Excerpt
> For an internal project called ‘ControlManiac’, which helps us with all the boring numbers juggling in our software studio, I had to add a global filter to the navigation bar, which lets me select the division I want to filter the data for on...

---
For an internal project called ‘ControlManiac’, which helps us with all the boring numbers juggling in our software studio, I had to add a global filter to the navigation bar, which lets me select the division I want to filter the data for on whatever view I am currently on.

Since we often switch views while using this app, it should keep its selection. It should also keep it upon reloading or revisiting the page and upon deployment of a new version.

This is what it looks like; it is the toggle element on the right:

![ControlManiac navigation bar](https://9elements.com/images/ctfl/28PNLoOUZTgaKqswsVmksx-352w-embedded.jpeg)

The Home, Projects, and Costs navigation options are all separate live views, within the same `live_session`.

I thought there would be a quite easy solution, but it turned out that all common approaches had their downsides:

-   Keeping it in the `assigns` only is not an option, since this is cleared upon changing the current live view.
-   Storing it in a session cookie would ensure it survives a full page reload but brings two problems: First, the session information is not reloaded upon changing the live views within the same `live_session`, because this is done entirely via web sockets. Thus, we would get the initial value the session head at the time of the first HTTP request. This brings me to the second problem: I neither can update the session since there is no HTTP request upon live navigation, and the session cookie cannot be updated via web socket.
-   Storing it in an ETS table solves the problem of switching live views within the same `live_session`, but of course, it would not survive an app restart, as is the case upon deployment of a new version.

I could, of course, pursue the session approach and just make sure to add a parameter every time I switch between the main live views, but this does feel very odd.

I eventually decided to go with a classic cookie-based session combined with an additional layer stored in an ETS table.

## Preparing the pipeline

To be able to load data via plug and `on_mount`, we first implement a module `ControlManiacWeb.DivisionSelector` and added it to the pipline:

```elixir
elixirdefmodule ControlManiacWeb.Router do
  use ControlManiacWeb, :router

  pipeline :browser do
    ...
    plug :fetch_current_division
  end

  ...

  scope "/", ControlManiacWeb do
    pipe_through [...]

    live_session :require_authenticated_user,
      on_mount: [
...,
        {ControlManiacWeb.DivisionSelector, :mount_current_division}
      ] do
      ...
    end
  end

  ...
```

-   `fetch_current_division` pug is added to the pipeline. Its task is to fetch the current division from cache or session and add it to `conn.assigns`.
-   `mount_current_division` is used for the live views. Its task is to load the current division from cache and add it to `socket.assigns`.

## Storing settings in the ETS table

Before we come to the implementation of these functions, lets have a look at how we implement the cache using an ETS table.

```elixir
elixirdefmodule ControlManiac.SettingsCache do
  use GenServer

  @name __MODULE__
  @tab :settings_cache
  @ttl 60 * 60 * 24 * 31

  # Client

  def start_link(_), do: GenServer.start_link(__MODULE__, [], name: @name)

  def insert(key, value) do
    expiration = :os.system_time(:seconds) + @ttl
    :ets.insert(@tab, {key, value, expiration})
  end

  def get(key, default \\ nil) do
    lookup =
      case :ets.lookup(@tab, key) do
        [{_, value, _} | _] -> value
        [] -> default
      end

    lookup
  end

  # Server

  def init(_) do
:ets.new(
      @tab,
      [:set, :named_table, :public, read_concurrency: true, write_concurrency: true]
    )

    {:ok, []}
  end

end
```

## Retrieving data

We now can use our SettingsCache together with normal sessions to implement the desired behavior for the pipeline. Please note that this implementation assumes we have a user present for whom we can store the data, and that we want to store the data on a per-user basis.

```elixir
elixirdefmodule ControlManiacWeb.DivisionSelector do
  import Plug.Conn

  alias ControlManiac.Accounts.User
  alias ControlManiac.SettingsCache

  @default_division -1
  @session_division_key "current_division_id"
  @cache_division_key :division_cache

  def fetch_current_division(conn, _opts) do
    current_user = conn.assigns[:current_user]

    assign(
      conn,
      :current_division,
      get_division_from_cache(current_user) ||
        get_division_from_session(conn, current_user) ||
        @default_division
    )
  end

  defp get_division_from_cache(nil), do: nil

  defp get_division_from_cache(%User{id: user_id}) do
    SettingsCache.get({@cache_division_key, user_id}, nil)
  end

  defp get_division_from_session(conn) do
    get_session(conn, @session_division_key)
  end

  ...

end
```

## Handling data for live views

So here comes the implementation of the `on_mount/4` function used for live views:

```elixir
elixirdef on_mount(:mount_current_division, _params, session, socket) do
  division_id =
    get_division_from_cache(socket.assigns[:current_user]) ||
      get_division_from_session_map(session) ||
      @default_division

  {
    :cont,
    Phoenix.Component.assign(
      socket,
      :current_division,
      division_id
    )
  }
end
```

We do this to handle a specific edge case: In case the server restarts and the socket connection is initiated again, we lost our cache. Upon reconnecting the socket there is no run through the plug pipeline (so, no `fetch_current_division/2` invoked), but since reconnection is done via XHR request, we get updated session info. This is especially useful after deployments: When the system reconnects, we get the correct value from the session.

We need the fallback to session info anyway because of this edge case, so we can also use it as a fallback for the first requests, where the ETS table cache might not be warm although a division is set in the session. This way, we don’t have to make sure the cache is up to date in `fetch_current_division/2:` We use the value from the initial session, and as soon as the user changes the division, the ETS table cache entry is created and the fallback will never be reached.

Since in `on_mount/4` we get the information stored in the session as a map, the function to retrieve it is slightly different:

```elixir
elixirdefp get_division_from_session_map(session) do
  if division_id = Map.get(session, @session_division_key) do
    String.to_integer(division_id)
  else
    nil
  end
end
```

So how do we make sure to store the most recent value in the session, given that by navigating only within a single `live_session`, we won’t have HTTP requests?

We do so by triggering a JS hook, which then performs an XHR request to store the actual data, an idea I copied from [FullstackPhoenix](https://fullstackphoenix.com/tutorials/set-session-values-from-liveview).

We first implement a controller with the sole purpose of adding data to the session, given it is within a list of allowed keys:

```elixir
elixirdefmodule ControlManiacWeb.StoreSessionController do
  use ControlManiacWeb, :controller

  @allowed_keys ~w(current_division_id)a

  def create(conn, params) do
    updated_conn =
      Enum.reduce(params, conn, fn {key, value}, acc_conn ->
        if String.to_atom(key) in @allowed_keys do
          put_session(acc_conn, String.to_atom(key), value)
        else
          acc_conn
        end
      end)

    send_resp(updated_conn, 200, "")
  end
end
```

```html
html<div id="store-session" phx-hook="StoreSession"></div>
```

```js
js// store_session.js

export const StoreSession = {
  mounted() {
    const token = document.querySelector('meta[name="csrf-token"]').getAttribute('content')

    this.handleEvent('store_session', data => {
      fetch('/store_session', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
          'X-CSRF-Token': token
        },
        body: JSON.stringify(data)
      })
    })
  }
}

// And don't forger to register the hook in app.js

import { StoreSession } from "./store_session"

let liveSocket = new LiveSocket("/live", Socket, {
  ...
  hooks: {
    StoreSession: StoreSession
  }
})
```

```elixir
elixirdef handle_event("switch-division", %{"id" => id}, socket) do
DivisionSelector.update_division(
    socket.assigns.current_user,
    String.to_integer(id)
  )

  socket =
    socket
    |> push_event("store_session", DivisionSelector.map_for_session(id))
    |> assign(:current_division, id)

  {:noreply, socket}
end
```

```elixir
elixirdef update_division(%User{id: user_id} = user, division_id) do
  SettingsCache.insert({@cache_division_key, user_id}, division_id)
end

def map_for_session(id) do
  %{@session_division_key => id}
end
```

In the real application, we also used `Phoenix.PubSub` to notify current views about the division change, which might be a topic for another blog post some time.

## Conclusion

Given that this is what I thought to be a very simple problem, I was quite surprised that I was not able to find an easier solution. But, on the other hand, handling global state is hard, no matter the framework, and at least within Elixir Phoenix, we are able to implement a solution in a very explicit and controllable way.
