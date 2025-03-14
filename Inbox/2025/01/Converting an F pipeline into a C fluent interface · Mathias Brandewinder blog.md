---
created: 2025-01-29T08:11:10 (UTC -03:00)
tags: []
source: https://brandewinder.com/2025/01/08/convert-fsharp-pipeline-into-csharp-fluent-interface/
author: 
---

# Converting an F# pipeline into a C# fluent interface · Mathias Brandewinder blog

> ## Excerpt
> 08 Jan 2025

---
08 Jan 2025

In my [previous post](https://brandewinder.com/2024/12/28/making-a-csharp-friendly-fsharp-library/), I went over one of the changes I made to my library, [Quipu](https://www.nuget.org/packages/Quipu), to make it more C# friendly. In this installment, I will go over another design change, turning the initial F# version, which used a classic pipeline, into a Fluent Interface.

For reference, here is how the original F# pipeline looks like:

```
<span>let</span> <span>f</span> <span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>=</span> <span>pown</span> <span>(</span><span>x</span> <span>-</span> <span>1</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>pown</span> <span>(</span><span>y</span> <span>-</span> <span>2</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>42</span><span>.</span><span>0</span>
<span>let</span> <span>solverResult</span> <span>=</span>
    <span>NelderMead</span><span>.</span><span>objective</span> <span>f</span>
    <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>0</span><span>.</span><span>000_0001</span>
    <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>startFrom</span> <span>(</span><span>Start</span><span>.</span><span>around</span> <span>(</span><span>100</span><span>.</span><span>0</span><span>;</span> <span>100</span><span>.</span><span>0</span><span>))</span>
    <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>minimize</span>
```

This _looks_ pretty similar to a Fluent Interface. It is not, though: it is a classic F# pipeline, chaining functions using the pipe-forward operator. From the F# side, this feels like a fluent interface, but for a C# consumer, it is more or less unusable.

Can we turn this into an actual C# friendly Fluent Interface? Yes we can, and this is what I will go over in this post.

## Fluent Interface, Take One

First, if we mimicked what the F# code does, how would a C# Fluent Interface look like? Probably something along these lines:

```
<span>Func</span><span>&lt;</span><span>Double</span><span>,</span><span>Double</span><span>,</span><span>Double</span><span>&gt;</span> <span>f</span> <span>=</span>
    <span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>=&gt;</span> <span>Math</span><span>.</span><span>Pow</span><span>(</span><span>x</span> <span>-</span> <span>1.0</span><span>,</span> <span>2</span><span>)</span> <span>+</span> <span>Math</span><span>.</span><span>Pow</span><span>(</span><span>y</span> <span>-</span> <span>2.0</span><span>,</span> <span>2</span><span>)</span> <span>+</span> <span>42.0</span><span>;</span>
<span>var</span> <span>solverResult</span> <span>=</span>
    <span>NelderMead</span>
        <span>.</span><span>Objective</span><span>(</span><span>f</span><span>)</span>
        <span>.</span><span>WithTolerance</span><span>(</span><span>0.0000001</span><span>)</span>
        <span>.</span><span>StartFrom</span><span>(</span><span>Start</span><span>.</span><span>Around</span><span>(</span><span>100.0</span><span>,</span> <span>100.0</span><span>))</span>
        <span>.</span><span>Minimize</span><span>();</span>
```

As it turns out, this _is_ exactly the current C# Quipu API. So how did we go from an F# pipeline to this?

Let’s start from the F# side, taking one of the pipeline steps for illustration:

```
<span>type</span> <span>NelderMead</span> <span>()</span> <span>=</span>
    <span>static</span> <span>member</span> <span>withTolerance</span> <span>(</span><span>tolerance</span><span>:</span> <span>float</span><span>)</span> <span>(</span><span>problem</span><span>:</span> <span>Problem</span><span>)</span> <span>=</span>
        <span>{</span> <span>problem</span> <span>with</span>
            <span>Configuration</span> <span>=</span> <span>{</span>
                <span>problem</span><span>.</span><span>Configuration</span> <span>with</span>
                    <span>Termination</span> <span>=</span> <span>Termination</span><span>.</span><span>tolerance</span> <span>tolerance</span>
                <span>}</span>
        <span>}</span>
```

[Source code](https://github.com/mathias-brandewinder/Quipu/blob/ee0c5be2815ac9131e57f7629cb2d891d76d2cb1/src/Quipu/NelderMead.fs#L79-L85)

> Sidebar: Why is `NelderMead` a class and not a module? And why is `withTolerance` a static method, and not a function on a module? I ended up using a class because, for other functions, I needed overloads.

The signature of the `withTolerance` function is

```
<span>withTolerance</span><span>:</span> <span>float</span> <span>-&gt;</span> <span>Problem</span> <span>-&gt;</span> <span>Problem</span>
```

If we already had an instance of a `Problem`, say, `initialProblem`, we could apply `withTolerance` using the [pipe-forward operator](https://learn.microsoft.com/en-us/dotnet/fsharp/language-reference/functions/#pipelines), which will return a new, updated `Problem`:

```
<span>initialProblem</span>
<span>|&gt;</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>0</span><span>.</span><span>000_001</span>
```

As long as we have functions that look along these lines:

```
<span>someProblemTransformation</span><span>:</span> <span>argument1</span> <span>-&gt;</span> <span>...</span> <span>-&gt;</span> <span>Problem</span> <span>-&gt;</span> <span>Problem</span>
```

… we can keep chaining them together, passing an initial `Problem` through a series of transformations that will eventually give us a `Problem`.

Stated differently, our original pipeline can be rewritten in the following equivalent code, fully expanding each step:

```
<span>let</span> <span>f</span> <span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>=</span> <span>pown</span> <span>(</span><span>x</span> <span>-</span> <span>1</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>pown</span> <span>(</span><span>y</span> <span>-</span> <span>2</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>42</span><span>.</span><span>0</span>
<span>let</span> <span>problem0</span> <span>=</span> <span>NelderMead</span><span>.</span><span>objective</span> <span>f</span>
<span>let</span> <span>problem1</span> <span>=</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>0</span><span>.</span><span>000_0001</span> <span>problem0</span>
<span>let</span> <span>problem2</span> <span>=</span> <span>NelderMead</span><span>.</span><span>startFrom</span> <span>(</span><span>Start</span><span>.</span><span>around</span> <span>[</span> <span>100</span><span>.</span><span>0</span><span>;</span> <span>100</span><span>.</span><span>0</span><span>])</span> <span>problem1</span>
<span>let</span> <span>solverResult</span> <span>=</span> <span>NelderMead</span><span>.</span><span>minimize</span> <span>problem2</span>
```

All we are doing is passing around a `Problem` and transforming it.

Can we do the same with C#? We can, by using essentially the same idea. All we need is a method on an instance, which returns a new instance of the same type. We could for instance add a method on the `Problem` type, and do something like this:

```
<span>type</span> <span>Problem</span> <span>=</span> <span>{</span>
    <span>// omitted for readability</span>
    <span>}</span>
    <span>with</span>
    <span>member</span> <span>this</span><span>.</span><span>WithTolerance</span> <span>(</span><span>tolerance</span><span>:</span> <span>float</span><span>):</span> <span>Problem</span> <span>=</span>
        <span>{</span> <span>this</span> <span>with</span>
            <span>// slightly simplified for readability</span>
            <span>Tolerance</span> <span>=</span> <span>tolerance</span>
        <span>}</span>
```

`WithTolerance` returns a `Problem`, so we can now chain the calls like so:

```
<span>var</span> <span>problem1</span> <span>=</span> <span>problem0</span><span>.</span><span>WithTolerance</span><span>(</span><span>0.001</span><span>);</span>
<span>var</span> <span>problem2</span> <span>=</span> <span>problem1</span><span>.</span><span>WithTolerance</span><span>(</span><span>0.01</span><span>);</span>
<span>var</span> <span>problem3</span> <span>=</span> <span>problem2</span><span>.</span><span>WithTolerance</span><span>(</span><span>0.1</span><span>);</span>
```

Or, omitting the intermediate variables, and calling `WithTolerance` directly on the `Problem` that the previous step returned:

```
<span>var</span> <span>problem3</span> <span>=</span>
    <span>problem0</span>
        <span>.</span><span>WithTolerance</span><span>(</span><span>0.001</span><span>)</span>
        <span>.</span><span>WithTolerance</span><span>(</span><span>0.01</span><span>)</span>
        <span>.</span><span>WithTolerance</span><span>(</span><span>0.1</span><span>);</span>
```

Of course, this example is a little absurd (you would not set the tolerance three times in a row, to three different values), but it illustrates the point. If we have methods on an instance that return an instance of the same type, we have a Fluent Interface.

## Fluent Interface, Take Two

While the general idea works, I wasn’t entirely satisfied with it. My issue was that the `Problem` type is not meant to be front-and-center. Leaving the type public is fine, so you can directly manipulate it in case you want to do something unusual, but by default you should not have to touch it.

In addition to this, `Problem` is a record which contains “non-obvious” types (`IVectorFunction`, `IStartingPoint`). Instantiating a `Problem` manually requires understanding how all these types work, and will be at best error prone and unpleasant (in particular for C#).

The F# pipeline completely hides `Problem` from the user, can we do something similar for C# consumers?

Ignoring for now how to instantiate a `Problem`, one approach would be to do something like this. Remove the `WithTolerance` method from `Problem`, and move it to the `NelderMead` class instead:

```
<span>// the constructor expects an instance of Problem</span>
<span>type</span> <span>NelderMead</span><span>(</span><span>problem</span><span>:</span> <span>Problem</span><span>)</span> <span>=</span>
    <span>member</span> <span>this</span><span>.</span><span>WithTolerance</span><span>(</span><span>tolerance</span><span>:</span> <span>float</span><span>)</span> <span>=</span>
        <span>// we update the Problem, using the existing F# method</span>
        <span>let</span> <span>updatedProblem</span> <span>=</span>
            <span>problem</span>
            <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>tolerance</span>
        <span>// and instantiate a new NelderMead, re-wrapping the updated Problem</span>
        <span>NelderMead</span><span>(</span><span>updatedProblem</span><span>)</span>
```

Instead of an empty constructor earlier, we expose a single constructor that expects an instance of a `Problem`. Because we want to chain method calls, we return a `NelderMead` from the `WithTolerance` method, so we can do the following:

```
<span>NelderMead</span><span>(</span><span>problem0</span><span>)</span>
    <span>.</span><span>WithTolerance</span><span>(</span><span>0.001</span><span>)</span>
    <span>.</span><span>WithTolerance</span><span>(...)</span>
```

We are still left with one problem, though. We need to pass a well-formed `Problem` in the constructor. How can we avoid that?

The F# pipeline gets around this by using factory methods. The first call in our original pipeline creates a `Problem`, using a static method `objective`:

```
<span>let</span> <span>f</span> <span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>=</span> <span>pown</span> <span>(</span><span>x</span> <span>-</span> <span>1</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>pown</span> <span>(</span><span>y</span> <span>-</span> <span>2</span><span>.</span><span>0</span><span>)</span> <span>2</span> <span>+</span> <span>42</span><span>.</span><span>0</span>
<span>let</span> <span>solverResult</span> <span>=</span>
    <span>NelderMead</span><span>.</span><span>objective</span> <span>f</span>
    <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>0</span><span>.</span><span>000_0001</span>
    <span>// more steps omitted</span>
```

We can easily achieve the same effect from the C# side, by hiding the default constructor, and exposing a similar factory method:

```
<span>type</span> <span>NelderMead</span> <span>private</span> <span>(</span><span>problem</span><span>:</span> <span>Problem</span><span>)</span> <span>=</span>

    <span>static</span> <span>member</span> <span>Objective</span><span>(</span><span>f</span><span>:</span> <span>System</span><span>.</span><span>Func</span><span>&lt;</span><span>float</span><span>,</span><span>float</span><span>,</span><span>float</span><span>&gt;)</span> <span>=</span>
        <span>NelderMead</span><span>.</span><span>objective</span> <span>f</span><span>.</span><span>Invoke</span>
        <span>|&gt;</span> <span>NelderMead</span>

    <span>member</span> <span>this</span><span>.</span><span>WithTolerance</span><span>(</span><span>tolerance</span><span>:</span> <span>float</span><span>)</span> <span>=</span>
        <span>problem</span>
        <span>|&gt;</span> <span>NelderMead</span><span>.</span><span>withTolerance</span> <span>tolerance</span>
        <span>|&gt;</span> <span>NelderMead</span>
```

Applying the same trick to the other steps of the pipeline leads to the following API:

```
<span>Func</span><span>&lt;</span><span>Double</span><span>,</span><span>Double</span><span>,</span><span>Double</span><span>&gt;</span> <span>f</span> <span>=</span>
    <span>(</span><span>x</span><span>,</span> <span>y</span><span>)</span> <span>=&gt;</span> <span>Math</span><span>.</span><span>Pow</span><span>(</span><span>x</span> <span>-</span> <span>1.0</span><span>,</span> <span>2</span><span>)</span> <span>+</span> <span>Math</span><span>.</span><span>Pow</span><span>(</span><span>y</span> <span>-</span> <span>2.0</span><span>,</span> <span>2</span><span>)</span> <span>+</span> <span>42.0</span><span>;</span>
<span>var</span> <span>solverResult</span> <span>=</span>
    <span>NelderMead</span>
        <span>.</span><span>Objective</span><span>(</span><span>f</span><span>)</span>
        <span>.</span><span>WithTolerance</span><span>(</span><span>0.0000001</span><span>)</span>
        <span>.</span><span>StartFrom</span><span>(</span><span>Start</span><span>.</span><span>Around</span><span>(</span><span>100.0</span><span>,</span> <span>100.0</span><span>))</span>
        <span>.</span><span>Minimize</span><span>();</span>
```

… which is a Fluent Interface that mimicks the original pipeline, but is also usable from the C# side.

## Parting thoughts

A couple of final comments before closing shop!

First, as of the [latest version (0.5.2)](https://github.com/mathias-brandewinder/Quipu/releases/tag/0.5.2), the F# and C# APIs are separated in 2 different namespaces, `Quipu` for F#, `Quipu.CSharp` for C#. I initially used a single class, as in this post, but as a result, the `NelderMead` type had a _lot_ of methods, mixing code formatting conventions (`withTolerance` and `WithTolerance` for example). I can see only one drawback to introducing this separation: you need to open the correct namespace depending on your preferred language. This seemed like an acceptable price to pay for the benefit of an uncluttered API.

Speaking of formatting, I also wanted the code to follow the expected standards for both F# and C#. I ended up using the `[<CompiledName>]` attribute on the `Start` type, like so:

```
<span>type</span> <span>Start</span> <span>=</span>
    <span>[&lt;</span><span>CompiledName</span><span>(</span><span>"Around"</span><span>)&gt;]</span>
    <span>static</span> <span>member</span> <span>around</span> <span>(</span><span>startingPoint</span><span>:</span> <span>seq</span><span>&lt;</span><span>float</span><span>&gt;)</span> <span>=</span>
        <span>// omitted code</span>
```

As a result, the code looks as you would expect in both languages:

```
<span>NelderMead</span><span>.</span><span>objective</span> <span>f</span>
<span>|&gt;</span> <span>NelderMead</span><span>.</span><span>startFrom</span> <span>(</span><span>Start</span><span>.</span><span>around</span> <span>(</span><span>100</span><span>.</span><span>0</span><span>;</span> <span>100</span><span>.</span><span>0</span><span>))</span>
```

```
<span>NelderMead</span>
    <span>.</span><span>Objective</span><span>(</span><span>f</span><span>)</span>
    <span>.</span><span>StartFrom</span><span>(</span><span>Start</span><span>.</span><span>Around</span><span>(</span><span>100.0</span><span>,</span> <span>100.0</span><span>))</span>
```

Finally, the goal was to design a C#-friendly API, and the best way to check if that goal is achieved is to experience potential painpoints yourself, by using your own code (aka dog-fooding). I ended up writing a battery of unit tests in C#, which is a simple but effective way to do that.

One thing I was wondering about is how I could also confirm API parity between the two versions. The thought here is that if I write a unit test in F#, it would be nice to automatically also run the same test, using the equivalent C# code. I am still mulling over that one, it is an interesting problem, but one I can leave to think about later :)

That’s what I got for today! I am still planning to make some changes to the library, but I expect these to be much less drastic than the recent ones. Anyways, [Quipu is usable as-is today](https://www.nuget.org/packages/Quipu) - if you have questions, feedback or requests, let me know! And in the meantime, I hope you found something of interest in this post.
