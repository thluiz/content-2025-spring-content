---
created: 2024-09-11T20:22:07 (UTC -03:00)
tags: [kotlin,functional,ddd,architecture,software,coding,development,engineering,inclusive,community]
source: https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8
author: 
---

# Introduction to Functional Domain Driven Design in Kotlin - DEV Community

> ## Excerpt
> Photo by Mel Poole on Unsplash           Introduction   In the past few years, Functional Programming...

---
Photo by [Mel Poole](https://unsplash.com/@melpoole?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash) on [Unsplash](https://unsplash.com/photos/black-and-white-heart-shaped-textile-LuaT29bdjMA?utm_content=creditCopyText&utm_medium=referral&utm_source=unsplash)

## [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#introduction)Introduction

In the past few years, **[Functional Programming](https://geekflare.com/functional-programming/) (FP)** has become increasingly popular in the world of Software Development, due to its many benefits: reduced complexity, better readability, better code reusability, robustness, and testing, etc.. Most mainstream programming languages now include FP features such as immutability, lambdas, and sealed classes.

On the other hand, **[Domain Driven Design](https://www.lucidchart.com/blog/domain-driven-design-introduction) (DDD)** is a methodology focused on modeling and designing complex enterprise applications around the core domain. Over the years, it has established itself as the de facto standard in this realm. However, DDD was initially conceived in **Object Oriented Programming (OOP)** languages like Java and C#. FP and OOP are fundamentally different: the same problem is solved with different solutions in each paradigm.

[![OOP vs FP](https://res.cloudinary.com/practicaldev/image/fetch/s--LNhexIns--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4a9jm02mnwyjgt528hd7.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--LNhexIns--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/4a9jm02mnwyjgt528hd7.png)

Credits: Robert C. Martin (Uncle Bob)

### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#functional-programming-and-domain-driven-design)Functional Programming and Domain Driven Design

Therefore, you might have already asked yourself: **Can I implement Domain Driven Design in a Functional way?** Does that even make sense? I am here today to explain that, yes, it is possible, and it not only makes sense but also offers numerous benefits.

In this article, I will outline the different pieces of the puzzle, how to combine them and achieve **Functional Domain Driven Design**, and what we can gain from it! We will be using Kotlin as a supporting language for the implementation.

## [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#isolating-domain-logic)Isolating Domain Logic

The first piece of the puzzle is to start **isolating the Domain logic**  
from the other layers / concerns of the application. The Domain layer should be “at the center” of the app and should have no dependency, while the other layers (use cases, infrastructure, etc.) should depend on the domain layer. One common way of doing that is to put the domain logic in its own Gradle module, without any dependencies on other modules.

[![An example of possible Application Architecture](https://res.cloudinary.com/practicaldev/image/fetch/s--0n6z0HZN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/buwjgg26kcnmchh4azok.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--0n6z0HZN--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/buwjgg26kcnmchh4azok.png)

An example of a possible Application Architecture with a Functional Domain layer

### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#domain-layer-vs-use-cases-layer)Domain Layer vs. Use Cases Layer

For that step, it is especially important to make a distinction between the Domain Layer and the Use Cases layer. These two layers are often merged or confused into a single one, and that can make it harder to introduce the functional domain layer. In case you're uncertain about the distinction between Use Cases and Domain, here are their respective responsibilities:

[![Domain Layer vs Use Cases Layer](https://res.cloudinary.com/practicaldev/image/fetch/s--tOOnyTJi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5pr6yje71ynp8w1tu4vi.png)](https://res.cloudinary.com/practicaldev/image/fetch/s--tOOnyTJi--/c_limit%2Cf_auto%2Cfl_progressive%2Cq_auto%2Cw_800/https://dev-to-uploads.s3.amazonaws.com/uploads/articles/5pr6yje71ynp8w1tu4vi.png)

Once that separation is achieved, we can start to make the Domain Layer functional (more on that in the next section). One key aspect of the Functional paradigm is that functions should be **pure**, meaning their output should only depend on their input and they should have **no side effects**. Conversely, an **impure** function has side effects or might not consistently return the same result for a given input. The important bit is that impure functions can call pure functions with no particular consequence, but pure functions cannot call an impure function without becoming impure as well. This is why our functional domain layer, which contains only pure functions, needs to be at the center of the app, while the other layers contain impure code and invoke the pure domain layer functions.

Later, we can choose to expand the functional core of our application by making the outer layers functional as well, starting with the Use Case layer up until the entry points (Controllers, Event Listeners, etc.). That decision is up to you and will depend on how comfortable your team is with FP, the framework you’re using, etc. By the way, this pattern is called **Functional Core, Imperative Shell**.

## [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#functional-domain-modeling)Functional Domain Modeling

The second piece of the puzzle is to start making the domain model functional. To achieve this, we have various tools in our functional toolbox. Today, we will see only a few, the most important ones, but I intend to write more articles to talk about the other tools.

### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#separate-state-and-logic)Separate state and logic

In the traditional DDD world, the entities contain both the state and the logic, and there is an emphasis on hiding and protecting the state from the outside world, aka encapsulation. Also, the domain logic is contained in methods on the entities themselves, which will mutate the entity fields directly.  

```
<span>class</span> <span>Order</span> <span>{</span>
    <span>var</span> <span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CREATED</span>
        <span>private</span> <span>set</span>

    <span>val</span> <span>id</span><span>:</span> <span>UUID</span> <span>=</span> <span>UUID</span><span>.</span><span>randomUUID</span><span>()</span>

    <span>fun</span> <span>cancel</span><span>()</span> <span>{</span>
        <span>if</span> <span>(</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span><span>)</span> <span>{</span>
            <span>throw</span> <span>IllegalStateException</span><span>(</span><span>"Cannot cancel already confirmed order"</span><span>)</span>
        <span>}</span>
        <span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span>
    <span>}</span>

    <span>fun</span> <span>confirm</span><span>()</span> <span>{</span>
        <span>if</span> <span>(</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span><span>)</span> <span>{</span>
            <span>throw</span> <span>IllegalStateException</span><span>(</span><span>"Cannot confirm already cancelled order"</span><span>)</span>
        <span>}</span>
        <span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span>
    <span>}</span>
<span>}</span>
```

However, in the FP paradigm, we try to avoid state and mutations; We prefer using immutable data structures instead and create copies of them. This approach is way simpler to reason about and less error-prone.

For the Entity’s state, we can simply use Kotlin’s `data class` with only immutable properties. If we want to “mutate” an entity, we need to create a copy of the original entity with the modified fields, using the very convenient `copy` method. For the Entity’s logic, we use pure functions that accept the current state as input, perform some logic, and return a new copied instance of the state with the relevant properties modified.  

```
<span>data class</span> <span>Order</span><span>(</span>
    <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>,</span>
    <span>val</span> <span>status</span><span>:</span> <span>OrderStatus</span><span>,</span>
<span>)</span>

<span>fun</span> <span>createOrder</span><span>()</span> <span>=</span> <span>Order</span><span>(</span>
    <span>id</span> <span>=</span> <span>UUID</span><span>.</span><span>randomUUID</span><span>(),</span>
    <span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CREATED</span><span>,</span>
<span>)</span>

<span>fun</span> <span>cancelOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>):</span> <span>Order</span> <span>{</span>
    <span>if</span> <span>(</span><span>order</span><span>.</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span><span>)</span> <span>{</span>
        <span>throw</span> <span>OrderException</span><span>(</span><span>"Cannot cancel already confirmed order"</span><span>)</span>
    <span>}</span>
    <span>return</span> <span>order</span><span>.</span><span>copy</span><span>(</span><span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span><span>)</span>
<span>}</span>

<span>fun</span> <span>confirmOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>):</span> <span>Order</span> <span>{</span>
    <span>if</span> <span>(</span><span>order</span><span>.</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span><span>)</span> <span>{</span>
        <span>throw</span> <span>OrderException</span><span>(</span><span>"Cannot confirm already cancelled order"</span><span>)</span>
    <span>}</span>
    <span>return</span> <span>order</span><span>.</span><span>copy</span><span>(</span><span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span><span>)</span>
<span>}</span> 
```

Using this method has some big advantages: immutable classes cannot cause unexpected side effects and mutations, since they cannot be modified in the first place; they can also be shared across threads without risk. Also, they're easier to understand since there's no mutation, eliminating the need to track state changes. Finally, pure functions give the same output for the same input and make the code easier to understand, maintain, test, and debug.

### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#leverage-the-type-system)Leverage the type system

Another important tool is to leverage the type system to bake some logic, validation, and meaning directly into the types.

#### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#tiny-types)Tiny types

**Value Classes** (aka **Tiny Types**), are wrappers around primitives (e.g., strings, integers) that give them domain-specific meaning and type safety. For example, consider the following code:  

```
<span>data class</span> <span>Order</span><span>(</span>
    <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>,</span>
    <span>val</span> <span>customerEmail</span><span>:</span> <span>String</span><span>,</span>
    <span>val</span> <span>customerAddress</span><span>:</span> <span>String</span><span>,</span>
    <span>val</span> <span>status</span><span>:</span> <span>OrderStatus</span><span>,</span>
<span>)</span>
```

As you can see, we have 2 string fields, `customerEmail` and `customerAddress`, which could easily be confused, and we could end up with the email in the address field or vice versa. Same for the `id` field: we could confuse the `Order` id with the `Product` id for example (I have seen that many times in production). On top of that, we probably want to avoid invalid emails, so we should probably add some email validation in the `Order` constructor. This validation might be repeated in multiple places.

Instead, what we can do is introduce tiny types for the “email”, “address” and “order id” types. For the email and address types, we can even add some validation in the constructor. This means we can now use the `Email` type and we do not have to do any validation anymore; the compiler is giving us the guarantee that the type has passed this validation.  

```
<span>@JvmInline</span> <span>value</span> <span>class</span> <span>OrderId</span><span>(</span><span>val</span> <span>value</span><span>:</span> <span>UUID</span><span>)</span>
<span>@JvmInline</span> <span>value</span> <span>class</span> <span>Email</span><span>(</span><span>val</span> <span>value</span><span>:</span> <span>String</span><span>)</span> <span>{</span>
    <span>init</span> <span>{</span> <span>/* Email validation logic here */</span> <span>}</span>
<span>}</span>
<span>@JvmInline</span> <span>value</span> <span>class</span> <span>Address</span><span>(</span><span>val</span> <span>value</span><span>:</span> <span>String</span><span>)</span> <span>{</span>
    <span>init</span> <span>{</span> <span>/* Address validation logic here */</span> <span>}</span>
<span>}</span>

<span>data class</span> <span>Order</span><span>(</span>
    <span>val</span> <span>id</span><span>:</span> <span>OrderId</span><span>,</span>
    <span>val</span> <span>customerEmail</span><span>:</span> <span>Email</span><span>,</span>
    <span>val</span> <span>customerAddress</span><span>:</span> <span>Address</span><span>,</span>
    <span>val</span> <span>status</span><span>:</span> <span>OrderStatus</span><span>,</span>
<span>)</span>
```

And the cherry on the cake? This wrapper only exists at compile time and will be erased at runtime, so there is no performance overhead (with some caveats).

#### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#sealed-classes)Sealed Classes

**Sealed Classes** can be used to represent the lifecycle of an entity directly into the types, and help prevent nullable fields. For example, imagine we have an `Order` entity with 3 different steps, each step having its own set of fields. We could represent it something like this:  

```
<span>data class</span> <span>Order</span><span>(</span>
    <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>,</span>
    <span>val</span> <span>creationDate</span><span>:</span> <span>Instant</span><span>,</span>
    <span>val</span> <span>status</span><span>:</span> <span>OrderStatus</span><span>,</span>
    <span>// only non-null when status == CANCELLED</span>
    <span>val</span> <span>cancellationReason</span><span>:</span> <span>CancellationReason</span><span>?,</span>
    <span>val</span> <span>cancellationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
    <span>// only non-null when status == CONFIRMED</span>
    <span>val</span> <span>confirmationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
<span>)</span>
```

As you can see, this class has multiple nullable fields, and it’s not immediately clear which field is expected in which step. We need to add some comments to explain that; if we add more steps, this technique will not scale and the model will become even more confusing. Furthermore, the nullable fields are always error-prone, and the less we have, the better we are!

What we can do is introduce a `sealed class` and represent each step as a child of the parent sealed class. Then, each child class can declare the fields that are specific to this step, as non-nullable.  

```
<span>sealed</span> <span>class</span> <span>Order</span> <span>{</span>
    <span>abstract</span> <span>val</span> <span>id</span><span>:</span> <span>UUID</span>
    <span>data class</span> <span>Created</span><span>(</span><span>override</span> <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>)</span> <span>:</span> <span>Order</span><span>()</span>
    <span>data class</span> <span>Cancelled</span><span>(</span>
        <span>override</span> <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>,</span>
        <span>val</span> <span>cancellationReason</span><span>:</span> <span>CancellationReason</span><span>?,</span>
        <span>val</span> <span>cancellationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
    <span>)</span> <span>:</span> <span>Order</span><span>()</span>
    <span>data class</span> <span>Confirmed</span><span>(</span>
        <span>override</span> <span>val</span> <span>id</span><span>:</span> <span>UUID</span><span>,</span>
        <span>val</span> <span>confirmationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
    <span>)</span> <span>:</span> <span>Order</span><span>()</span>
<span>}</span>

<span>val</span> <span>Order</span><span>.</span><span>status</span><span>:</span> <span>OrderStatus</span>
    <span>get</span><span>()</span> <span>=</span> <span>when</span> <span>(</span><span>this</span><span>)</span> <span>{</span>
        <span>is</span> <span>Order</span><span>.</span><span>Created</span> <span>-&gt;</span> <span>OrderStatus</span><span>.</span><span>CREATED</span>
        <span>is</span> <span>Order</span><span>.</span><span>Cancelled</span> <span>-&gt;</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span>
        <span>is</span> <span>Order</span><span>.</span><span>Confirmed</span> <span>-&gt;</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span>
    <span>}</span>
```

If we need to perform logic depending on the exact step, we will need to use `when` and pattern matching. Since the type hierarchy is restricted, the compiler will help us to verify that all cases are covered, by triggering a compilation error if a case is not covered.  

```
<span>fun</span> <span>Order</span><span>.</span><span>cancel</span><span>(</span>
    <span>cancellationReason</span><span>:</span> <span>CancellationReason</span><span>,</span>
    <span>cancellationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
<span>):</span> <span>Order</span><span>.</span><span>Cancelled</span> <span>=</span> <span>when</span> <span>(</span><span>this</span><span>)</span> <span>{</span>
    <span>is</span> <span>Order</span><span>.</span><span>Created</span> <span>-&gt;</span> <span>Order</span><span>.</span><span>Cancelled</span><span>(</span>
        <span>id</span> <span>=</span> <span>id</span><span>,</span>
        <span>cancellationReason</span> <span>=</span> <span>cancellationReason</span><span>,</span>
        <span>cancellationDateTime</span> <span>=</span> <span>cancellationDateTime</span><span>,</span>
    <span>)</span>
    <span>is</span> <span>Order</span><span>.</span><span>Confirmed</span> <span>-&gt;</span> <span>Order</span><span>.</span><span>Cancelled</span><span>(</span>
        <span>id</span> <span>=</span> <span>id</span><span>,</span>
        <span>cancellationReason</span> <span>=</span> <span>cancellationReason</span><span>,</span>
        <span>cancellationDateTime</span> <span>=</span> <span>cancellationDateTime</span><span>,</span>
    <span>)</span>
    <span>is</span> <span>Order</span><span>.</span><span>Cancelled</span> <span>-&gt;</span> <span>throw</span> <span>IllegalStateException</span><span>(</span><span>"Order is already cancelled"</span><span>)</span>
<span>}</span>
```

#### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#arrows-either-data-type)Arrow's Either data type

The traditional way of handling failures in OOP languages has always been exception handling. Whenever we detect a failure case, we can `throw` an exception: an exception object is created with a copy of the stack trace, and the execution of the current function and its parents is stopped, until the nearest `catch` block in the call stack is reached. Then that catch block can perform appropriate recovery logic. If there is no catch block, our program will simply crash. In our domain example, that would look something like this:  

```
<span>fun</span> <span>confirmOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>):</span> <span>Order</span> <span>{</span>
    <span>if</span> <span>(</span><span>order</span><span>.</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span><span>)</span> <span>{</span>
        <span>throw</span> <span>OrderException</span><span>(</span><span>"Cannot confirm already cancelled order"</span><span>)</span>
    <span>}</span>
    <span>return</span> <span>order</span><span>.</span><span>copy</span><span>(</span><span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span><span>)</span>
<span>}</span>

<span>fun</span> <span>handleConfirmOrder</span><span>(</span><span>id</span><span>:</span> <span>String</span><span>)</span> <span>{</span>
    <span>try</span> <span>{</span>
        <span>val</span> <span>order</span> <span>=</span> <span>findOrderById</span><span>(</span><span>id</span><span>)</span>
        <span>val</span> <span>confirmedOrder</span> <span>=</span> <span>confirmOrder</span><span>(</span><span>order</span><span>)</span>
        <span>saveOrder</span><span>(</span><span>confirmedOrder</span><span>)</span>
    <span>}</span> <span>catch</span> <span>(</span><span>e</span><span>:</span> <span>Exception</span><span>)</span> <span>{</span>
        <span>log</span><span>.</span><span>error</span><span>(</span><span>"Error confirming order $id"</span><span>,</span> <span>e</span><span>)</span>
        <span>// recovery logic</span>
    <span>}</span>
<span>}</span>
```

When we are used to exceptions, this code does not seem to pose any problem. However, exceptions can cause many issues: it is sometimes not easy to identify which function is throwing exceptions, and if they are throwing, which type of exception exactly. Same, it's not always clear where the nearest catch block is. Control flow becomes harder to understand, even more if exceptions are improperly used. On top of that, exceptions can degrade performance, since they require to create [a copy of the stack](https://www.baeldung.com/java-exceptions-performance).

Luckily there are other simpler and type-safe ways of handling failures. One efficient way to do that in Kotlin is to use the [Either](https://apidocs.arrow-kt.io/arrow-core/arrow.core/-either/index.html) class of the [Arrow](https://github.com/arrow-kt/arrow) library. The idea is to encapsulate failures in the return type of the function, instead of throwing exceptions. The `Either` data type represents a value that can be either `Left` or `Right`. By convention, the left path represents failure and the right path represents success. When the function needs to signal a failure case, it will simply return a failure object wrapped in an `Either.Left`. Conversely, the success path return value needs to be wrapped in an `Either.Right`. Later, the calling code is forced by the compiler to handle the failure case to extract the success case, since the result is wrapped in an `Either`. In terms of code, this would look something like this:  

```
<span>fun</span> <span>confirmOrder</span><span>(</span><span>order</span><span>:</span> <span>Order</span><span>):</span> <span>Either</span><span>&lt;</span><span>OrderError</span><span>,</span> <span>Order</span><span>&gt;</span> <span>{</span>
    <span>if</span> <span>(</span><span>order</span><span>.</span><span>status</span> <span>==</span> <span>OrderStatus</span><span>.</span><span>CANCELLED</span><span>)</span> <span>{</span>
        <span>return</span> <span>Either</span><span>.</span><span>Left</span><span>(</span><span>OrderError</span><span>(</span><span>"Cannot confirm already cancelled order"</span><span>))</span>
    <span>}</span>
    <span>return</span> <span>Either</span><span>.</span><span>Right</span><span>(</span><span>order</span><span>.</span><span>copy</span><span>(</span><span>status</span> <span>=</span> <span>OrderStatus</span><span>.</span><span>CONFIRMED</span><span>))</span>
<span>}</span>

<span>fun</span> <span>handleConfirmOrder</span><span>(</span><span>id</span><span>:</span> <span>String</span><span>)</span> <span>{</span>
    <span>val</span> <span>order</span> <span>=</span> <span>findOrderById</span><span>(</span><span>id</span><span>)</span>
    <span>val</span> <span>result</span> <span>=</span> <span>confirmOrder</span><span>(</span><span>order</span><span>)</span>
    <span>when</span> <span>(</span><span>result</span><span>)</span> <span>{</span>
        <span>is</span> <span>Either</span><span>.</span><span>Left</span> <span>-&gt;</span> <span>{</span>
            <span>log</span><span>.</span><span>error</span><span>(</span><span>"Error confirming order $id: ${result.value.message}"</span><span>)</span>
            <span>// recovery logic</span>
        <span>}</span>
        <span>is</span> <span>Either</span><span>.</span><span>Right</span> <span>-&gt;</span> <span>saveOrder</span><span>(</span><span>result</span><span>.</span><span>value</span><span>)</span>
    <span>}</span>
<span>}</span>
```

The main benefit is that now we have achieved type-safe error handling; it is now crystal clear which function can fail: it's in the function return type! On top of that, the control flow is now much easier to understand: in case of failure, the function will simply return a different value than if it had succeeded.

If we combine all the tools we have learned today, we obtain something like this:  

```
<span>@JvmInline</span> <span>value</span> <span>class</span> <span>OrderId</span><span>(</span><span>val</span> <span>value</span><span>:</span> <span>UUID</span><span>)</span>
<span>sealed</span> <span>class</span> <span>Order</span> <span>{</span>
    <span>abstract</span> <span>val</span> <span>id</span><span>:</span> <span>OrderId</span>
    <span>data class</span> <span>Created</span><span>(</span><span>override</span> <span>val</span> <span>id</span><span>:</span> <span>OrderId</span><span>)</span> <span>:</span> <span>Order</span><span>()</span>
    <span>data class</span> <span>Cancelled</span><span>(</span>
        <span>override</span> <span>val</span> <span>id</span><span>:</span> <span>OrderId</span><span>,</span>
        <span>val</span> <span>cancellationReason</span><span>:</span> <span>CancellationReason</span><span>?,</span>
        <span>val</span> <span>cancellationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
    <span>)</span> <span>:</span> <span>Order</span><span>()</span>
    <span>data class</span> <span>Confirmed</span><span>(</span>
        <span>override</span> <span>val</span> <span>id</span><span>:</span> <span>OrderId</span><span>,</span>
        <span>val</span> <span>confirmationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
    <span>)</span> <span>:</span> <span>Order</span><span>()</span>
<span>}</span>
<span>fun</span> <span>createOrder</span><span>():</span> <span>Order</span> <span>=</span> <span>Order</span><span>.</span><span>Created</span><span>(</span><span>OrderId</span><span>(</span><span>UUID</span><span>.</span><span>randomUUID</span><span>()))</span>
<span>fun</span> <span>confirmOrder</span><span>(</span>
    <span>order</span><span>:</span> <span>Order</span><span>,</span>
    <span>confirmationDateTime</span><span>:</span> <span>Instant</span><span>,</span>
<span>):</span> <span>Either</span><span>&lt;</span><span>OrderError</span><span>,</span> <span>Order</span><span>&gt;</span> <span>=</span> <span>when</span> <span>(</span><span>order</span><span>)</span> <span>{</span>
    <span>is</span> <span>Order</span><span>.</span><span>Cancelled</span> <span>-&gt;</span> <span>Either</span><span>.</span><span>Left</span><span>(</span><span>OrderError</span><span>(</span><span>"Cannot confirm already cancelled order"</span><span>))</span>
    <span>is</span> <span>Order</span><span>.</span><span>Confirmed</span> <span>-&gt;</span> <span>Either</span><span>.</span><span>Left</span><span>(</span><span>OrderError</span><span>(</span><span>"Cannot confirm already confirmed order"</span><span>))</span>
    <span>is</span> <span>Order</span><span>.</span><span>Created</span> <span>-&gt;</span> <span>Either</span><span>.</span><span>Right</span><span>(</span><span>Order</span><span>.</span><span>Confirmed</span><span>(</span><span>order</span><span>.</span><span>id</span><span>,</span> <span>confirmationDateTime</span><span>))</span>
<span>}</span>
```

These tools not only increase the type safety of our domain model by reducing nulls, preventing wrong assignments, and reducing the need for validation, but they also increase the readability and expressiveness of our code!

## [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#conclusion)Conclusion

In this article, we have learned the basic principles of Functional DDD, and how to start using it in your application now:

-   First, correctly isolating the domain layer from the other layers so that it can be made functional. The other layers can also gradually be made functional if desired.
    
-   Second, make the domain model functional by:
    
    -   Separating state and logic, the state being represented by immutable data classes and the logic represented by pure functions, thus simplifying the model and making it easier to reason about.
    -   Using tools available in the Kotlin type system, such as Tiny Types, Sealed Classes and Arrow's Either, to enhance type safety and code readability.

If you found this post helpful, please consider sharing it with your colleagues. I plan to write more articles about Functional DDD in Kotlin, mainly about domain modeling. I also intend to create a sample repository to show everything wired together. If you have any questions or doubts about this subject, please let me know in the comment, and I will try to address it either as a comment or a blog post. Thank you for reading!

### [](https://dev.to/petitcl/introduction-to-functional-domain-driven-design-in-kotlin-5dg8#resources)Resources

-   [Functional Domain Modeling in Kotlin | 47 Degrees](https://www.47deg.com/blog/functional-domain-modeling/)
-   [My journey from Aggregates to Functional Composition - Event-Driven.io](https://event-driven.io/en/my_journey_from_aggregates/)
-   [How To Design Domain Model in Kotlin | HackerNoon](https://hackernoon.com/how-to-design-domain-model-in-kotlin-5t1931d8)
-   [Domain Modeling Made Functional - Scott Wlaschin](https://www.youtube.com/watch?v=Up7LcbGZFuo)
-   [Functional architecture - The pits of success - Mark Seemann](https://www.youtube.com/watch?v=US8QG9I1XW0)
-   [From Dependency injection to dependency rejection - Mark Seemann](https://www.goodreads.com/book/show/179133.Domain_Driven_Design)
-   [Domain-Driven Design: Tackling Complexity in the Heart of Software - Eric Evans](https://www.goodreads.com/book/show/179133.Domain_Driven_Design)
