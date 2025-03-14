I was recently looking at the [Duende Software](https://duendesoftware.com/) codebase, and I kept seeing the same suggestion offered by the IDE tooling whenever I encountered a `ConcurrentDictionary`: **“Closure can be eliminated: method has overload to avoid closure creation.”**

While the suggestion appears in the tooling, there isn’t a quick fix action to apply the change. It left me scratching my head because there wasn’t an immediately obvious solution.

This post will define closures and explain their problems. We’ll also explain how to change your usage of `ConcurrentDictionary` to avoid closures altogether.

## [What Are Closures?](https://khalidabuhakmeh.com/the-curious-case-of-dotnet-concurrentdictionary-and-closures?_bhlid=99ae6ca4cac3757797f6645fb212311fb52211fa#what-are-closures)

If you’ve ever worked with an `Action`, `Func`, `delegate`, or LINQ, then you’ve likely encountered a closure. A closure is a language mechanism that allows you to treat a function with **free variables** as if it were an object instance you may pass, invoke, or use in another context from when you first created it. [Justin Etheredge](https://www.simplethread.com/c-closures-explained/) has a great article explaining closures in-depth, but it’s when you use a lambda with a state outside the current scope of the lambda.

Let’s create a closure by capturing a variable in a straightforward example.

In the code above, the compiler needs to capture the `name` parameter to ensure all future calls to our `hello` lambda can execute. Capture can cause issues that may be difficult to predict until you execute your code.

1.  Additional allocations are needed to capture a value and can have resource utilization implications.
2.  A value captured from outside the closure scope may be alterable if it is a reference type. Unintended state change can lead to unpredictable behavior.
3.  Long-lived references may lead to memory leaks in the long run.

To avoid capture, ensure all lambdas pass state required as arguments.

Now that we understand the basics, let’s examine the `ConcurrentDictionary` suggestion and see how we might fix it.

## [ConcurrentDictionary.GetOrAdd and Closure Creation](https://khalidabuhakmeh.com/the-curious-case-of-dotnet-concurrentdictionary-and-closures?_bhlid=99ae6ca4cac3757797f6645fb212311fb52211fa#concurrentdictionarygetoradd-and-closure-creation)

Let’s write a straightforward use of the `GetOrAdd` method on `ConcurrentDictionary` and see what the issue might be.

Looking at the code, what variable do you think is creating the unnecessary closure?

If you guessed `value`, then you would be correct!

How do we fix the closure since our tooling now suggests that there is a solution to this issue?

Well, let’s refactor and see how our code changes. I’ll add parameter prefixes to make clear what is happening.

So there is an overload on `ConcurrentDictionary.GetOrAdd` that takes three parameters:

1.  The key to our value.
2.  The lambda function responsible for creating our value when we do not find it.
3.  A singular factory argument. You must wrap multiple arguments in a container class.

Using this overload, we now avoid closures, reduce allocations, and avoid potentially nasty concurrency or memory leak issues.

If you’re using `ConcurrentDictionary` check to see if you’re using `GetOrAdd` and see if you’re using the more efficient overload. Thanks for reading and sharing my posts with friends and colleagues. Cheers.

![Khalid Abuhakmeh's Picture](https://khalidabuhakmeh.com/assets/images/authorimage.jpg)

## About Khalid Abuhakmeh

## Read Next

[![ASP.NET Core and Chunking HTTP Cookies](https://res.cloudinary.com/abuhakmeh/image/fetch/c_limit,f_auto,q_auto,w_500/https://khalidabuhakmeh.com/assets/images/posts/misc/aspnet-core-http-cookies-chunking.jpg)](https://khalidabuhakmeh.com/aspnet-core-and-chunking-http-cookies)

[![Strongly-Typed Markdown for ASP.NET Core Content Apps](https://res.cloudinary.com/abuhakmeh/image/fetch/c_limit,f_auto,q_auto,w_500/https://khalidabuhakmeh.com/assets/images/posts/misc/strongly-typed-markdown-aspnetcore-content-apps.jpg)](https://khalidabuhakmeh.com/strongly-typed-markdown-for-aspnet-core-content-apps)