![](https://andrewtimberlake.com/images/blog/test_attributes.jpg)

When a LiveView application begins to get complicated, testing it can become brittle. When we use CSS selectors to find the elements we want to assert on, we are tightly coupling our tests to the HTML structure of our LiveView.  
As the HTML structure changes, our tests will break.

A test example from the Phoenix LiveView docs looks like this:

```elixir
{:ok, view, html} = live(conn, "/users") html = view |> element("#user-13 a", "Delete") |> render_click() refute html =~ "user-13" refute view |> element("#user-13") |> has_element?()
```

This makes some assumptions about the HTML markup. There is an element with `id="user-13"` and an anchor tag with the text `Delete`.  
If you decide to change the HTML to use a button instead of an anchor tag, your test will break.  
Chances are that the id is being used here only for the test and again, if it’s removed, the test will break. The same will happen if you change the word "Delete" to a trash-can icon.

To make our tests more robust, we can use test attributes to make our tests more flexible, but we don’t want test-specific attributes in our production HTML. (I got this idea from [ember-test-selectors](https://github.com/mainmatter/ember-test-selectors).)

## data-test- attributes

If we use attributes in our markup that are prefixed with `data-test-`, we have a way of identifying the elements we want in our tests and decoupling our tests from the HTML structure.

I added a [pull request](https://github.com/phoenixframework/phoenix_live_view/pull/3649) to Phoenix LiveView to have these attributes stripped out in production, but José showed that using a macro that sets attribute values to `nil` in production works without run time overhead and without hiding the removal buried in a configuration file.

Building on his suggestion, I did some more researdh and found that you can add dynamic attributes using `{ ... }` within an HTML tag. I have added a simple macro to my CoreComponents module that generates `data-test-` attributes.

Here’s the macro I have in my CoreComponents module:

```elixir
defmodule MyAppWeb.CoreComponents do if Mix.env() == :prod do defmacro test_attrs(_attrs), do: [] else @doc """ Generates data-test- attributes useful for identifying elements in tests without relying on markup. In production the macro is a no-op and no attributes are generated. Because it is a macro, there is no runtime cost. ## Examples <p {test_attrs(foo: "bar")}>Hello</p> becomes <p data-test-foo="bar">Hello</p> <p {test_attrs([:foo, bar: "baz"])}>Hello</p> becomes <p data-test-foo data-test-bar="baz">Hello</p> <p {test_attrs(foo: true)}>Hello</p> becomes <p data-test-foo>Hello</p> """ defmacro test_attrs(attrs) do attrs |> List.wrap() |> Enum.map(fn {k, v} -> {to_test_attribute(k), v} k -> {to_test_attribute(k), true} end) end defp to_test_attribute(attr) do String.to_atom( "data-test-" <> (attr |> to_string() |> String.replace("_", "-")) ) end end end
```

You could change the above macro condition to `Mix.env() != :test`, but I find it helpful to be able to see the data-test- attributes in the HTML in development as well, which helps me with building out my tests.  
You could also use a config flag to toggle the macro in whichever environment you need.

With the following example HEEX markup:  
(I’m adding two deletion related attributes to show the test attributes with and without values.)

```html
<div :for={user <- @users} {test_attrs(user: user.id)}> {user.name} <a href="/users/13" {test_attrs([:delete, action: "delete"])} phx-click="delete" phx-value="13">Delete</a> </div>
```

produces the following HTML:

```html
<div data-test-user="13"> User 13 <a href="/users/13" data-test-delete data-test-action="delete" phx-click="delete" phx-value="13">Delete</a> </div>
```

We can test it with the following:

```elixir
user = insert_user() {:ok, view, html} = live(conn, "/users") assert view |> element("[data-test-user=#{user.id}]") |> has_element?() html = view |> element("[data-test-user=#{user.id}][data-test-delete]") |> render_click() # or html = view |> element("[data-test-user=#{user.id}][data-test-action=delete]") |> render_click() refute view |> element("[data-test-user=#{user.id}]") |> has_element?()
```

If we change our markup to use a table, a button, and an icon, the test will still pass.

```html
<table> <tr :for={user <- @users} {test_attrs(user: user.id)}> <td>{user.name}</td> <td> <button {test_attrs([:delete, action: "delete"])} phx-click="delete" phx-value="13"> <.icon name="trash-can" /> </button> </td> </tr> </table>
```

This will produce the following HTML (and yet our tests remain unchanged):

```html
<table> <tr data-test-user="13"> <td>User 13</td> <td> <button data-test-delete data-test-action="delete" phx-click="delete" phx-value="13"> <.icon name="t rash-can" /> </button> </td> </tr> </table>
```

30 Jan 2025

I’m [available for hire](https://andrewtimberlake.com/hire), whether you need a little help working through a particular problem, or help with a larger project.