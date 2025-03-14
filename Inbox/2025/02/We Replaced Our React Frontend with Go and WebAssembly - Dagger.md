---
created: 2025-02-19T16:10:02 (UTC -03:00)
tags: []
source: https://dagger.io/blog/replaced-react-with-go
author: 
---

# We Replaced Our React Frontend with Go and WebAssembly - Dagger

> ## Excerpt
> Powerful, programmable CI/CD engine that runs your pipelines in
containers — pre-push on your local machine and/or post-push in CI

---
A few weeks ago, we [launched Dagger Cloud v3](https://dagger.io/blog/dagger-cloud-v3), a completely new user interface for [Dagger Cloud](https://dagger.cloud/). One of the main differences between v3 and its v2 predecessor is that the new UI is written in [WebAssembly (WASM)](https://en.wikipedia.org/wiki/WebAssembly) using Go. At first glance, this might seem an odd choice - Go typically isn't the first language you think of when deciding to program a Web UI - but we had good reasons. In this blog post, I'll explain why we chose WebAssembly, some of our implementation challenges (and how we worked around them), and the results.

#### Two Codebases = More Work, Fewer Features

Dagger works by building up a DAG of operations and evaluating them, often in parallel. By nature, this is a difficult thing to display. To help users make sense of it, we offer [two real-time visualization interfaces](https://docs.dagger.io/features/visualization): the Dagger terminal UI (TUI), included in the Dagger CLI, and Dagger Cloud, an online Web dashboard. The Dagger TUI is implemented in Go, and Dagger Cloud (pre-v3) was written in React.

Obviously, we want both user interfaces to be as close to each other as possible. But the actual act of interpreting Dagger's event stream in real-time and producing a UI is pretty involved. Some of the more complex event streams we've seen have hundreds of thousands of OpenTelemetry spans, and managing the data structures around them gets very complicated, very quickly. The Web UI often couldn't keep up with the huge volume of data it had to process and it would become laggy and slow; to fix this performance bottleneck, we were forced into a different implementation model for the React application.

So, we ended up with two interfaces trying to accomplish the same thing, one of them in one language and ecosystem (TypeScript/React), the other in a totally different language and ecosystem (Go), and we couldn't easily share business logic between them. As a small team, we need to ship fast. Having to re-implement every feature twice was just a massive tax on our velocity.

We started thinking about a new approach to Dagger Cloud, with two main goals:

-   Unify the codebases, to eliminate duplication and make it more efficient to ship new features
    
-   Deliver on the promise of a crisp, snappy Web UI, matching the speed and performance of the terminal UI
    

#### Choosing Go + WebAssembly

Our starting goal was to be able to reuse one codebase for both Dagger Cloud and the TUI. We decided fairly early to make it a Go codebase. Technically, we could have gone the other way and used TypeScript for the TUI. But we're primarily a team of Go engineers, so selecting Go made it easier for others in the team to contribute, to add a feature or drop in for a few hours to help debug an issue. In addition to standardizing on a single language, it gave us flexibility and broke down silos in our team.

Once we decided to run Go code directly in the browser, WebAssembly was the logical next step. But there were still a couple of challenges:

-   The Go + WebAssembly combination is still not as mature as React and other JavaScript frameworks. There are no ready-made component libraries to pull from, the developer tooling isn't as rich, and so on. We knew that we would need to build most of our UI components from scratch.
    
-   There is a hard 2 GB memory limit for WebAssembly applications in most browsers. We expected this to be a problem when viewing large traces, and we knew we would have to do a lot of optimization to minimize memory usage and keep the UI stable. This wasn't entirely bad though; the silver lining here was that any memory usage improvements made to the WebAssembly UI would also benefit TUI users, since it was now a shared codebase.
    

#### De-Risking the Project

Once we'd made the decision, the next question was, "how do we build this?" We decided to build the new WebAssembly-based UI in the [Go-app framework](https://go-app.dev/). Go-app is a high-level framework specifically for [Progressive Web Apps (PWAs)](https://en.wikipedia.org/wiki/Progressive_web_app) in WebAssembly. It offers key Go benefits, like fast compilation and native static typing, and it also follows a component-based UI model, like React, which made the transition easier.

Since the Go + WebAssembly combination isn't mainstream, there was some healthy skepticism within the Dagger team about its feasibility. For example, there was no real ecosystem for Go-app UI components and we knew we’d have to write our own, but we weren’t sure how easy or difficult this would be. We also had concerns over integrations with other services (Tailwind, Auth0, Intercom, PostHog), and about rendering many hundreds of live-updating components at the same time. 

To answer these questions and de-risk the project, I spent almost a month prototyping, with the goal of re-implementing as much of the existing UI as possible in Go-app. As it turned out, there weren't many blockers: WebAssembly is already a [well-documented open standard](https://webassembly.org/specs/) and most other questions were answered in [Go-app’s own documentation](https://go-app.dev/reference). The biggest challenge, as expected, was the memory usage limit, which required careful design and optimization.

#### From Prototype to Production

Once we had a working proof of concept, the team's comfort level increased significantly and we kicked off project "awesome wasm" to deliver a production implementation. Here are a few notes from the journey:

-   Memory usage was easily the most existential threat to the project’s success. I spent a lot of time figuring out how to render 200k+ lines of log output without crashing. This led to optimizations deep in our [virtual terminal rendering library](https://github.com/vito/midterm), which dramatically reduced TUI memory usage at the same time (as mentioned already, sharing codebases means that important optimizations in one interface become "free" in the other!)
    
-   Go WASM is slow at parsing large amounts of JSON, which led to dramatic architecture changes and the creation of a “smart backend” for incremental data loading over WebSockets, using Go's rarely-used [encoding/gob format](https://pkg.go.dev/encoding/gob).
    
-   Initially, the WASM file was around 32 MB. By applying [Brotli compression](https://github.com/google/brotli), we were able to bring it down to around 4.6 MB. We tried to perform Brotli compression on-the-fly in our CDN but the file was too large, so eventually we just included the compression step into our build process.
    
-   Apart from the memory challenges, most of our other initial worries turned out unfounded. The UI components weren’t very hard to write, integrations with other services were straightforward, and I found good techniques for handling component updates in real-time.
    
-   There were a number of useful NPM packages I found, so I wondered if I could use them with Go. WebAssembly has a straightforward interface to both Go and JavaScript, so I built a [Dagger module that uses Browserify to load an NPM package](https://daggerverse.dev/mod/github.com/vito/daggerverse/browserify@d368836636284116d090e271742904fea369cf72). This module allows us to generate a JavaScript file that can be included in a Go application. This means that we can work primarily in Go and then, if needed, we have a way to load helpers that are implemented in native JavaScript.
    
-   Disclaimer: I'm not a React professional so with that in mind...it seemed to me that React had a very rigid way of implementing components, while Go-app was much more flexible. In Go-app, you can have any component update whenever you like, which gives you many more degrees of freedom for optimization. For example, I needed to optimize a component rendering 150,000+ lines of output. Just having the ability to try different approaches and then pick the one that worked best, made the entire exercise much easier!
    
-   Even though Go-app doesn't have React-like developer tools built into the browser, I was able to use Go's own tools (pprof) plus the default profiler built into the browser for profiling and debugging. This was very useful to inspect functions calls, track CPU and memory usage, and evaluate the effectiveness of different approaches for optimizing memory usage.
    
-   I discovered a side benefit of using Go-app: since Dagger Cloud is built as a PWA, it can be installed as a desktop or a mobile application. This makes it possible to launch Dagger Cloud like a native application and get a full-screen experience without needing to open a browser first, or just have a dedicated icon in your desktop taskbar/dock.
    

We soft-launched Dagger Cloud v3 to our [Dagger Commanders](https://dagger.io/commanders) a few weeks ago to collect feedback and made it available to everyone shortly thereafter.

#### Benefits

Our switch from React to WASM has resulted in a more consistent user experience across all Dagger interfaces, and better overall performance and lower memory usage, especially when rendering large and complex traces.

From an engineering perspective too, the benefits to our team are significant. Optimizations very often involve just as much, if not more, work than actually implementing features. So it's great to not have to spend time optimizing the Web UI, and then more time optimizing the TUI, and instead actually focus on delivering new features.

#### Should You Do This?

Dagger Cloud v3 has the Dagger community buzzing and one of the more common questions we've been fielding recently is: who should consider doing this and who shouldn't?

We want to be clear that we're not generally recommending making front-ends in Go. We had some very good reasons to do it: a team of strong Go engineers; a complex UI that TypeScript/React didn't scale well for; a requirement for standardization and reuse between two codebases; and a company-wide mandate to increase our velocity. That's a fairly specific set of circumstances. If you're in similar circumstances, this is certainly an option worth evaluating; if not, there are other tools and standards that you should consider first.

Dagger Cloud v3 is still in beta and we're excited for you to [try it out](https://v3.dagger.cloud/). If you'd like to know more about our implementation or simply have feedback to share on the new UI, join our Discord and [let us know](https://discord.com/invite/dagger-io) what you think!
