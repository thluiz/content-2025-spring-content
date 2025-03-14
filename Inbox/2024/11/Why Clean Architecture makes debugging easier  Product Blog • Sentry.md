---
created: 2024-09-26T10:45:22 (UTC -03:00)
tags: []
source: https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev
author: 
---

# Why Clean Architecture makes debugging easier | Product Blog • Sentry

> ## Excerpt
> With Clean Architecture, projects become testable, predictable & easier to debug. Learn how Clean Architecture can make your next project more manageable here.

---
![Why Clean Architecture makes debugging easier](https://images.ctfassets.net/em6l9zw4tzag/6VrjPVFuDC9G1T8XFZnYX8/82e6aea53cbf700798b523311babd5d6/0924_DTSD-961-debuggability-bugzapper-hero.jpg?w=2520&h=945&fl=progressive&q=50&fm=jpg)

Let’s start with things we already know - complex projects are inherently hard to debug. The more complicated they are, the harder it is to debug them. The size of the project naturally defines complexity’s lower bounds, but even the smallest projects can become unnecessarily complex and messy if you don’t pay attention to how you structure them. Though we can’t eliminate complexity, we can manage it effectively with the right approach.

Clean Architecture is one of those approaches. It’s a set of rules that organize code in a way that reduces unnecessary coupling, makes your codebase testable, and most importantly, in this case - predictable. While it doesn’t remove complexity, it makes it more manageable. In this article, we’ll take a look at what Clean Architecture is and how it makes debugging easier.

## [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#what-is-clean-architecture)What is Clean Architecture?

We’re not going to do a full 101 in this article, just provide a brief explanation. Clean Architecture is an implementation of previous ideas like [Hexagonal Architecture](https://alistair.cockburn.us/hexagonal-architecture/), [Onion Architecture](http://jeffreypalermo.com/blog/the-onion-architecture-part-1/), and [Screaming Architecture](https://blog.cleancoder.com/uncle-bob/2011/09/30/Screaming-Architecture.html). It aims to make projects testable, independent of UI, framework, database, and external dependencies. It achieves this through “layers”:

-   **Frameworks & Drivers Layer:** keeps all the UI framework functionality, plus any additional “consumers” of the core app logic (e.g. API handlers and webhooks).
-   **Interface Adapters Layer:** defines Controllers (that orchestrate use cases) and Presenters (that convert controllers’ results into UI-friendly data shape).
-   **Application Layer:** defines the business logic of the app. This layer contains the main functionality of the app. It keeps “use cases” - functions that define individual operations like “createTodo” or “updateUser”. Aside from use cases, it also keeps interfaces that define the signature of the Services and Repositories.
-   **Entities Layer:** keeps your model definitions (business rules), custom errors, and any shape of data that are being used across the app.
-   **Infrastructure Layer:** pulls in the interfaces from the Application layer and implements them in Services and Repositories. This layer also implements libraries like database drivers, SDKs, utility libraries, and authentication libraries.

The goal of the layering is to isolate all third-party frameworks and libraries into the topmost layers so that the application core does not depend on them. Each layer can only depend on the layer below it, but not above it. This blog post from The Clean Code Blog shows the original diagram, but I actually drew it from a different perspective that might be easier to understand:

![](https://images.ctfassets.net/em6l9zw4tzag/20bSR045HkxRrPPJwv8abb/580d5e53964cfa69e8ab5d8c5607f8a3/clean-architecture-alternative-diagram.png "clean-architecture-alternative-diagram")

To learn more about Clean Architecture, you can check out [my YouTube video](https://www.youtube.com/watch?v=jJVAla0dWJo) where I explain how to implement Clean Architecture in a Next.js project.

## [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#does-clean-architecture-make-debugging-easier)Does Clean Architecture make debugging easier?

Since Clean Architecture dictates how to structure your code, all of the operations in your app will look like this:

1.  _Web_: A “consumer” (e.g. API endpoint, server action, RPC handler, webhook) will invoke a specific _controller_
2.  _Controller_: The _controller_ will perform _authentication_ and _input validation_ checks and throw specific errors accordingly. If there are no errors, it will invoke a number of use cases that define the individual operations
3.  _Application_: The use cases will perform _authorization_ checks and throw specific errors if needed. If no there are no errors, it will invoke a number of _repositories_ that interact with various data sources (e.g. database, external APIs) and modify the persisted data
4.  _Infrastructure_: The _repository_ will use the data source library (e.g. database driver, SDK) to fulfill the data request and throw specific errors if it fails to do so

![](https://images.ctfassets.net/em6l9zw4tzag/cbzwpLvRRNJI8yJSy1kNn/4b40df38e82c044e501bf156306a21da/create-link-flow.png "create-link-flow")

Because of this, debugging is easier from two aspects: consistent traces, and layer-specific errors.

### [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#consistent-traces)Consistent traces

If we were to apply [tracing](https://docs.sentry.io/concepts/key-terms/tracing/distributed-tracing/?ref=dailydev) to our Clean Architecture project and check out its trace, we’ll see this:

![](https://images.ctfassets.net/em6l9zw4tzag/2mqRSFpMjh6p8Bekzf3Py1/38a7fc27033936a72d75e0ac3df3cc78/clean-architecture-trace-view.png "clean-architecture-trace-view")

Each operation will produce a very similar trace to this one, with the only difference being which controller, use cases, services, and repositories are being invoked. But it’s always going to be the controller at the top, with services and use cases inside, followed by repositories at the third level.

Consistent traces are easier to scan when viewing, and identifying performance bottlenecks is nearly instantaneous once you get used to the traces’ common structure.

### [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#layer-specific-errors)Layer-specific errors

Because each layer throws its own specific set of errors, figuring out where the error originates from is very obvious. If an “InputParseError” was thrown, it must be thrown by a controller because controllers handle input validation. If an “UnauthorizedError” was thrown, it must be thrown by a use case because use cases handle authorization checks.

![](https://images.ctfassets.net/em6l9zw4tzag/2yMg3ItyKJAR52cbIGhlo/782072d831a46c0b13165652e01c1f28/layer-specific-errors.png "layer-specific-errors")

The screenshot above shows four errors that are being thrown by different controllers and use cases. Clicking into one of them will give us additional information like the [stack trace](https://sentry.io/features/stacktrace/?ref=dailydev), the suspect commit, the environment, and any tags set by us that help with triaging and debugging:

![](https://images.ctfassets.net/em6l9zw4tzag/JkFO5LRDnxQ75laLV6FwL/10c0ccac0f96b418e53c5dd8ea26dd63/input-parse-error-details.png "input-parse-error-details")

Just by the type of the error (InputParseError) we know that it comes from a controller. From the stack trace we can see that it comes from the `create-todo-controller.ts` file specifically. From this point, we can check other info such as tags and the suspect commit, or we can navigate to see the full trace when this specific error happened. In this case, if we have unit tests covering the core app logic, then the cause of the error is most likely in the frontend part.

## [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#how-to-prevent-bugs-in-clean-architecture-applications)How to prevent bugs in Clean Architecture applications

One of the main reasons why projects adopt Clean Architecture (and other similar architectures) is to achieve testability. Isolating third-party dependencies makes your core app’s logic easy to test, and each layer having unique responsibilities makes writing unit tests straightforward. Defining custom errors in the Entities layer and using them in the app’s core logic also makes error handling straightforward.

![](https://images.ctfassets.net/em6l9zw4tzag/3I9qbY5zRxtYKIGItcOY9b/f4eb29fa59fc829d6ad1493deb150702/unit-tests.png "unit-tests")

Since we know that use cases handle authorization checks, we’re good to only test for unauthorized invocations and also the main scenario - it does what it’s supposed to do. Controllers on the other hand perform authentication checks, input validation, and invoke multiple use cases, so aside from the main scenario we’ll also test that they throw specific errors when we pass invalid input, and also invoke them without an authentication session. We can cover our services and repositories with unit tests (if possible), or mock them.

If our unit tests make sure that the main scenario works, and that appropriate errors are being thrown in specific scenarios, then the chances of creating bugs are significantly reduced compared to a more lax coding approach.

Maintaining discipline reduces opportunities for bugs to be created.

With [Codecov](https://about.codecov.io/), you can keep track of your tests’ coverage, so you know if all conditions and lines are properly tested. [Code coverage](https://sentry.io/for/code-coverage/?ref=dailydev) tells you when you don’t have a unit test that tests a specific `throw` statement, or that tests conditional logic that depends on a value of a certain argument. That’s how a decent amount of issues are caused. Here’s a screenshot of Codecov that shows 100% coverage on my [nextjs-clean-architecture](https://app.codecov.io/gh/nikolovlazar/nextjs-clean-architecture/tree/main/src) project (follow the link if you want to click around):

![](https://images.ctfassets.net/em6l9zw4tzag/1uCFCF5OWW4gDkc6CpyIze/21797d943af9292f6084593ca16a6e1a/codecov.png "codecov")

With a 100% coverage of my core app logic code, and my custom errors thoughtfully placed in specific layers, I’m confident that my project’s features will continue working just fine, and also that the app will react accordingly in cases where there’s something wrong.

## [](https://blog.sentry.io/why-clean-architecture-makes-debugging-easier/?ref=dailydev#make-code-more-predictable-with-clean-architecture)Make code more predictable with Clean Architecture

Debugging complex projects is never easy, but Clean Architecture brings a level of organization and discipline that can turn a daunting task into a manageable one. By enforcing a clear separation of concerns across the different layers, Clean Architecture not only reduces unnecessary coupling, but also makes your codebase more predictable and easier to test.

The separation of concerns aspect makes us create well defined boundaries that become natural points to add tracing. This also lets you do a broader analysis across your entire app when debugging issues, for example, you can look for N+1 and other systematic problems caused by business logic code, or find missing database indexes in repositories just by looking for slow spans.

The beauty of this architecture (and any other architecture really) lies in its consistency. When errors occur, you can quickly identify their source based on the type of the error thrown itself, whether it’s input validation error in the controller, or an authorization check in a use case. This makes the process of diagnosing issues much more straightforward and less time-consuming.

It doesn’t just make debugging easier, it also prevents those bugs from cropping up in the first place.

Clean Architecture’s emphasis on testability makes sure that every piece of your core logic can be covered by tests, reducing the risk of unexpected issues. With tools like [Sentry](https://sentry.io/welcome/?ref=dailydev) for detailed tracing and Codecov for comprehensive test coverage, you’re not just fixing bugs - you’re proactively making sure that they don’t happen.

So in the end, while we can’t eliminate the inherent complexity of software development, we can use the tools that Clean Architecture gives us to manage it effectively. It turns debugging from a frantic search into a methodical process, allowing you to focus more on building great features and less on chasing down elusive bugs.
