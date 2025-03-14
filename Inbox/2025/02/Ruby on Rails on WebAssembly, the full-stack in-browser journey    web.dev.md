Published: January 31, 2025

Imagine running a fully functional blog in your browser—not just the frontend, but the backend, too. No servers or clouds involved—just you, your browser, and… [WebAssembly](https://webassembly.org/)! By allowing server-side frameworks to run locally, WebAssembly is blurring the boundaries of classic web development and opening up exciting new possibilities. In this post, Vladimir Dementyev (Head of Backend at [Evil Martians](https://evilmartians.com/)) shares the progress on making [Ruby on Rails](https://rubyonrails.org/) Wasm- and browser-ready:

-   How to bring Rails into the browser in 15 minutes.
-   Behind the scenes of Rails wasmification.
-   Future of Rails and Wasm.

## Ruby on Rails' famous "blog in 15 minutes" now running right in your browser

Ruby on Rails is a web framework focused on developer productivity and shipping things fast. It's the technology used by industry leaders such as [GitHub](https://github.blog/engineering/architecture-optimization/building-github-with-ruby-and-rails/) and [Shopify](https://shopify.engineering/shopify-made-patterns-in-our-rails-apps). The popularity of the framework began many years ago with the release of the famous ["How to build a blog in 15 minutes"](https://www.youtube.com/watch?v=Gzj723LkRJY) video published by David Heinemeier Hansson (or DHH). Back in 2005, it was unimaginable to build a fully working web application in such a short time. It felt like _magic_!

Today, I'd like to bring this magical feeling back by creating a Rails application that runs fully in your browser. Your journey starts with creating a basic Rails application the usual way, and then packaging it for Wasm.

## Background: a "blog in 15 minutes" on the command line

Assuming you have [Ruby and Ruby on Rails installed on your machine](https://guides.rubyonrails.org/v5.0/getting_started.html#installing-rails), you start with creating a new Ruby on Rails application and scaffolding some functionality (just like in the original "blog in 15 minutes" video):

```

$<span> </span>rails<span> </span>new<span> </span>--css<span>=</span>tailwind<span> </span>web_dev_blog

<span>  </span>create<span>  </span>.ruby-version
<span>  </span>...

$<span> </span><span>cd</span><span> </span>web_dev_blog

$<span> </span>bin/rails<span> </span>generate<span> </span>scaffold<span> </span>Post<span> </span>title:string<span> </span>date:date<span> </span>body:text

<span>  </span>create<span>    </span>db/migrate/20241217183624_create_posts.rb
<span>  </span>create<span>    </span>app/models/post.rb
<span>  </span>...

$<span> </span>bin/rails<span> </span>db:migrate

<span>==</span><span> </span><span>20241217183624</span><span> </span>CreatePosts:<span> </span><span>migrating</span><span> </span><span>====================</span>
--<span> </span>create_table<span>(</span>:posts<span>)</span>
<span>   </span>-&gt;<span> </span><span>0</span>.0017s
<span>==</span><span> </span><span>20241217183624</span><span> </span>CreatePosts:<span> </span>migrated<span> </span><span>(</span><span>0</span>.0018s<span>)</span><span> </span><span>===========</span>
```

Without even touching the codebase, you can now run the application and see it in action:

```
$<span> </span>bin/dev

<span>=</span>&gt;<span> </span>Booting<span> </span><span>Puma</span>
<span>=</span>&gt;<span> </span>Rails<span> </span><span>8</span>.0.1<span> </span>application<span> </span>starting<span> </span><span>in</span><span> </span>development
...
*<span> </span>Listening<span> </span>on<span> </span>http://127.0.0.1:3000
```

Now, you can open _your_ blog at [http://localhost:3000/posts](http://localhost:3000/posts) and start writing posts!

![A Ruby on Rails blog launched from the command line running in the browser.](https://web.dev/static/blog/ruby-on-rails-on-webassembly/webdevrubyonra--rv7ehku4d.png)

You have a very bare-bone, but functional blog application built in minutes. It's a full-stack, _server-controlled_ application: you have a database ([SQLite](https://www.sqlite.org/)) to keep your data, a web server to handle HTTP requests ([Puma](https://puma.io/)), and a Ruby program to keep your business logic, provide UI, and process user interactions. Finally, there is a thin layer of JavaScript ([Turbo](https://turbo.hotwired.dev/)) to streamline the browsing experience.

The official Rails demo continues in the direction of deploying this application onto a bare metal server and, thus, making it production-ready. Your journey will continue in the opposite direction: instead of putting your application somewhere far away, you'll "deploy" it locally.

## Next level: a "blog in 15 minutes" in Wasm

Since the addition of WebAssembly, browsers became capable of running not only JavaScript code, but any code compilable into Wasm. And Ruby is not an exception. Surely, Rails is more than Ruby, but before digging into the differences, let us continue the demo and _wasmify_ (a verb coined by the [wasmify-rails](https://github.com/palkan/wasmify-rails) library) the Rails application!

You only need to execute a few commands to compile your blog application into a Wasm module and run it in the browser.

First, you install the wasmify-rails library using Bundler (the `npm` of Ruby) and run its generator using the Rails CLI:

```
$<span> </span>bundle<span> </span>add<span> </span>wasmify-rails

$<span> </span>bin/rails<span> </span>wasmify:install

<span>  </span>create<span>  </span>config/wasmify.yml
<span>  </span>create<span>  </span>config/environments/wasm.rb
<span>  </span>...
<span>  </span>info<span>  </span>✅<span> </span>The<span> </span>application<span> </span>is<span> </span>prepared<span> </span><span>for</span><span> </span>Wasm-ificaiton!
```

The `wasmify:rails` command configures a dedicated "wasm" [execution environment](https://docs.ruby-lang.org/en/3.3/Process.html#module-Process-label-Execution+Environment) (in addition to the default "development", "test", and "production" environments) and installs the required dependencies. For a greenfield Rails application, this is enough to make it Wasm-ready.

Next, build the core Wasm module containing the Ruby runtime, the standard library, and all the application dependencies:

```
$<span> </span>bin/rails<span> </span>wasmify:build

<span>==</span>&gt;<span> </span>RubyWasm::BuildSource<span>(</span><span>3</span>.3<span>)</span><span> </span>--<span> </span>Building
...
<span>==</span>&gt;<span> </span>RubyWasm::CrossRubyProduct<span>(</span>ruby-3.3-wasm32-unknown-wasip1-full-4aaed4fbda7afe0bdf4e22167afd101e<span>)</span><span> </span>--<span> </span><span>done</span><span> </span><span>in</span><span> </span><span>47</span>.37s
INFO:<span> </span>Packaging<span> </span>gem:<span> </span>rake-13.2.1
...
INFO:<span> </span>Packaging<span> </span>gem:<span> </span>wasmify-rails-0.2.0
INFO:<span> </span>Packaging<span> </span>setup.rb:<span> </span>bundle/setup.rb
INFO:<span> </span>Size:<span> </span><span>73</span>.77<span> </span>MB
```

This step can take some time: you must build Ruby from source to properly link native extensions (written in C) from the third-party libraries. This (temporary) drawback is covered later in the post.

The compiled Wasm module is just a _foundation_ for your application. You must also _pack_ the application code itself and all the _assets_ (for example, images, CSS, JavaScript). Before doing the packing, create a basic launcher application that could be used to run the _wasmified_ Rails in the browser. For that, there's also a generator command:

```
$<span> </span>bin/rails<span> </span>wasmify:pwa

<span>  </span>create<span>  </span>pwa
<span>  </span>create<span>  </span>pwa/boot.html
<span>  </span>create<span>  </span>pwa/boot.js
<span>  </span>...
<span>  </span>prepend<span>  </span>config/wasmify.yml
```

The previous command generates a minimal PWA application built with [Vite](https://vite.dev/) that can be used locally to test the compiled Rails Wasm module or be deployed statically to distribute the app.

Now, with the launcher, all you need is to pack the whole application into a single Wasm binary:

```
$<span> </span>bin/rails<span> </span>wasmify:pack
...
Packed<span> </span>the<span> </span>application<span> </span>to<span> </span>pwa/app.wasm
Size:<span> </span><span>76</span>.2<span> </span>MB
```

That's it! Run the launcher app and see your Rails blogging application running fully within the browser:

```
$<span> </span><span>cd</span><span> </span>pwa/

$<span> </span>yarn<span> </span>dev

<span>  </span>VITE<span> </span>v4.5.5<span>  </span>ready<span> </span><span>in</span><span> </span><span>290</span><span> </span>ms

<span>  </span>➜<span>  </span>Local:<span>   </span>http://localhost:5173/
```

Go to [http://localhost:5173](http://localhost:5173/), wait a bit for the "Launch" button to become active, and click it—enjoy working with the Rails app running locally in your browser!

![A Ruby on Rails blog launched from a browser tab running in another browser tab.](https://web.dev/static/blog/ruby-on-rails-on-webassembly/webdevrubyonra--zy63y1yawld.png)

Doesn't it feel like magic running a monolithic server-side application not just on your machine but within the browser sandbox? For me (even though I'm the "sorcerer"), it still looks like a fantasy. But there is no magic involved, only the progress of technology.

## Demo

You can experience the demo embedded in the article or launch the [demo in a standalone window](https://rails-blog-on-wasm.vladem.com/boot.html). Check out the [source code on GitHub](https://github.com/palkan/rails-15min-blog-on-wasm).

## Behind the scenes of Rails on Wasm

To better understand the challenges (and solutions) of packing a server-side application into a Wasm module, the rest of this post explains the components that are part of this architecture.

A web application depends on many more things than just a programming language used to write the application code. Each component must also be brought to your\_ local deployment environment\_—the browser. What's exciting about the "blog in 15 minutes" demo, is that this can be achieved without rewriting the application code. The same code was used to run the application in a classic, server-side mode and in the browser.

![The components that make up a Ruby on Rails app: a web server, a database, a queue, and storage. Plus the core Ruby components: the gems, native extensions, system tools, and the Ruby VM.](https://web.dev/static/blog/ruby-on-rails-on-webassembly/webdevrubyonra--ova3x5e0ifi.png)

A framework, like Ruby on Rails, gives you an interface, an abstraction to _communicate_ with infrastructure components. The following section discusses how you can employ the framework architecture to serve the somewhat esoteric local serving needs.

### The foundation: ruby.wasm

Ruby became officially [Wasm-ready in 2022](https://www.ruby-lang.org/en/news/2022/12/25/ruby-3-2-0-released/) (since version 3.2.0) meaning that the C source code could be compiled to Wasm and bring a Ruby VM anywhere you want. The [ruby.wasm](https://github.com/ruby/ruby.wasm) project ships precompiled modules and JavaScript bindings to run Ruby in the browser (or any other JavaScript runtime). The ruby:wasm project also comes with the build tools that lets You build a custom Ruby version with additional dependencies—this is very important for projects relying on libraries with C extensions. Yes, you can compile native extensions into Wasm, too! (Well, not any extension yet, but most of them).

Currently, Ruby fully supports the WebAssembly System Interface, [WASI 0.1](https://wasi.dev/interfaces#wasi-01). [WASI 0.2](https://wasi.dev/interfaces#wasi-02) which includes the [Component Model](https://component-model.bytecodealliance.org/) is already in the alpha state and a few steps from completion.Once WASI 0.2 is supported it will eliminate the current need of recompiling the whole language every time you need to add new native dependencies: they could be componentized.

As a side effect, the Component Model should also help with reducing the bundle size. You can learn more about the ruby.wasm development and progress from the [What you can do with Ruby on WebAssembly](https://speakerdeck.com/kateinoigakukun/what-you-can-do-with-ruby-on-webassembly) talk.

So, the Ruby part of the Wasm equation is solved. But Rails as a web framework needs all of the components shown in the previous diagram. Read on to learn how to put other components into the browser and link them together in Rails.

### Connect to a database running in the browser

SQLite3 comes with an official [Wasm distribution](https://sqlite.org/wasm/doc/trunk/index.md) and a corresponding [JavaScript wrapper](https://www.npmjs.com/package/@sqlite.org/sqlite-wasm), therefore is ready to be embedded in-browser. PostgreSQL for Wasm is available through the [PGlite](https://pglite.dev/) project. Therefore, you only need to figure out how to connect to the in-browser database from the Rails on Wasm application.

A component, or _sub-framework_, of Rails responsible for data modeling and database interactions is called Active Record (yes, named after the [ORM design pattern](https://en.wikipedia.org/wiki/Active_record_pattern)). Active Record abstracts away the actual SQL-speaking database implementation from the application code through the database adapters. Out of the box, Rails gives you SQLite3, PostgreSQL, and MySQL adapters. However, they all assume connecting to _real_ databases available over the network. To overcome this, you can write your own adapters to connect to _local_, in-browser databases!

This is how SQLite3 Wasm and PGlite adapters implemented as a part of the Wasmify Rails project are created:

-   The adapter class inherits from the corresponding built-in adapter (for example, `class PGliteAdapter < PostgreSQLAdapter`), so you can re-use the actual query preparation and results parsing logic.
-   Instead of the low-level database connection, you use an **external interface** object that lives in the JavaScript runtime—a bridge between a Rails Wasm module and a database.

For example, here is the bridge implementation for SQLite3 Wasm:

```
<span>export</span><span> </span><span>function</span><span> </span><span>registerSQLiteWasmInterface</span><span>(</span><span>worker</span><span>,</span><span> </span><span>db</span><span>,</span><span> </span><span>opts</span><span> </span><span>=</span><span> </span><span>{})</span><span> </span><span>{</span>
<span>  </span><span>const</span><span> </span><span>name</span><span> </span><span>=</span><span> </span><span>opts</span><span>.</span><span>name</span><span> </span><span>||</span><span> </span><span>"sqliteForRails"</span><span>;</span>

<span>  </span><span>worker</span><span>[</span><span>name</span><span>]</span><span> </span><span>=</span><span> </span><span>{</span>
<span>    </span><span>exec</span><span>:</span><span> </span><span>function</span><span> </span><span>(</span><span>sql</span><span>)</span><span> </span><span>{</span>
<span>      </span><span>let</span><span> </span><span>cols</span><span> </span><span>=</span><span> </span><span>[];</span>
<span>      </span><span>let</span><span> </span><span>rows</span><span> </span><span>=</span><span> </span><span>db</span><span>.</span><span>exec</span><span>(</span><span>sql</span><span>,</span><span> </span><span>{</span><span> </span><span>columnNames</span><span>:</span><span> </span><span>cols</span><span>,</span><span> </span><span>returnValue</span><span>:</span><span> </span><span>"resultRows"</span><span> </span><span>});</span>

<span>      </span><span>return</span><span> </span><span>{</span>
<span>        </span><span>cols</span><span>,</span>
<span>        </span><span>rows</span><span>,</span>
<span>      </span><span>};</span>
<span>    </span><span>},</span>

<span>    </span><span>changes</span><span>:</span><span> </span><span>function</span><span> </span><span>()</span><span> </span><span>{</span>
<span>      </span><span>return</span><span> </span><span>db</span><span>.</span><span>changes</span><span>();</span>
<span>    </span><span>},</span>
<span>  </span><span>};</span>
<span>}</span>
```

From the application perspective, the shift from a _real_ database to an in-browser one is just a matter of configuration:

```
<span># config/database.yml</span>
<span>development</span><span>:</span>
<span>  </span><span>adapter</span><span>:</span><span> </span><span>sqlite3</span>

<span>production</span><span>:</span>
<span>  </span><span>adapter</span><span>:</span><span> </span><span>sqlite3</span>

<span>wasm</span><span>:</span>
<span>  </span><span>adapter</span><span>:</span><span> </span><span>sqlite3_wasm</span>
<span>  </span><span>js_interface</span><span>:</span><span> </span><span>"sqliteForRails"</span>
```

Working with a local database doesn't require a lot of effort. However, if data synchronization with some _central_ source of truth is required, then you may face a challenge of a higher level. This question is out of the scope of this post (hint: check out the [Rails on PGlite and ElectricSQL demo](https://github.com/palkan/rails-on-wasm-playground/pull/7)).

## Service worker as a web server

Another essential component of any web application is a web server. Users interact with web applications using HTTP requests. Thus, you need a way to route HTTP requests triggered by navigation or form submissions to your Wasm module. Luckily, the browser has an answer for that—**service workers**.

A service worker is a special kind of a Web Worker that acts as a proxy between the JavaScript application and the network. It can intercept requests and manipulate them, for example: serve cached data, redirect to other URLs or… to Wasm modules! Here is a sketch of a service working serving requests using a Rails application running in Wasm:

```
<span>// The vm variable holds a reference to the Wasm module with a</span>
<span>// Ruby VM initialized</span>
<span>let</span><span> </span><span>vm</span><span>;</span>
<span>// The db variable holds a reference to the in-browser</span>
<span>// database interface</span>
<span>let</span><span> </span><span>db</span><span>;</span>

<span>const</span><span> </span><span>initVM</span><span> </span><span>=</span><span> </span><span>async</span><span> </span><span>(</span><span>progress</span><span>,</span><span> </span><span>opts</span><span> </span><span>=</span><span> </span><span>{})</span><span> </span><span>=</span>&gt;<span> </span><span>{</span>
<span>  </span><span>if</span><span> </span><span>(</span><span>vm</span><span>)</span><span> </span><span>return</span><span> </span><span>vm</span><span>;</span>
<span>  </span><span>if</span><span> </span><span>(</span><span>!</span><span>db</span><span>)</span><span> </span><span>{</span>
<span>    </span><span>await</span><span> </span><span>initDB</span><span>(</span><span>progress</span><span>);</span>
<span>  </span><span>}</span>
<span>  </span><span>vm</span><span> </span><span>=</span><span> </span><span>await</span><span> </span><span>initRailsVM</span><span>(</span><span>"/app.wasm"</span><span>);</span>
<span>  </span><span>return</span><span> </span><span>vm</span><span>;</span>
<span>};</span>

<span>const</span><span> </span><span>rackHandler</span><span> </span><span>=</span><span> </span><span>new</span><span> </span><span>RackHandler</span><span>(</span><span>initVM</span><span>});</span>

<span>self</span><span>.</span><span>addEventListener</span><span>(</span><span>"fetch"</span><span>,</span><span> </span><span>(</span><span>event</span><span>)</span><span> </span><span>=</span>&gt;<span> </span><span>{</span>
<span>  </span><span>// ...</span>
<span>  </span><span>return</span><span> </span><span>event</span><span>.</span><span>respondWith</span><span>(</span>
<span>    </span><span>rackHandler</span><span>.</span><span>handle</span><span>(</span><span>event</span><span>.</span><span>request</span><span>)</span>
<span>  </span><span>);</span>
<span>});</span>
```

The "fetch" is triggered every time a request is made by the browser. You can obtain the request information (URL, HTTP headers, body) and construct your own request object.

Rails, like most Ruby web applications, relies on the [Rack interface](https://github.com/rack/rack) for working with HTTP requests. Rack interface describes the format of the request and response objects as well as the interface of the underlying HTTP handler (application). You can express these properties as follows:

```
<span>request</span><span> </span><span>=</span><span> </span><span>{</span>
<span>   </span><span>"REQUEST_METHOD"</span><span> </span><span>=</span>&gt;<span> </span><span>"GET"</span><span>,</span>
<span>   </span><span>"SCRIPT_NAME"</span><span>    </span><span>=</span>&gt;<span> </span><span>""</span><span>,</span>
<span>   </span><span>"SERVER_NAME"</span><span>  </span><span>=</span>&gt;<span> </span><span>"localhost"</span><span>,</span>
<span>   </span><span>"SERVER_PORT"</span><span> </span><span>=</span>&gt;<span> </span><span>"3000"</span><span>,</span>
<span>   </span><span>"PATH_INFO"</span><span>      </span><span>=</span>&gt;<span> </span><span>"/posts"</span>
<span>}</span>

<span>handler</span><span> </span><span>=</span><span> </span><span>proc</span><span> </span><span>do</span><span> </span><span>|</span><span>env</span><span>|</span>
<span>  </span><span>[</span>
<span>    </span><span>200</span><span>,</span>
<span>    </span><span>{</span><span>"Content-Type"</span><span> </span><span>=</span>&gt;<span> </span><span>"text/html"</span><span>},</span>
<span>    </span><span>[</span><span>"&lt;!doctype html&gt;&lt;html&gt;&lt;body&gt;Hello Web!&lt;/body&gt;&lt;/html&gt;"</span><span>]</span>
<span>  </span><span>]</span>
<span>end</span>

<span>handler</span><span>.</span><span>call</span><span>(</span><span>request</span><span>)</span><span> </span><span>#=&gt; [200, {...}, [...]]</span>
```

If you found the request format familiar, then you've probably worked with [CGI](https://en.wikipedia.org/wiki/Common_Gateway_Interface) back in the days.

The `RackHandler` JavaScript object is responsible for converting requests and responses between JavaScript and Ruby realms. Given that Rack is used by most Ruby web applications, the implementation becomes universal, not Rails-specific. The [actual implementation](https://github.com/palkan/wasmify-rails/blob/main/src/rack.js) is too long to post here though.

A service worker is one of the key integral points of an in-browser web application. It's not only an HTTP proxy, but also a caching layer and a _network switcher_ (that is, you can build a _local-first_ or _offline-capable_ application). This is also a component that can help you serve user-uploaded files.

### Keep file uploads in the browser

One of the first additional features to implement in your fresh blog application is likely to be support for file uploads, or more specifically, attaching images to posts. To achieve this, you need a way to store and serve files.

In Rails, the part of the framework responsible for dealing with file uploads is called [Active Storage](https://guides.rubyonrails.org/active_storage_overview.html). Active Storage gives developers abstractions and interfaces to work with files without thinking about the low-level storage mechanism. No matter where you store your files, on a hard drive or in the cloud, the application code stays unaware of it.

Similarly to Active Record, in order to support a custom storage mechanism, all you need is to implement a corresponding _storage service_ adapter. Where to store files in the browser?

The traditional option is to use a database. Yes, you can store files as blobs in the database, no additional infrastructure components required. And there is already a ready-made plugin for that in Rails, [Active Storage Database](https://github.com/WizardComputer/activestorage_database). However, serving files stored in a database through the Rails application running within WebAssembly is not ideal because it involves rounds of (de-)serialization that are not free.

A better and more browser-optimized solution would be to use File System APIs and process file uploads and server uploaded files directly from the service worker. A perfect candidate for such infrastructure is the [OPFS](https://developer.mozilla.org/docs/Web/API/File_System_API/Origin_private_file_system) (origin private file system), a very recent browser API that will definitely play an important role for the future in-browser applications.

## What Rails and Wasm can achieve together

I'm pretty sure you've been asking yourself this question as you started reading the article: why run a server-side framework in the browser? The idea of a framework or a library being server-side (or client-side) is just a label. Good code and, especially, a good abstraction works everywhere. Labels shouldn't stop you from exploring new possibilities and pushing boundaries of the framework (for example, Ruby on Rails) as well as the boundaries of the runtime (WebAssembly). Both could benefit from such unconventional use cases.

There are plenty of _conventional_, or practical, use cases, too.

First, bringing the framework to the browser opens enormous **learning and prototyping opportunities**. Imagine being able to play with libraries, plugins, and patterns right in your browser and together with other people. [Stackblitz](https://stackblitz.com/) made this possible for JavaScript frameworks. Another example is a [WordPress Playground](https://web.dev/articles/wordpress-playground) that made it possible to play with WordPress themes without leaving the web page. Wasm could enable something similar for Ruby and its ecosystem.

There's a special case of in-browser coding especially useful to open source developers—**triaging and debugging issues**. Again, StackBlitz made this a thing for JavaScript projects: you create a minimal reproduction script, point at the link in a GitHub Issue, and spare maintainers the time on reproducing your scenario. And, actually, it's already started happening in Ruby thanks to the [RunRuby.dev](https://runruby.dev/) project (here is an [example issue](https://github.com/palkan/action_policy-graphql/issues/53#issuecomment-2492483778) resolved with the in-browser reproduction).

Another use case is **offline-capable** (or offline-aware) **applications**. Offline-capable applications that usually work using the network, but when there is no connection, they stay usable. For example, an email client that lets you search through your inbox while offline. Or, a music library application with the "Store on device" capability, so your favourite music keeps beating even if there is no network connection. Both examples depend on the **data stored locally**, not just using a cache as with classic PWAs.

Finally, building local (or desktop) applications with Rails also makes sense, because the productivity the framework gives you doesn't depend on the runtime. Full-featured frameworks suit well for building personal data- and logic-heavy applications. And using Wasm as a portable distribution format is also a viable option.

It's just the beginning of this Rails on Wasm journey. You can learn more about the challenges and solutions in the [Ruby on Rails on WebAssembly](https://writebook-on-wasm.fly.dev/5/ruby-on-rails-on-webassembly/) ebook (which, by the way, is an offline-capable Rails application itself).