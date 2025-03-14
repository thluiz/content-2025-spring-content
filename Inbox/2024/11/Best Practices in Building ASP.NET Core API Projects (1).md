---
created: 2024-09-30T10:27:57 (UTC -03:00)
tags: []
source: https://www.telerik.com/blogs/best-practices-building-aspnet-core-api-projects
author: Peter Vogel
---

# Best Practices in Building ASP.NET Core API Projects

> ## Excerpt
> Here’s what you need to do to create an ASP.NET Core API backend that you can live with.

---
In many ways, the best practices for an ASP.NET Core backend are the same as for any other application you create. But there are some best practices that address issues unique to structuring API projects (and even to ASP.NET Core projects).

As the web development universe moves more and more to client-side development, you have to start paying more attention to how you set up your API backend. Fortunately, as with other application development, applying the [SOLID principles](https://en.wikipedia.org/wiki/SOLID) and the appropriate [design patterns](https://www.telerik.com/blogs/aspnet-core-basics-knowing-applying-design-patterns) give you your best shot at creating a backend that you live with … at least, where API backends are similar to other kinds of applications.

However, there are some areas where API projects and, specifically, ASP.NET Core API projects are different from other kinds of applications. Here are some best practices in those areas.

## Minimal APIs vs. APIControllers

Your first decision in creating a API backend with ASP.NET Core is deciding whether you’ll use [API Controller classes](https://www.telerik.com/blogs/aspnet-core-beginners-web-apis) or [minimal APIs](https://www.telerik.com/blogs/building-testing-minimal-apis-aspnet-core-7).

There’s a lot to be said for the functionality that API controller classes provide: Model binding, parameter-based routing, model validation and more—all stuff you don’t have to build. In addition, if you have experience with building MVC controllers, API controllers will look very familiar to you, letting you start turning out functionality faster than moving to a new technology (i.e., minimal APIs). API controller classes also support ASP.NET Core’s Application Parts which let you avoid loading the whole application when only part of it is needed.

But having said that, minimal APIs are easier to write than API controller classes: Just start adding code in Program.cs and you’ve created your API. With minimal APIs, your backend project could have a single code file: Program.cs. In addition, minimal APIs provide you with options around [managing routes](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/route-handlers) through the MapGroup method that aren’t available in controller classes. Unless you have a compelling reason to use APIControllers (e.g., leveraging experience with MVC Controllers, leveraging existing model classes with data validation support), you want to use minimal APIs.

Best practice here is finding ways to avoid bloat in your Program.cs file. If you’re building a simple service with a very focused, limited set of functionality (a licensing service, for example), having all your code in Program.cs might be a reasonable approach. However, for larger applications, managing the size of your Program.cs file becomes a problem: If you’re not careful, team members will end up fighting over who gets to check Program.cs out and the result of merges will become … interesting.

There are a variety of strategies for managing Program.cs bloat. You can move the body of your API methods out of Program.cs by calling methods on classes [instantiated in Program.cs code](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/route-handlers?view=aspnetcore-8.0#instance-method) or by [calling static methods on classes](https://learn.microsoft.com/en-us/aspnet/core/fundamentals/minimal-apis/route-handlers?view=aspnetcore-8.0#static-method) within your API methods.

Alternatively, you can split Program.cs into multiple files by configuring it as a partial class with your minimal API definitions in one of the partial class files. Another option (my favorite) is to pass Program.cs’s Application Builder object to a method (or methods) in a file outside of Program.cs and define your minimal API methods there.

The best practice here is not which strategy you pick but to pick one and apply it early in the life of your project before your Program.cs file grows out of control.

## Your External ‘Client Experience’

In general, applications can be thought of as a way of converting backend resources (e.g., the customer table, the sales order table and the products table in your database) into a coherent user experience (the sales order update page). The same is true for your services.

Logistically, you’ll want to organize your services around your organization’s departments in order to (for example) simplify funding, assign support teams and manage the stakeholders involved in deciding on your service’s evolution. Technically, you’ll want to structure your API projects using the SOLID principles, Separation of Concerns and so on.

None of those, of course, matter to the clients who will be accessing your service and, if you’re not careful, a typical client transaction will need to traverse multiple services. The “purchasing a product” transaction, for example, may require a client to integrate services spread across the sales, inventory, warehouse, shipping and accounts receivable services.

Your clients will, instead, want to be able to send a request to a single endpoint to perform what the client regards as a single transaction. You’ll need to distribute that single request over the multiple services that you’ve created to support that request (and, occasionally, to do the “atypical” thing, clients will need to access those individual services directly).

To handle this, you need a façade front end for your API (something like [Azure API Management](https://learn.microsoft.com/en-us/azure/api-management/api-management-key-concepts)). If the façade you’re using doesn’t support orchestration (the distribution of a single request over multiple backend services), then you should either write your own request-specific orchestrators or buy a third-party tool.

Normally, I’m a big fan of third-party tools because they let me concentrate on the functionality that’s unique to my application. However, in the absence of an orchestrator, writing your own request-specific orchestrator isn’t a bad practice (and may even qualify as a best practice).

Creating a request-specific orchestrator shouldn’t be a major effort, provided you keep your orchestrator focused on a specific transaction (i.e., don’t try to evolve the orchestrator designed to handle “purchasing a product” into a general purpose tool). In fact, if the orchestrator for a transaction isn’t simple—if it isn’t just handing requests and portions of a message off to various services—it’s a sign that your service architecture has problems.

## Your Project’s Internal Structure

Your API backend will evolve and change over time. As you enhance your API backend, always consider whether any new code belongs in an existing project or should be the start point for a new project.

Remember that in ASP.NET you’re _not_ required to have all the code for any endpoint in the same project (and the reverse is also true: code for different endpoints can be in the same project). You should think about how responsibility for maintaining/funding this code will be distributed across teams and departments and structure your projects to support that.

As services evolve, it may make sense to create two projects out of an existing project (typically because a project is getting too big for its team or is violating the Single Responsibility Principle or Separation of Concerns). That division will typically be handled by distributing the API’s features over two or more projects.

The best practice here is to organize the code inside your API project by features to make it easier to distribute your code over new projects. If there are multiple teams supporting a single API project (which may be a sign that’s time to split the project up), typically each team will be responsible for different features. As a result, folders inside your project should hold the unique code for each of your API’s features (which may or may not correspond with your endpoints); project resources that are shared by multiple features (e.g., base classes or helper classes) should be in their own folders.

## Versioning Services

As you add or remove functionality from the services behind an endpoint, there will often be a period of time where both the old and the new version of a service have to be available. That may be permanent (to support clients who don’t want to access added functionality in the new version) or short-term (to support a migration period). During this period, you need to support versioning so that clients can pick the version of the service they want.

You have options here: You can incorporate versioning information in the endpoint’s URL (e.g., …/v1/customers vs. …/v2/customers), in the endpoint’s querystring (e.g., …/customers?v=1 vs. …/customers?v=2), or in a header (X-API-Version has been provisionally registered since 2005 for this purpose).

There is no generally accepted right answer here. Personally, I think that embedding version information in the URL violates the convention that endpoints are consistent over time. As a result, I prefer using the querystring or header strategies. You’ll make your own decision. Your clients are probably holding your endpoint’s URLs in configuration files that are easy to update so modifying URLs isn’t an onerous task for the clients as long as version information precedes any parameter values in the URL. The real effort for clients, after all, is in restructuring messages and taking advantage of new functionality.

Whatever strategy you select, the best practice here is to leverage the ASP.Versioning.Http NuGet package in ASP.NET Core to implement versioning. That package adds the AddApiVersioning extension to the Services object which, in turn, lets you specify ApiVersioningOptions in Program.cs. By default, the package expects version information to be in the querystring but adding support for reading a header is simple (and the package also supports versioning the URL, if you insist). If you’re using Swagger, you’ll also want to add the Asp.Versioning.Mvc.ApiExplorer package.

Actually tying your code to a version is handled differently in minimal APIs than in APIControllers—[Milan Jovanović](https://www.milanjovanovic.tech/blog/api-versioning-in-aspnetcore) has a great discussion of the options with the necessary code.

This isn’t an exhaustive list (other best practices: [document your API](https://www.telerik.com/blogs/aspnet-core-basics-documenting-apis), [use JSON](https://www.telerik.com/blogs/asp.net-core-basics--working-with-json) and [more](https://www.telerik.com/blogs/7-tips-building-good-web-api)). But if you implement these practices, you’ll have a better shot of creating an API backend that you can live with (at least, without more pain than is absolutely necessary).
