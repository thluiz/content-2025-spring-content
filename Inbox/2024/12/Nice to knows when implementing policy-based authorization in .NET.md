---
created: 2024-09-30T11:05:51 (UTC -03:00)
tags: [Angular,NgRx,TypeScript,Blog,Blogger,Tim,Deschryver,Tim
Deschryver,Belgium]
source: https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#
author: Tim Deschryver
---

# Nice to knows when implementing policy-based authorization in .NET

> ## Excerpt
> I assumed to know how policy-based authorization works in .NET, but I was wrong. Let's cover the basics to get a better understanding of how to implement a policy, and what to look out for. I also share some tips and tricks that improve your authorization layer.

---
Modified September 13, 2024

I am writing this post to share my experience after I encountered a couple of unforeseen behaviors while implementing a policy-based authorization layer in .NET. I hope this post helps you avoid the same "pitfalls", or at least raise some awareness about this topic.

Reflecting back, this was due to my lack of understanding of how policy-based authorization works in .NET. Instead of getting a deep understanding of the topic, I made some assumptions, which were wrong (don't just assume, but test and verify!). The [Policy Authorization documentation](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-8.0&WT.mc_id=DT-MVP-5004452) does a good job explaining the concepts, and covers most of the scenarios you might encounter. I recommend you read it before writing your own policies so you don't make the same mistakes I did.

Getting this right is critical, as it's the first line of defense against unauthorized access to your application. What's why it's ranked first on the OWASP TOP 10 list, [A01:2021 – Broken Access Control](https://owasp.org/Top10/A01_2021-Broken_Access_Control/) [A01 Broken Access Control](https://cheatsheetseries.owasp.org/cheatsheets/DotNet_Security_Cheat_Sheet.html#a01-broken-access-control).

## [Recap: Policies in ASP.NET](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#recap-policies-in-aspnet) link

In ASP.NET, policies are used to define requirements that must be met in order to give a user the authorization to access an endpoint.

Within Web API projects you can use policies to guard and protect endpoints from unauthorized users that don't meet the criteria. Instead of writing the authorization logic in the endpoint itself, you can refactor this into a policy that is reusable across multiple endpoints. By extracting the authorization logic from the endpoint, you can make your code more clear and keep your endpoints concise.

To register a policy:

1.  Add the `AddAuthorization()` middleware;
2.  Use the `AddPolicy()` method to define a policy with its requirements;
3.  Register the authorization handler(s) to the DI container;
4.  Enable the authorization middleware in the application by invoking `UseAuthorization()`;
5.  Guard your endpoint with the policy by using `RequireAuthorization()` for Minimal APIs, for controllers use the `[Authorize]` attribute;

Create a new class that inherits from  `AuthorizationHandler<TRequirement>`, you will have to implement the `HandleRequirementAsync` method, which's responsibility is evaluating the request against the requirement.

In the `HandleRequirementAsync` method, you can implement the authorization logic and use the passed in `AuthorizationHandlerContext` object to flag the requirement as (un)met.

-   Use the `context.Succeed(requirement)` method to indicate that the requirement is met;
-   Use the `context.Fail()` method to indicate that the requirement is not met;
-   Don't invoke the `context.Fail()` method when the requirement to let another handler evaluate the requirement (more on this later);

The requirement in this case is a simple class that implements the `IAuthorizationRequirement` interface. For more complex requirements you can add properties to the requirement class, which can be used to configure the requirement.

## [Nice to knows](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#nice-to-knows) link

### [All policies (handlers) are always evaluated](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#all-policies-handlers-are-always-evaluated) link

This was the biggest surprise to me.

I expected that when a requirement is not fulfilled that it would short-circuit the process and stop the evaluation of the other policy handlers. But, this is not the case.

When multiple policies are applied to an endpoint, all of them are evaluated regardless of the outcome of the previous policies. The same scenario applies when a requirement is validated by multiple handlers (yes, you can have a requirement that is validated by more than one handler - more on this later).

I was interested in this decision, and it turns out that this is by design as it allows policy handlers to execute side-effects, such as logging. This can be crucial information for auditing purposes.

If your use case is different you can opt out of this behavior and stop the evaluation of the other policies on a first failure. To do so, set the `InvokeHandlersAfterFailure` property to `false` in the `AuthorizationOptions` configuration. This will stop the evaluation of the remaining policies after the first failure.

Be aware that this only takes place when a policy handler explicitly invokes the `context.Fail()` method. A handler that does not invoke `context.Succeed()`, and simply returns, is not sufficient to stop the evaluation of the other policies.

See the implementation of [DefaultAuthorizationService](https://github.com/dotnet/aspnetcore/blob/63c492e22b06d4903cb4e7aee037295c71e1ec37/src/Security/Authorization/Core/src/DefaultAuthorizationService.cs#L62) to understand how policies are evaluated.

#### [Example](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#example) link

In the following example, the two policies `PolicyName` and `OtherPolicyName` require a different policy requirement, and is implemented by two different handlers. These handlers are always evaluated regardless of the outcome of the other policy.

### [The order of execution is not guaranteed](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#the-order-of-execution-is-not-guaranteed) link

The order in which the policies are evaluated cannot be guaranteed. You cannot safely rely on the order of the policies when implementing your authorization logic.

A handler should be implemented as stand alone and should be written in a way that does not depend on the order of the policies.

If for some reason you want to know which policies are still in the pipeline, you can use the `PendingRequirements` property of the `AuthorizationHandlerContext` object.

### [Unauthenticated requests are also evaluated by policies](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#unauthenticated-requests-are-also-evaluated-by-policies) link

But what happens if the user is not authenticated?

To understand this behavior we first need to know how ASP.NET deals with (un)authenticated requests. Using the `RequireAuthorization()` method (in Minimal APIs) or the  `Authorize` attribute (in controllers), adds the `DenyAnonymousAuthorizationRequirement` requirement to the authorization pipeline.

With the knowledge that all policies are always evaluated and the insights cannot safely rely on the order of the policies, we can conclude that our policy handlers are also called even when the user is not authenticated.

As a solution, always check if the user is authenticated within your policy handlers. For some inspiration, take a look at how  `DenyAnonymousAuthorizationRequirement` is [implemented](https://github.com/dotnet/aspnetcore/blob/63c492e22b06d4903cb4e7aee037295c71e1ec37/src/Security/Authorization/Core/src/DenyAnonymousAuthorizationRequirement.cs#L22).

#### [Example](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#example) link

In the following example, the endpoint is protected with the default `RequireAuthorization()`, which means that the user must be authenticated. Additionally, the endpoint is also protected by the policy `PolicyName`, using `RequireAuthorization("PolicyName")`. The policy handler of `PolicyName` is evaluated regardless if the user is authenticated.

### [Difference between `FallbackPolicy` and `DefaultPolicy`](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#difference-between-fallbackpolicy-and-defaultpolicy) link

While configuring the authorization options, you might have noticed the `FallbackPolicy` and `DefaultPolicy` properties. While they might sound similar, they have different purposes.

-   The `FallbackPolicy` is used when an endpoint does not have any policies applied to it.
-   The `DefaultPolicy` sets the default policy for an endpoint that uses the `Authorize` attribute or the `RequireAuthorization()` method, but does not specify any policies.

### [Multiple handlers for the same requirement](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#multiple-handlers-for-the-same-requirement) link

We touched on this topic earlier, but it's worth going into more detail. When multiple handlers that check an identical requirement are registered, these handlers are all executed.

For the requirement to be met, it's sufficient that one of the handlers succeeds. The same applies when a handler fails, the requirement is not met.

This makes it important to how handlers handle unsuccessful authorization attempts. I suggest using the `context.Fail()` method in exceptional cases only. Use it only to enforce a strict rule that must be met. In other all other cases simply do not invoke the `context.Succeed()`. This makes it flexible for other handlers to evaluate the requirement.

The example in the [documentation](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/policies?view=aspnetcore-8.0&WT.mc_id=DT-MVP-5004452#why-would-i-want-multiple-handlers-for-a-requirement) shows a good example of how to handle multiple handlers for the same requirement. It uses a building entry system as an example, where the requirement is to have a valid badge to enter the building. If an employee forgets the badge, she can still enter the building by making use of a temporary badge.

Because the primary handler (verify the badge) does not call the `context.Fail()` method, the secondary handler (temporary badge) can still evaluate the requirement and give the user access to the building.

#### [Example](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#example) link

In the example below, the endpoint is protected by a single policy. However, that policy is implemented by two authorization handlers (handling an identical requirement). The endpoint is only accessible when one of the policies invokes `context.Succeed()` and the other does not invoke `context.Fail()`. The endpoint is not accessible in the following scenarios:

-   One of the policies invokes `context.Fail()`;
-   None of the policies do not invoke `context.Succeed()`;

### [Authorization handler that checks multiple requirements](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#authorization-handler-that-checks-multiple-requirements) link

So far we've only seen authorization handlers that check a single requirement. For use cases where a handler needs to check multiple requirements, you can create a handler that implements the `IAuthorizationHandler` interface. Using the context have access to all the pending requirements using `PendingRequirements`, and can evaluate them accordingly.

This can be useful when you want to have some more control over the authorization flow, for example, "OR" logic can easier be implemented.

### [Resource-based authorization](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#resource-based-authorization) link

For the cases when you want to check if the user has access to a specific resource there's the `IAuthorizationService` service. Instead of marking an endpoint using `RequireAuthorization()` (or the `[Authorize]` attribute), you can manually invoke the authorization flow.

In fact, you can also replace the `RequireAuthorization()` with the `IAuthorizationService` service for the previous examples. The reworked example injects the `IAuthorizationService` service and invokes the `AuthorizeAsync()` method to check if the user has enough permissions to access the endpoint.

The `AuthorizeAsync` method accepts the user and the policy name as arguments and returns an `AuthorizationResult` object.

This approach doesn't bring much value in this case, but it can be useful when you want to check the authorization of a specific resource. `AuthorizeAsync` additionally accepts a resource object. This resource object can be used to check if the user has access to that resource.

Why is this useful? This technique can be leveraged to avoid multiple roundtrips to the database. Instead of retrieving a resource in the policy handler and in the endpoint, you can retrieve the resource in the endpoint and pass it to the policy handler.

The authorization handler that is configured to handle the policy can also accept a second parameter, which is the resource object. In this case, the retrieved document is passed to the handler.

Register the handler with the DI container and configure the policy is done the same way as before.

## [Conclusion](https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net#conclusion) link

In this blog post, we went through the basics of policy-based authorization in .NET. Why it's useful, and how to implement a policy-based authorization layer in your application using requirements (`IAuthorizationRequirement`) and authorization handlers (`AuthorizationHandler`).

The key takeaways when building your own policies are:

-   If a policy requirement is not satisfied, the other policies are still evaluated (this also applies when the user is not authenticated)
-   Policies are evaluated in an unpredictable order, do not rely on the order of the policies or authorization handlers
-   It's possible to write multiple authorization handlers for the same requirement. Here it's sufficient that one of the handlers succeeds for the requirement to be met. Invoke `context.Fail()` in one of the handlers only when you want to enforce a strict rule, in other cases simply do nothing when the requirement is not met (in this scenario a different handler can verify the requirement).
-   A single authorization handler (using `IAuthorizationHandler`) can check multiple requirements (can be used to implement an `AND` or `OR` logic) at once.
-   For [resource-based authorization](https://learn.microsoft.com/en-us/aspnet/core/security/authorization/resourcebased?view=aspnetcore-8.0&WT.mc_id=DT-MVP-5004452) leverage the `IAuthorizationService` service to manually invoke the authorization flow to avoid multiple roundtrips to the database (performance matters!).

Feel free to update this blog post on [GitHub](https://github.com/timdeschryver/timdeschryver.dev/tree/main/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net/index.md), thanks in advance!

#### Join My Newsletter (WIP)

Join my weekly newsletter to receive my latest blog posts and bits, directly in your inbox.

#### Support me

I appreciate it if you would support me if have you enjoyed this post and found it useful, thank you in advance.

[![Buy Me a Coffee at ko-fi.com](https://timdeschryver.dev/images/kofi.webp)](https://ko-fi.com/W7W579HXS) [![PayPal logo](data:image/svg+xml;base64,PHN2ZyB3aWR0aD0iMTAxcHgiIGhlaWdodD0iMzIiIHZpZXdCb3g9IjAgMCAxMDEgMzIiIHByZXNlcnZlQXNwZWN0UmF0aW89InhNaW5ZTWluIG1lZXQiIHhtbG5zPSJodHRwOiYjeDJGOyYjeDJGO3d3dy53My5vcmcmI3gyRjsyMDAwJiN4MkY7c3ZnIj48cGF0aCBmaWxsPSIjMDAzMDg3IiBkPSJNIDEyLjIzNyAyLjggTCA0LjQzNyAyLjggQyAzLjkzNyAyLjggMy40MzcgMy4yIDMuMzM3IDMuNyBMIDAuMjM3IDIzLjcgQyAwLjEzNyAyNC4xIDAuNDM3IDI0LjQgMC44MzcgMjQuNCBMIDQuNTM3IDI0LjQgQyA1LjAzNyAyNC40IDUuNTM3IDI0IDUuNjM3IDIzLjUgTCA2LjQzNyAxOC4xIEMgNi41MzcgMTcuNiA2LjkzNyAxNy4yIDcuNTM3IDE3LjIgTCAxMC4wMzcgMTcuMiBDIDE1LjEzNyAxNy4yIDE4LjEzNyAxNC43IDE4LjkzNyA5LjggQyAxOS4yMzcgNy43IDE4LjkzNyA2IDE3LjkzNyA0LjggQyAxNi44MzcgMy41IDE0LjgzNyAyLjggMTIuMjM3IDIuOCBaIE0gMTMuMTM3IDEwLjEgQyAxMi43MzcgMTIuOSAxMC41MzcgMTIuOSA4LjUzNyAxMi45IEwgNy4zMzcgMTIuOSBMIDguMTM3IDcuNyBDIDguMTM3IDcuNCA4LjQzNyA3LjIgOC43MzcgNy4yIEwgOS4yMzcgNy4yIEMgMTAuNjM3IDcuMiAxMS45MzcgNy4yIDEyLjYzNyA4IEMgMTMuMTM3IDguNCAxMy4zMzcgOS4xIDEzLjEzNyAxMC4xIFoiPjwvcGF0aD48cGF0aCBmaWxsPSIjMDAzMDg3IiBkPSJNIDM1LjQzNyAxMCBMIDMxLjczNyAxMCBDIDMxLjQzNyAxMCAzMS4xMzcgMTAuMiAzMS4xMzcgMTAuNSBMIDMwLjkzNyAxMS41IEwgMzAuNjM3IDExLjEgQyAyOS44MzcgOS45IDI4LjAzNyA5LjUgMjYuMjM3IDkuNSBDIDIyLjEzNyA5LjUgMTguNjM3IDEyLjYgMTcuOTM3IDE3IEMgMTcuNTM3IDE5LjIgMTguMDM3IDIxLjMgMTkuMzM3IDIyLjcgQyAyMC40MzcgMjQgMjIuMTM3IDI0LjYgMjQuMDM3IDI0LjYgQyAyNy4zMzcgMjQuNiAyOS4yMzcgMjIuNSAyOS4yMzcgMjIuNSBMIDI5LjAzNyAyMy41IEMgMjguOTM3IDIzLjkgMjkuMjM3IDI0LjMgMjkuNjM3IDI0LjMgTCAzMy4wMzcgMjQuMyBDIDMzLjUzNyAyNC4zIDM0LjAzNyAyMy45IDM0LjEzNyAyMy40IEwgMzYuMTM3IDEwLjYgQyAzNi4yMzcgMTAuNCAzNS44MzcgMTAgMzUuNDM3IDEwIFogTSAzMC4zMzcgMTcuMiBDIDI5LjkzNyAxOS4zIDI4LjMzNyAyMC44IDI2LjEzNyAyMC44IEMgMjUuMDM3IDIwLjggMjQuMjM3IDIwLjUgMjMuNjM3IDE5LjggQyAyMy4wMzcgMTkuMSAyMi44MzcgMTguMiAyMy4wMzcgMTcuMiBDIDIzLjMzNyAxNS4xIDI1LjEzNyAxMy42IDI3LjIzNyAxMy42IEMgMjguMzM3IDEzLjYgMjkuMTM3IDE0IDI5LjczNyAxNC42IEMgMzAuMjM3IDE1LjMgMzAuNDM3IDE2LjIgMzAuMzM3IDE3LjIgWiI+PC9wYXRoPjxwYXRoIGZpbGw9IiMwMDMwODciIGQ9Ik0gNTUuMzM3IDEwIEwgNTEuNjM3IDEwIEMgNTEuMjM3IDEwIDUwLjkzNyAxMC4yIDUwLjczNyAxMC41IEwgNDUuNTM3IDE4LjEgTCA0My4zMzcgMTAuOCBDIDQzLjIzNyAxMC4zIDQyLjczNyAxMCA0Mi4zMzcgMTAgTCAzOC42MzcgMTAgQyAzOC4yMzcgMTAgMzcuODM3IDEwLjQgMzguMDM3IDEwLjkgTCA0Mi4xMzcgMjMgTCAzOC4yMzcgMjguNCBDIDM3LjkzNyAyOC44IDM4LjIzNyAyOS40IDM4LjczNyAyOS40IEwgNDIuNDM3IDI5LjQgQyA0Mi44MzcgMjkuNCA0My4xMzcgMjkuMiA0My4zMzcgMjguOSBMIDU1LjgzNyAxMC45IEMgNTYuMTM3IDEwLjYgNTUuODM3IDEwIDU1LjMzNyAxMCBaIj48L3BhdGg+PHBhdGggZmlsbD0iIzAwOWNkZSIgZD0iTSA2Ny43MzcgMi44IEwgNTkuOTM3IDIuOCBDIDU5LjQzNyAyLjggNTguOTM3IDMuMiA1OC44MzcgMy43IEwgNTUuNzM3IDIzLjYgQyA1NS42MzcgMjQgNTUuOTM3IDI0LjMgNTYuMzM3IDI0LjMgTCA2MC4zMzcgMjQuMyBDIDYwLjczNyAyNC4zIDYxLjAzNyAyNCA2MS4wMzcgMjMuNyBMIDYxLjkzNyAxOCBDIDYyLjAzNyAxNy41IDYyLjQzNyAxNy4xIDYzLjAzNyAxNy4xIEwgNjUuNTM3IDE3LjEgQyA3MC42MzcgMTcuMSA3My42MzcgMTQuNiA3NC40MzcgOS43IEMgNzQuNzM3IDcuNiA3NC40MzcgNS45IDczLjQzNyA0LjcgQyA3Mi4yMzcgMy41IDcwLjMzNyAyLjggNjcuNzM3IDIuOCBaIE0gNjguNjM3IDEwLjEgQyA2OC4yMzcgMTIuOSA2Ni4wMzcgMTIuOSA2NC4wMzcgMTIuOSBMIDYyLjgzNyAxMi45IEwgNjMuNjM3IDcuNyBDIDYzLjYzNyA3LjQgNjMuOTM3IDcuMiA2NC4yMzcgNy4yIEwgNjQuNzM3IDcuMiBDIDY2LjEzNyA3LjIgNjcuNDM3IDcuMiA2OC4xMzcgOCBDIDY4LjYzNyA4LjQgNjguNzM3IDkuMSA2OC42MzcgMTAuMSBaIj48L3BhdGg+PHBhdGggZmlsbD0iIzAwOWNkZSIgZD0iTSA5MC45MzcgMTAgTCA4Ny4yMzcgMTAgQyA4Ni45MzcgMTAgODYuNjM3IDEwLjIgODYuNjM3IDEwLjUgTCA4Ni40MzcgMTEuNSBMIDg2LjEzNyAxMS4xIEMgODUuMzM3IDkuOSA4My41MzcgOS41IDgxLjczNyA5LjUgQyA3Ny42MzcgOS41IDc0LjEzNyAxMi42IDczLjQzNyAxNyBDIDczLjAzNyAxOS4yIDczLjUzNyAyMS4zIDc0LjgzNyAyMi43IEMgNzUuOTM3IDI0IDc3LjYzNyAyNC42IDc5LjUzNyAyNC42IEMgODIuODM3IDI0LjYgODQuNzM3IDIyLjUgODQuNzM3IDIyLjUgTCA4NC41MzcgMjMuNSBDIDg0LjQzNyAyMy45IDg0LjczNyAyNC4zIDg1LjEzNyAyNC4zIEwgODguNTM3IDI0LjMgQyA4OS4wMzcgMjQuMyA4OS41MzcgMjMuOSA4OS42MzcgMjMuNCBMIDkxLjYzNyAxMC42IEMgOTEuNjM3IDEwLjQgOTEuMzM3IDEwIDkwLjkzNyAxMCBaIE0gODUuNzM3IDE3LjIgQyA4NS4zMzcgMTkuMyA4My43MzcgMjAuOCA4MS41MzcgMjAuOCBDIDgwLjQzNyAyMC44IDc5LjYzNyAyMC41IDc5LjAzNyAxOS44IEMgNzguNDM3IDE5LjEgNzguMjM3IDE4LjIgNzguNDM3IDE3LjIgQyA3OC43MzcgMTUuMSA4MC41MzcgMTMuNiA4Mi42MzcgMTMuNiBDIDgzLjczNyAxMy42IDg0LjUzNyAxNCA4NS4xMzcgMTQuNiBDIDg1LjczNyAxNS4zIDg1LjkzNyAxNi4yIDg1LjczNyAxNy4yIFoiPjwvcGF0aD48cGF0aCBmaWxsPSIjMDA5Y2RlIiBkPSJNIDk1LjMzNyAzLjMgTCA5Mi4xMzcgMjMuNiBDIDkyLjAzNyAyNCA5Mi4zMzcgMjQuMyA5Mi43MzcgMjQuMyBMIDk1LjkzNyAyNC4zIEMgOTYuNDM3IDI0LjMgOTYuOTM3IDIzLjkgOTcuMDM3IDIzLjQgTCAxMDAuMjM3IDMuNSBDIDEwMC4zMzcgMy4xIDEwMC4wMzcgMi44IDk5LjYzNyAyLjggTCA5Ni4wMzcgMi44IEMgOTUuNjM3IDIuOCA5NS40MzcgMyA5NS4zMzcgMy4zIFoiPjwvcGF0aD48L3N2Zz4=)](https://www.paypal.com/donate?hosted_button_id=59M5TFPQJS8SQ)

#### Share this post

[Twitter](https://twitter.com/intent/tweet?text=Nice%20to%20knows%20when%20implementing%20policy-based%20authorization%20in%20.NET&via=tim_deschryver&url=https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net) [LinkedIn](https://www.linkedin.com/sharing/share-offsite/?url=https://timdeschryver.dev/blog/nice-to-knows-when-implementing-policy-based-authorization-in-net)
