---
created: 2024-09-30T11:08:32 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/09/16/functor-products/
author: Mark Seemann
---

# Functor products

> ## Excerpt
> A tuple or class of functors is also a functor. An article for object-oriented developers.

---
_A tuple or class of functors is also a functor. An article for object-oriented developers._

This article is part of [a series of articles about functor relationships](https://blog.ploeh.dk/2022/07/11/functor-relationships). In this one you'll learn about a universal composition of [functors](https://blog.ploeh.dk/2018/03/22/functors). In short, if you have a [product type](https://en.wikipedia.org/wiki/Product_type) of functors, that data structure itself gives rise to a functor.

Together with other articles in this series, this result can help you answer questions such as: _Does this data structure form a functor?_

Since functors tend to be quite common, and since they're useful enough that many programming languages have special support or syntax for them, the ability to recognize a potential functor can be useful. Given a type like `Foo<T>` (C# syntax) or `Bar<T1, T2>`, being able to recognize it as a functor can come in handy. One scenario is if you yourself have just defined such a data type. Recognizing that it's a functor strongly suggests that you should give it a `Select` method in C#, a `map` function in [F#](https://fsharp.org/), and so on.

Not all generic types give rise to a (covariant) functor. Some are rather [contravariant functors](https://blog.ploeh.dk/2021/09/02/contravariant-functors), and some are [invariant](https://blog.ploeh.dk/2022/08/01/invariant-functors).

If, on the other hand, you have a data type which is a product of two or more (covariant) functors _with the same type parameter_, then the data type itself gives rise to a functor. You'll see some examples in this article.

### Abstract shape [#](https://blog.ploeh.dk/2024/09/16/functor-products/#9fc25288b4504ff3b4fabe932ecf2ea2)

Before we look at some examples found in other code, it helps if we know what we're looking for. Most (if not all?) languages support product types. In canonical form, they're just tuples of values, but in an object-oriented language like C#, such types are typically classes.

Imagine that you have two functors `F` and `G`, and you're now considering a data structure that contains a value of both types.

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>FAndG</span>&lt;<span>T</span>&gt;
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>FAndG</span>(<span>F</span>&lt;<span>T</span>&gt;&nbsp;<span>f</span>,&nbsp;<span>G</span>&lt;<span>T</span>&gt;&nbsp;<span>g</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;F&nbsp;=&nbsp;<span>f</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;G&nbsp;=&nbsp;<span>g</span>;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>F</span>&lt;<span>T</span>&gt;&nbsp;F&nbsp;{&nbsp;<span>get</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>G</span>&lt;<span>T</span>&gt;&nbsp;G&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>//&nbsp;Methods&nbsp;go&nbsp;here...</span>
```

The name of the type is `FAndG<T>` because it contains both an `F<T>` object and a `G<T>` object.

Notice that it's an essential requirement that the individual functors (here `F` and `G`) are parametrized by the same type parameter (here `T`). If your data structure contains `F<T1>` and `G<T2>`, the following 'theorem' doesn't apply.

The point of this article is that such an `FAndG<T>` data structure forms a functor. The `Select` implementation is quite unsurprising:

```
<span>public</span>&nbsp;<span>FAndG</span>&lt;<span>TResult</span>&gt;&nbsp;<span>Select</span>&lt;<span>TResult</span>&gt;(<span>Func</span>&lt;<span>T</span>,&nbsp;<span>TResult</span>&gt;&nbsp;<span>selector</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>FAndG</span>&lt;<span>TResult</span>&gt;(F.<span>Select</span>(<span>selector</span>),&nbsp;G.<span>Select</span>(<span>selector</span>));
}
```

Since we've assumed that both `F` and `G` already are functors, they must come with some projection function. In C# it's [idiomatically](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic) called `Select`, while in F# it'd typically be called `map`:

```
<span>//&nbsp;('a&nbsp;-&gt;&nbsp;'b)&nbsp;-&gt;&nbsp;FAndG&lt;'a&gt;&nbsp;-&gt;&nbsp;FAndG&lt;'b&gt;</span>
<span>let</span>&nbsp;<span>map</span>&nbsp;<span>f</span>&nbsp;<span>fandg</span>&nbsp;=&nbsp;{&nbsp;F&nbsp;=&nbsp;<span>F</span>.<span>map</span>&nbsp;<span>f</span>&nbsp;<span>fandg</span>.F;&nbsp;G&nbsp;=&nbsp;<span>G</span>.<span>map</span>&nbsp;<span>f</span>&nbsp;<span>fandg</span>.G&nbsp;}
```

assuming a record type like

```
<span>type</span>&nbsp;<span>FAndG</span>&lt;<span>'a</span>&gt;&nbsp;=&nbsp;{&nbsp;F&nbsp;:&nbsp;<span>F</span>&lt;<span>'a</span>&gt;;&nbsp;G&nbsp;:&nbsp;<span>G</span>&lt;<span>'a</span>&gt;&nbsp;}
```

In both the C# `Select` example and the F# `map` function, the composed functor passes the function argument (`selector` or `f`) to both `F` and `G` and uses it to map both constituents. It then composes a new product from these individual results.

I'll have more to say about how this generalizes to a product of more than two functors, but first, let's consider some examples.

### List Zipper [#](https://blog.ploeh.dk/2024/09/16/functor-products/#e3b18df7ac4440d7aada000ce27044f3)

One of the simplest example I can think of is a List Zipper, which [in Haskell](https://learnyouahaskell.com/zippers) is nothing but a type alias of a tuple of lists:

```
<span>type</span>&nbsp;ListZipper&nbsp;a&nbsp;=&nbsp;([a],[a])
```

In the article [A List Zipper in C#](https://blog.ploeh.dk/2024/08/26/a-list-zipper-in-c) you saw how the `ListZipper<T>` class composes two `IEnumerable<T>` objects.

```
<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;values;
<span>public</span>&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;Breadcrumbs&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
<span>private</span>&nbsp;<span>ListZipper</span>(<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;<span>values</span>,&nbsp;<span>IEnumerable</span>&lt;<span>T</span>&gt;&nbsp;<span>breadcrumbs</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.values&nbsp;=&nbsp;<span>values</span>;
&nbsp;&nbsp;&nbsp;&nbsp;Breadcrumbs&nbsp;=&nbsp;<span>breadcrumbs</span>;
}
```

Since we already know that sequences like `IEnumerable<T>` form functors, we now know that so must `ListZipper<T>`. And indeed, the `Select` implementation looks similar to the above 'shape outline'.

```
<span>public</span>&nbsp;<span>ListZipper</span>&lt;<span>TResult</span>&gt;&nbsp;<span>Select</span>&lt;<span>TResult</span>&gt;(<span>Func</span>&lt;<span>T</span>,&nbsp;<span>TResult</span>&gt;&nbsp;<span>selector</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>ListZipper</span>&lt;<span>TResult</span>&gt;(values.<span>Select</span>(<span>selector</span>),&nbsp;Breadcrumbs.<span>Select</span>(<span>selector</span>));
}
```

It passes the `selector` function to the `Select` method of both `values` and `Breadcrumbs`, and composes the results into a `new ListZipper<TResult>`.

While this example is straightforward, it may not be the most compelling, because `ListZipper<T>` composes two identical functors: `IEnumerable<T>`. The knowledge that functors compose is more general than that.

### Non-empty collection [#](https://blog.ploeh.dk/2024/09/16/functor-products/#051a4cc14ad74c2ca2f62fd12051f97c)

Next after the above List Zipper, the simplest example I can think of is a non-empty list. On this blog I originally introduced it in the article [Semigroups accumulate](https://blog.ploeh.dk/2017/12/11/semigroups-accumulate), but here I'll use the variant from [NonEmpty catamorphism](https://blog.ploeh.dk/2023/08/07/nonempty-catamorphism). It composes a single value of the type `T` with an `IReadOnlyCollection<T>`.

```
<span>public</span>&nbsp;<span>NonEmptyCollection</span>(<span>T</span>&nbsp;<span>head</span>,&nbsp;<span>params</span>&nbsp;<span>T</span>[]&nbsp;<span>tail</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>head</span>&nbsp;==&nbsp;<span>null</span>)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>throw</span>&nbsp;<span>new</span>&nbsp;<span>ArgumentNullException</span>(<span>nameof</span>(<span>head</span>));
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.Head&nbsp;=&nbsp;<span>head</span>;
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.Tail&nbsp;=&nbsp;<span>tail</span>;
}
 
<span>public</span>&nbsp;<span>T</span>&nbsp;Head&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
<span>public</span>&nbsp;<span>IReadOnlyCollection</span>&lt;<span>T</span>&gt;&nbsp;Tail&nbsp;{&nbsp;<span>get</span>;&nbsp;}
```

The `Tail`, being an `IReadOnlyCollection<T>`, easily forms a functor, since it's a kind of list. But what about `Head`, which is a 'naked' `T` value? Does that form a functor? If so, which one?

Indeed, a 'naked' `T` value is isomorphic to [the Identity functor](https://blog.ploeh.dk/2018/09/03/the-identity-functor). This situation is an example of how knowing about the Identity functor is useful, even if you never actually write code that uses it. Once you realize that `T` is equivalent with a functor, you've now established that `NonEmptyCollection<T>` composes two functors. Therefore, it must itself form a functor, and you realize that you can give it a `Select` method.

```
<span>public</span>&nbsp;<span>NonEmptyCollection</span>&lt;<span>TResult</span>&gt;&nbsp;<span>Select</span>&lt;<span>TResult</span>&gt;(<span>Func</span>&lt;<span>T</span>,&nbsp;<span>TResult</span>&gt;&nbsp;<span>selector</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>NonEmptyCollection</span>&lt;<span>TResult</span>&gt;(<span>selector</span>(Head),&nbsp;Tail.<span>Select</span>(<span>selector</span>).<span>ToArray</span>());
}
```

Notice that even though we understand that `T` is equivalent to the Identity functor, there's no reason to actually wrap `Head` in an `Identity<T>` [container](https://bartoszmilewski.com/2014/01/14/functors-are-containers/) just to call `Select` on it and unwrap the result. Rather, the above `Select` implementation directly invokes `selector` with `Head`. It is, after all, a function that takes a `T` value as input and returns a `TResult` object as output.

### Ranges [#](https://blog.ploeh.dk/2024/09/16/functor-products/#d721c5ba6eda4016be1417ea01105bea)

It's hard to come up with an example that's both somewhat compelling and realistic, and at the same time prototypically pure. Stripped of all 'noise' functor products are just tuples, but that hardly makes for a compelling example. On the other hand, most other examples I can think of combine results about functors where they compose in more than one way. Not only as products, but also as sums of functors, as well as nested compositions. You'll be able to read about these in future articles, but for the next examples, you'll have to accept some claims about functors at face value.

In [Range as a functor](https://blog.ploeh.dk/2024/02/12/range-as-a-functor) you saw how both `Endpoint<T>` and `Range<T>` are functors. The article shows functor implementations for each, in both C#, F#, and [Haskell](https://www.haskell.org/). For now we'll ignore the deeper underlying reason why `Endpoint<T>` forms a functor, and instead focus on `Range<T>`.

In Haskell I never defined an explicit `Range` type, but rather just treated ranges as tuples. As stated repeatedly already, tuples are the essential products, so if you accept that `Endpoint` gives rise to a functor, then a 'range tuple' does, too.

In F# `Range` is defined like this:

```
<span>type</span>&nbsp;Range&lt;'a&gt;&nbsp;=&nbsp;{&nbsp;LowerBound&nbsp;:&nbsp;Endpoint&lt;'a&gt;;&nbsp;UpperBound&nbsp;:&nbsp;Endpoint&lt;'a&gt;&nbsp;}
```

Such a record type is also easily identified as a product type. In a sense, we can think of a record type as a 'tuple with metadata', where the metadata contains _names_ of elements.

In C# `Range<T>` is a class with two `Endpoint<T>` fields.

```
<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>Endpoint</span>&lt;<span>T</span>&gt;&nbsp;min;
<span>private</span>&nbsp;<span>readonly</span>&nbsp;<span>Endpoint</span>&lt;<span>T</span>&gt;&nbsp;max;
 
<span>public</span>&nbsp;<span>Range</span>(<span>Endpoint</span>&lt;<span>T</span>&gt;&nbsp;<span>min</span>,&nbsp;<span>Endpoint</span>&lt;<span>T</span>&gt;&nbsp;<span>max</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.min&nbsp;=&nbsp;<span>min</span>;
&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>.max&nbsp;=&nbsp;<span>max</span>;
}
```

In a sense, you can think of such an immutable class as equivalent to a record type, only requiring substantial [ceremony](https://blog.ploeh.dk/2019/12/16/zone-of-ceremony). The point is that because a range is a product of two functors, it itself gives rise to a functor. You can see all the implementations in [Range as a functor](https://blog.ploeh.dk/2024/02/12/range-as-a-functor).

### Binary tree Zipper [#](https://blog.ploeh.dk/2024/09/16/functor-products/#25e4dea36f644217ba1e28f2a509f3ab)

In [A Binary Tree Zipper in C#](https://blog.ploeh.dk/2024/09/09/a-binary-tree-zipper-in-c) you saw that the `BinaryTreeZipper<T>` class has two class fields:

```
<span>public</span>&nbsp;<span>BinaryTree</span>&lt;<span>T</span>&gt;&nbsp;Tree&nbsp;{&nbsp;<span>get</span>;&nbsp;}
<span>public</span>&nbsp;<span>IEnumerable</span>&lt;<span>Crumb</span>&lt;<span>T</span>&gt;&gt;&nbsp;Breadcrumbs&nbsp;{&nbsp;<span>get</span>;&nbsp;}
```

Both have the same generic type parameter `T`, so the question is whether `BinaryTreeZipper<T>` may form a functor? We now know that the answer is affirmative if `BinaryTree<T>` and `IEnumerable<Crumb<T>>` are both functors.

For now, believe me when I claim that this is the case. This means that you can add a `Select` method to the class:

```
<span>public</span>&nbsp;<span>BinaryTreeZipper</span>&lt;<span>TResult</span>&gt;&nbsp;<span>Select</span>&lt;<span>TResult</span>&gt;(<span>Func</span>&lt;<span>T</span>,&nbsp;<span>TResult</span>&gt;&nbsp;<span>selector</span>)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>BinaryTreeZipper</span>&lt;<span>TResult</span>&gt;(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tree.<span>Select</span>(<span>selector</span>),
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Breadcrumbs.<span>Select</span>(<span>c</span>&nbsp;=&gt;&nbsp;<span>c</span>.<span>Select</span>(<span>selector</span>)));
}
```

By now, this should hardly be surprising: Call `Select` on each constituent functor and create a proper return value from the results.

### Higher arities [#](https://blog.ploeh.dk/2024/09/16/functor-products/#1179fc33850d430780c843583c16adcb)

All examples have involved products of only two functors, but the result generalizes to higher arities. To gain an understanding of why, consider that it's always possible to rewrite tuples of higher arities as nested pairs. As an example, a triple like `(42, "foo", True)` can be rewritten as `(42, ("foo", True))` without loss of information. The latter representation is a pair (a two-tuple) where the first element is `42`, but the second element is another pair. These two representations are isomorphic, meaning that we can go back and forth without losing data.

By induction you can generalize this result to any arity. The point is that the only data type you need to describe a product is a pair.

Haskell's [base](https://hackage.haskell.org/package/base) library defines a specialized container called [Product](https://hackage.haskell.org/package/base/docs/Data-Functor-Product.html) for this very purpose: If you have two `Functor` instances, you can `Pair` them up, and they become a single `Functor`.

Let's start with a `Pair` of `Maybe` and a list:

```
ghci&gt; Pair (Just "foo") ["bar", "baz", "qux"]
Pair (Just "foo") ["bar","baz","qux"]
```

This is a single 'object', if you will, that composes those two `Functor` instances. This means that you can map over it:

```
ghci&gt; elem 'b' &lt;$&gt; Pair (Just "foo") ["bar", "baz", "qux"]
Pair (Just False) [True,True,False]
```

Here I've used the infix `<$>` operator as an alternative to `fmap`. By composing with `elem 'b'`, I'm asking every value inside the container whether or not it contains the character `b`. The `Maybe` value doesn't, while the first two list elements do.

If you want to compose three, rather than two, `Functor` instances, you just nest the `Pairs`, just like you can nest tuples:

```
ghci&gt; elem 'b' &lt;$&gt; Pair (Identity "quux") (Pair (Just "foo") ["bar", "baz", "qux"])
Pair (Identity False) (Pair (Just False) [True,True,False])
```

This example now introduces the `Identity` container as a third `Functor` instance. I could have used any other `Functor` instance instead of `Identity`, but some of them are more awkward to create or display. For example, the [Reader](https://blog.ploeh.dk/2021/08/30/the-reader-functor) or [State](https://blog.ploeh.dk/2021/07/19/the-state-functor) functors have no `Show` instances in Haskell, meaning that GHCi doesn't know how to print them as values. Other `Functor` instances didn't work as well for the example, since they tend to be more awkward to create. As an example, any non-trivial [Tree](https://hackage.haskell.org/package/containers/docs/Data-Tree.html#t:Tree) requires substantial editor space to express.

### Conclusion [#](https://blog.ploeh.dk/2024/09/16/functor-products/#329c3274f8f54171905d747867fc293b)

A product of functors may itself be made a functor. The examples shown in this article are all constrained to two functors, but if you have a product of three, four, or more functors, that product still gives rise to a functor.

This is useful to know, particularly if you're working in a language with only partial support for functors. Mainstream languages aren't going to automatically turn such products into functors, in the way that Haskell's `Product` container almost does. Thus, knowing when you can safely give your generic types a `Select` method or `map` function may come in handy.

There are more rules like this one. The next article examines another.

**Next:** Functor sums.
