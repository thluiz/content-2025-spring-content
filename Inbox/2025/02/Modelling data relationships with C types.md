---
created: 2025-02-20T11:34:35 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/
author: Mark Seemann
---

# Modelling data relationships with C# types

> ## Excerpt
> A C# example implementation of Ghosts of Departed Proofs.

---
_A C# example implementation of Ghosts of Departed Proofs._

This article continues where [Modelling data relationships with F# types](https://blog.ploeh.dk/2025/01/20/modelling-data-relationships-with-f-types) left off. It ports the [F#](https://fsharp.org/) example code to C#. If you don't read F# source code, you may instead want to read [Implementing rod-cutting](https://blog.ploeh.dk/2024/12/23/implementing-rod-cutting) to get a sense of the problem being addressed.

I'm going to assume that you've read enough of the previous articles to get a sense of the example, but in short, this article examines if it's possible to use the type system to model data relationships. Specifically, we have methods that operate on a collection and a number. The precondition for calling these methods is that the number is a valid (one-based) index into the collection.

While you would typically implement such a precondition with a [Guard Clause](https://en.wikipedia.org/wiki/Guard_(computer_science)) and communicate it via documentation, you can also use the _Ghosts of Departed Proofs_ technique to instead leverage the type system. Please see [the previous article for an overview](https://blog.ploeh.dk/2025/01/20/modelling-data-relationships-with-f-types).

That said, I'll repeat one point here: The purpose of these articles is to showcase a technique, using a simple example to make it, I hope, sufficiently clear what's going on. All this machinery is hardly warranted for an example as simple as this. All of this is a demonstration, not a recommendation.

### Size proofs [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#24f5e14404424695aca0a2c7e049b0a5)

As in the previous article, we may start by defining what a 'size proof' looks like. In C#, it may [idiomatically](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic) be a class with an `internal` constructor.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>Size</span>&lt;<span>T</span>&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Value&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>internal</span>&nbsp;<span>Size</span>(<span>int</span>&nbsp;<span>value</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Value&nbsp;=&nbsp;<span>value</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Also&nbsp;override&nbsp;ToString,&nbsp;Equals,&nbsp;and&nbsp;GetHashCode...</span>
}
```

Since the constructor is `internal` it means that client code can't create `Size<T>` instances, and thereby client code can't decide a concrete type for the phantom type `T`.

### Issuing size proofs [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#0f22adf378da484b8dc85e372f82a0d6)

How may client code create `Size<T>` objects? It may ask a `PriceList<T>` object to issue a proof:

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>PriceList</span>&lt;<span>T</span>&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>IReadOnlyCollection</span>&lt;<span>int</span>&gt;&nbsp;Prices&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>internal</span>&nbsp;<span>PriceList</span>(<span>IReadOnlyCollection</span>&lt;<span>int</span>&gt;&nbsp;<span>prices</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Prices&nbsp;=&nbsp;<span>prices</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Size</span>&lt;<span>T</span>&gt;?&nbsp;<span>TryCreateSize</span>(<span>int</span>&nbsp;<span>candidate</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(0&nbsp;&lt;&nbsp;<span>candidate</span>&nbsp;&amp;&amp;&nbsp;<span>candidate</span>&nbsp;&lt;=&nbsp;Prices.Count)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Size</span>&lt;<span>T</span>&gt;(<span>candidate</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>else</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;More&nbsp;members&nbsp;go&nbsp;here...</span>
```

If the requested `candidate` integer represents a valid (one-indexed) position in the `PriceList<T>` object, the return value is a `Size<T>` object that contains the `candidate`. If, on the other hand, the `candidate` isn't in the valid range, no object is returned.

Since both `PriceList<T>` and `Size<T>` classes are immutable, once a 'size proof' has been issued, it remains valid. As I've previously argued, [immutability makes encapsulation simpler](https://blog.ploeh.dk/2024/06/12/simpler-encapsulation-with-immutability).

This kind of API does, however, look like it's [turtles all the way down](https://en.wikipedia.org/wiki/Turtles_all_the_way_down). After all, the `PriceList<T>` constructor is also `internal`. Now the question becomes: How does client code create `PriceList<T>` objects?

The short answer is that it doesn't. Instead, it'll be given an object by the library API. You'll see how that works later, but first, let's review what such an API enables us to express.

### Proof-based Cut API [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#079355423c6d4a7d8daaeffc08661adb)

As described in [Encapsulating rod-cutting](https://blog.ploeh.dk/2025/01/06/encapsulating-rod-cutting), returning a collection of 'cut' objects better communicates postconditions than returning a tuple of two arrays, as [the original algorithm suggested](https://blog.ploeh.dk/2024/12/23/implementing-rod-cutting). In other words, we're going to need a type for that.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>record</span>&nbsp;<span>Cut</span>&lt;<span>T</span>&gt;(<span>int</span>&nbsp;<span>Revenue</span>,&nbsp;<span>Size</span>&lt;<span>T</span>&gt;&nbsp;<span>Size</span>);
```

In this case we can get by with a simple [record type](https://learn.microsoft.com/dotnet/csharp/language-reference/builtin-types/record). Since one of the properties is of the type `Size<T>`, client code can't create `Cut<T>` instances, just like it can't create `Size<T>` or `PriceList<T>` objects. This is what we want, because a `Cut<T>` object encapsulates a proof that it's valid, related to the original collection of prices.

We can now define the `Cut` method as an instance method on `PriceList<T>`. Notice how all the `T` type arguments line up. As input, the `Cut` method only accepts `Size<T>` proofs issued by a compatible price list. This is enforced at compile time, not at run time.

```
<span>public</span>&nbsp;<span>IReadOnlyCollection</span>&lt;<span>Cut</span>&lt;<span>T</span>&gt;&gt;&nbsp;<span>Cut</span>(<span>Size</span>&lt;<span>T</span>&gt;&nbsp;<span>n</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>p</span>&nbsp;=&nbsp;Prices.<span>Prepend</span>(0).<span>ToArray</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>r</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>int</span>[<span>n</span>.Value&nbsp;+&nbsp;1];
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>s</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>int</span>[<span>n</span>.Value&nbsp;+&nbsp;1];
&nbsp;&nbsp;&nbsp;&nbsp;<span>r</span>[0]&nbsp;=&nbsp;0;
&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;(<span>int</span>&nbsp;<span>j</span>&nbsp;=&nbsp;1;&nbsp;<span>j</span>&nbsp;&lt;=&nbsp;<span>n</span>.Value;&nbsp;<span>j</span>++)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>q</span>&nbsp;=&nbsp;<span>int</span>.MinValue;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;(<span>int</span>&nbsp;<span>i</span>&nbsp;=&nbsp;1;&nbsp;<span>i</span>&nbsp;&lt;=&nbsp;<span>j</span>;&nbsp;<span>i</span>++)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>candidate</span>&nbsp;=&nbsp;<span>p</span>[<span>i</span>]&nbsp;+&nbsp;<span>r</span>[<span>j</span>&nbsp;-&nbsp;<span>i</span>];
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>q</span>&nbsp;&lt;&nbsp;<span>candidate</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>q</span>&nbsp;=&nbsp;<span>candidate</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>s</span>[<span>j</span>]&nbsp;=&nbsp;<span>i</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>r</span>[<span>j</span>]&nbsp;=&nbsp;<span>q</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>cuts</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>List</span>&lt;<span>Cut</span>&lt;<span>T</span>&gt;&gt;();
&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;(<span>int</span>&nbsp;<span>i</span>&nbsp;=&nbsp;1;&nbsp;<span>i</span>&nbsp;&lt;=&nbsp;<span>n</span>.Value;&nbsp;<span>i</span>++)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>revenue</span>&nbsp;=&nbsp;<span>r</span>[<span>i</span>];
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>size</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Size</span>&lt;<span>T</span>&gt;(<span>s</span>[<span>i</span>]);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>cuts</span>.<span>Add</span>(<span>new</span>&nbsp;<span>Cut</span>&lt;<span>T</span>&gt;(<span>revenue</span>,&nbsp;<span>size</span>));
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>cuts</span>;
}
```

For good measure, I'm showing the entire implementation, but you only need to pay attention to the method signature. The point is that `n` is constrained _by the type system_ to be in a valid range.

### Proof-based Solve API [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#ce55a32904434bd99c7f42d896fa7102)

The same technique can be applied to the `Solve` method. Just align the `T`s.

```
<span>public</span>&nbsp;<span>IReadOnlyCollection</span>&lt;<span>Size</span>&lt;<span>T</span>&gt;&gt;&nbsp;<span>Solve</span>(<span>Size</span>&lt;<span>T</span>&gt;&nbsp;<span>n</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>cuts</span>&nbsp;=&nbsp;<span>Cut</span>(<span>n</span>).<span>ToArray</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sizes</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>List</span>&lt;<span>Size</span>&lt;<span>T</span>&gt;&gt;();
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>size</span>&nbsp;=&nbsp;<span>n</span>;
&nbsp;&nbsp;&nbsp;&nbsp;<span>while</span>&nbsp;(<span>size</span>.Value&nbsp;&gt;&nbsp;0)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>sizes</span>.<span>Add</span>(<span>cuts</span>[<span>size</span>.Value&nbsp;-&nbsp;1].Size);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>size</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Size</span>&lt;<span>T</span>&gt;(<span>size</span>.Value&nbsp;-&nbsp;<span>cuts</span>[<span>size</span>.Value&nbsp;-&nbsp;1].Size.Value);
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>sizes</span>;
}
```

This is another instance method on `PriceList<T>`, which is where `T` is defined.

### Proof-based revenue API [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#57994590468c40fa87ba24f0d158720b)

Finally, we may also implement a method to calculate the revenue from a given sequence of cuts.

```
<span>public</span>&nbsp;<span>int</span>&nbsp;<span>CalculateRevenue</span>(<span>IReadOnlyCollection</span>&lt;<span>Size</span>&lt;<span>T</span>&gt;&gt;&nbsp;<span>cuts</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>arr</span>&nbsp;=&nbsp;Prices.<span>ToArray</span>();
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>cuts</span>.<span>Sum</span>(<span>c</span>&nbsp;=&gt;&nbsp;<span>arr</span>[<span>c</span>.Value&nbsp;-&nbsp;1]);
}
```

Not surprisingly, I hope, `CalculateRevenue` is another instance method on `PriceList<T>`. The `cuts` will typically come from a call to `Solve`, but it's entirely possible for client code to create an ad-hoc collection of `Size<T>` objects by repeatedly calling `TryCreateSize`.

### Running client code [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#97eba480d25e4ed8a5f7b8b0efb3a528)

How does client code use this API? It calls an `Accept` method with an implementation of this interface:

```
<span>public</span>&nbsp;<span>interface</span>&nbsp;<span>IPriceListVisitor</span>&lt;<span>TResult</span>&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>TResult</span>&nbsp;<span>Visit</span>&lt;<span>T</span>&gt;(<span>PriceList</span>&lt;<span>T</span>&gt;&nbsp;<span>priceList</span>);
}
```

Why 'visitor'? This doesn't quite look like a [Visitor](https://en.wikipedia.org/wiki/Visitor_pattern), and yet, it still does.

Imagine, for a moment, that we could enumerate all types that `T` could inhabit.

```
<span>TResult</span>&nbsp;<span>Visit</span>(<span>PriceList</span>&lt;<span>Type1</span>&gt;&nbsp;<span>priceList</span>);
<span>TResult</span>&nbsp;<span>Visit</span>(<span>PriceList</span>&lt;<span>Type2</span>&gt;&nbsp;<span>priceList</span>);
<span>TResult</span>&nbsp;<span>Visit</span>(<span>PriceList</span>&lt;<span>Type3</span>&gt;&nbsp;<span>priceList</span>);
<span>//&nbsp;⋮</span>
<span>TResult</span>&nbsp;<span>Visit</span>(<span>PriceList</span>&lt;<span>TypeN</span>&gt;&nbsp;<span>priceList</span>);
```

Clearly we can't do that, since `T` is infinite, but _if_ we could, the interface would look like a Visitor.

I find the situation sufficiently similar to name the interface with the _Visitor_ suffix. Now we only need a class with an `Accept` method.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>RodCutter</span>(<span>IReadOnlyCollection</span>&lt;<span>int</span>&gt;&nbsp;<span>prices</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>TResult</span>&nbsp;<span>Accept</span>&lt;<span>TResult</span>&gt;(<span>IPriceListVisitor</span>&lt;<span>TResult</span>&gt;&nbsp;<span>visitor</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>visitor</span>.<span>Visit</span>(<span>new</span>&nbsp;<span>PriceList</span>&lt;<span>object</span>&gt;(<span>prices</span>));
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

Client code may create a `RodCutter` object, as well as one or more classes that implement `IPriceListVisitor<TResult>`, and in this way interact with the library API.

Let's see some examples. We'll start with the original [CLRS](https://blog.ploeh.dk/ref/clrs) example, written as an [xUnit.net](https://xunit.net/) test.

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>ClrsExample</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>RodCutter</span>([1,&nbsp;5,&nbsp;8,&nbsp;9,&nbsp;10,&nbsp;17,&nbsp;17,&nbsp;20,&nbsp;24,&nbsp;30]);
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>Accept</span>(<span>new</span>&nbsp;<span>CutRodVisitor</span>(10));
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>expected</span>&nbsp;=&nbsp;<span>new</span>[]&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(&nbsp;1,&nbsp;&nbsp;1),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(&nbsp;5,&nbsp;&nbsp;2),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(&nbsp;8,&nbsp;&nbsp;3),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(10,&nbsp;&nbsp;2),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(13,&nbsp;&nbsp;2),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(17,&nbsp;&nbsp;6),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(18,&nbsp;&nbsp;1),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(22,&nbsp;&nbsp;2),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(25,&nbsp;&nbsp;3),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;(30,&nbsp;10)
&nbsp;&nbsp;&nbsp;&nbsp;};
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>(<span>expected</span>,&nbsp;<span>actual</span>);
}
```

`CutRodVisitor` is a nested class that implements the `IPriceListVisitor<TResult>` interface:

```
<span>private</span>&nbsp;<span>class</span>&nbsp;<span>CutRodVisitor</span>(<span>int</span>&nbsp;<span>i</span>)&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp;<span>IPriceListVisitor</span>&lt;<span>IReadOnlyCollection</span>&lt;(<span>int</span>,&nbsp;<span>int</span>)&gt;&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>IReadOnlyCollection</span>&lt;(<span>int</span>,&nbsp;<span>int</span>)&gt;&nbsp;<span>Visit</span>&lt;<span>T</span>&gt;(<span>PriceList</span>&lt;<span>T</span>&gt;&nbsp;<span>priceList</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>n</span>&nbsp;=&nbsp;<span>priceList</span>.<span>TryCreateSize</span>(<span>i</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>n</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;[];
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>else</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>cuts</span>&nbsp;=&nbsp;<span>priceList</span>.<span>Cut</span>(<span>n</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>cuts</span>.<span>Select</span>(<span>c</span>&nbsp;=&gt;&nbsp;(<span>c</span>.Revenue,&nbsp;<span>c</span>.Size.Value)).<span>ToArray</span>();
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

The `CutRodVisitor` class returns a collection of tuples. Why doesn't it just return `cuts` directly?

It can't, because it wouldn't type-check. Think about it for a moment. When you implement the interface, you need to pick a type for `TResult`. You can't, however, declare it to implement `IPriceListVisitor<Cut<T>>` (where `T` would be the `T` from `Visit<T>`), because at the class level, you don't know what `T` is. Neither does the compiler.

Your `Visit<T>` implementation must work for _any_ `T`.

### Preventing misalignment [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#39d9ed0b81044e5a8f7ac990cd457fad)

Finally, here's a demonstration of how the phantom type prevents confusing or mixing up two (or more) different price lists. Consider this rather artificial example:

```
[<span>Fact</span>]
<span>public</span>&nbsp;<span>void</span>&nbsp;<span>NestTwoSolutions</span>()
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>sut</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>RodCutter</span>([1,&nbsp;2,&nbsp;2]);
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>inner</span>&nbsp;=&nbsp;<span>new</span>&nbsp;<span>RodCutter</span>([1]);
 
&nbsp;&nbsp;&nbsp;&nbsp;(<span>int</span>,&nbsp;<span>int</span>)?&nbsp;<span>actual</span>&nbsp;=&nbsp;<span>sut</span>.<span>Accept</span>(<span>new</span>&nbsp;<span>NestedRevenueVisitor</span>(<span>inner</span>));
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Assert</span>.<span>Equal</span>((3,&nbsp;1),&nbsp;<span>actual</span>);
}
```

This unit test creates two price arrays and calls `Accept` on one of them (the 'outer' one, you may say), while passing the `inner` one to the Visitor, which at first glance just looks like this:

```
<span>private</span>&nbsp;<span>class</span>&nbsp;<span>NestedRevenueVisitor</span>(<span>RodCutter</span>&nbsp;<span>inner</span>)&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp;<span>IPriceListVisitor</span>&lt;(<span>int</span>,&nbsp;<span>int</span>)?&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;(<span>int</span>,&nbsp;<span>int</span>)?&nbsp;<span>Visit</span>&lt;<span>T</span>&gt;(<span>PriceList</span>&lt;<span>T</span>&gt;&nbsp;<span>priceList</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>inner</span>.<span>Accept</span>(<span>new</span>&nbsp;InnerRevenueVisitor&lt;<span>T</span>&gt;(<span>priceList</span>));
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Inner&nbsp;visitor&nbsp;goes&nbsp;here...</span>
}
```

Notice that it only delegates to yet another Visitor, passing the 'outer' `priceList` as a constructor parameter to the next Visitor. The purpose of this is to bring two `PriceList<T>` objects in scope at the same time. This will enable us to examine what happens if we make a programming mistake.

First, however, here's the proper, working implementation _without_ mistakes:

```
<span>private</span>&nbsp;<span>class</span>&nbsp;<span>InnerRevenueVisitor</span>&lt;<span>T</span>&gt;(<span>PriceList</span>&lt;<span>T</span>&gt;&nbsp;<span>priceList1</span>)&nbsp;:&nbsp;<span>IPriceListVisitor</span>&lt;(<span>int</span>,&nbsp;<span>int</span>)?&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;(<span>int</span>,&nbsp;<span>int</span>)?&nbsp;<span>Visit</span>&lt;<span>T1</span>&gt;(<span>PriceList</span>&lt;<span>T1</span>&gt;&nbsp;<span>priceList2</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>n1</span>&nbsp;=&nbsp;<span>priceList1</span>.<span>TryCreateSize</span>(3);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>n1</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>else</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>cuts1</span>&nbsp;=&nbsp;<span>priceList1</span>.<span>Solve</span>(<span>n1</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>revenue1</span>&nbsp;=&nbsp;<span>priceList1</span>.<span>CalculateRevenue</span>(<span>cuts1</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>n2</span>&nbsp;=&nbsp;<span>priceList2</span>.<span>TryCreateSize</span>(1);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>n2</span>&nbsp;<span>is</span>&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>null</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>else</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>cuts2</span>&nbsp;=&nbsp;<span>priceList2</span>.<span>Solve</span>(<span>n2</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;<span>revenue2</span>&nbsp;=&nbsp;<span>priceList2</span>.<span>CalculateRevenue</span>(<span>cuts2</span>);
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;(<span>revenue1</span>,&nbsp;<span>revenue2</span>);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

Notice how both `priceList1` and `priceList2` are now both in scope. So far, they're _not_ mixed up, so the `Visit` implementation queries first one and then another for the optimal revenue. If all works well (which it does), it returns a tuple with the two revenues.

What happens if I make a mistake? What if, for example, I write `priceList2.Solve(n1)`? It shouldn't be possible to use `n1`, which was issued by `pricelist1`, with `priceList2`. And indeed this isn't possible. With that mistake, the code doesn't compile. The compiler error is:

> Argument 1: cannot convert from 'Ploeh.Samples.RodCutting.Size' to 'Ploeh.Samples.RodCutting.Size'

When you look at the types, that makes sense. After all, there's no guarantee that `T` is equal to `T1`.

You'll run into similar problems if you mix up the two 'contexts' in other ways. The code doesn't compile. Which is what you want.

### Conclusion [#](https://blog.ploeh.dk/2025/02/03/modelling-data-relationships-with-c-types/#1075e4f3ff6548cf8806ae9f419910b1)

This article demonstrates how to use the _Ghosts of Departed Proofs_ technique in C#. In some ways, I find that it comes across as more idiomatic in C# than in F#. I think this is because rank-2 polymorphism is only possible in F# when using its object-oriented features. Since F# is a functional-first programming language, it seems a little out of place there, whereas it looks more at home in C#.

Perhaps I should have designed the F# code to make use of objects to the same degree as I've done here in C#.

I think I actually like how the C# API turned out, although having to define and implement a class every time you need to supply a Visitor may feel a bit cumbersome. Even so, [developer experience shouldn't be exclusively about saving a few keystrokes](https://blog.ploeh.dk/2024/05/13/gratification). After all, [typing isn't a bottleneck](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck).
