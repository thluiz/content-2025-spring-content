---
created: 2025-03-04T19:48:41 (UTC -03:00)
tags: []
source: https://sergeyteplyakov.github.io/Blog/production_investigations/2025/02/26/Finalizers_are_tricker_than_you_might_think_p1.html?utm_source=newsletter.csharpdigest.net&utm_medium=newsletter&utm_campaign=finalizers-are-tricker-than-you-think&_bhlid=9a9098718c6d68f8d1ac508da0d9e69a94476d19
author: Sergey Teplyakov
---

# Finalizers are tricker than you might think. Part 1 | Dissecting the Code

> ## Excerpt
> I was looking at some crash dumps recently and found one interesting case that I want to talk about here.

---
I was looking at some crash dumps recently and found one interesting case that I want to talk about here.

Let’s say we want to create a very simple file copier. It takes two paths in the constructor, opens two streams, and copies the content from one to another in the CopyAsync method:

```
<span>#</span><span>nullable</span> <span>enable</span>
<span>public</span> <span>sealed</span> <span>class</span> <span>FileCopier</span> <span>:</span> <span>IDisposable</span>
<span>{</span>
    <span>private</span> <span>readonly</span> <span>Stream</span> <span>_source</span><span>;</span>
    <span>private</span> <span>readonly</span> <span>Stream</span> <span>_destination</span><span>;</span>

    <span>public</span> <span>FileCopier</span><span>(</span><span>string</span> <span>sourcePath</span><span>,</span> <span>string</span> <span>destinationPath</span><span>)</span>
    <span>{</span>
        <span>_source</span> <span>=</span> <span>new</span> <span>FileStream</span><span>(</span><span>sourcePath</span><span>,</span> <span>FileMode</span><span>.</span><span>Open</span><span>);</span>
        <span>_destination</span> <span>=</span> <span>new</span> <span>FileStream</span><span>(</span><span>destinationPath</span><span>,</span> <span>FileMode</span><span>.</span><span>Create</span><span>);</span>
    <span>}</span>

    <span>public</span> <span>async</span> <span>Task</span> <span>CopyAsync</span><span>()</span>
        <span>=&gt;</span> <span>await</span> <span>_source</span><span>.</span><span>CopyToAsync</span><span>(</span><span>_destination</span><span>);</span>

    <span>public</span> <span>void</span> <span>Dispose</span><span>()</span> <span>=&gt;</span> <span>Dispose</span><span>(</span><span>disposing</span><span>:</span> <span>true</span><span>);</span>

    <span>~</span><span>FileCopier</span><span>()</span>
    <span>{</span>
        <span>Console</span><span>.</span><span>WriteLine</span><span>(</span><span>"Running ~FileCopier"</span><span>);</span>
        <span>Dispose</span><span>(</span><span>disposing</span><span>:</span> <span>false</span><span>);</span>
    <span>}</span>

    <span>private</span> <span>void</span> <span>Dispose</span><span>(</span><span>bool</span> <span>disposing</span><span>)</span>
    <span>{</span>
        <span>_source</span><span>.</span><span>Dispose</span><span>();</span>
        <span>_destination</span><span>.</span><span>Dispose</span><span>();</span>
    <span>}</span>
<span>}</span>
```

The class implements the `IDisposable` interface and has a finalizer to make sure the resources are freed if the `Dispose` method is not called. Here is how we could use it:

```
<span>private</span> <span>static</span> <span>async</span> <span>Task</span> <span>Copy</span><span>(</span><span>string</span> <span>source</span><span>,</span> <span>string</span> <span>destination</span><span>)</span>
<span>{</span>
    <span>using</span> <span>(</span><span>var</span> <span>copier</span> <span>=</span> <span>new</span> <span>FileCopier</span><span>(</span><span>source</span><span>,</span> <span>destination</span><span>))</span>
    <span>{</span>
        <span>await</span> <span>copier</span><span>.</span><span>CopyAsync</span><span>();</span>
    <span>}</span>
<span>}</span>

<span>static</span> <span>async</span> <span>Task</span> <span>Main</span><span>(</span><span>string</span><span>[]</span> <span>args</span><span>)</span>
<span>{</span>
    <span>string</span> <span>source</span> <span>=</span> <span>"source.txt"</span><span>;</span>
    <span>string</span> <span>destination</span> <span>=</span> <span>"destination.txt"</span><span>;</span>
    
    <span>try</span>
    <span>{</span>
        <span>await</span> <span>Copy</span><span>(</span><span>source</span><span>,</span> <span>destination</span><span>);</span>
        <span>Console</span><span>.</span><span>WriteLine</span><span>(</span><span>"Copy is done!"</span><span>);</span>
    <span>}</span>
    <span>catch</span><span>(</span><span>Exception</span> <span>ex</span><span>)</span>
    <span>{</span>
        <span>Console</span><span>.</span><span>WriteLine</span><span>(</span><span>$"Error: </span><span>{</span><span>ex</span><span>.</span><span>Message</span><span>}</span><span>"</span><span>);</span>
        <span>GC</span><span>.</span><span>Collect</span><span>();</span>
        <span>GC</span><span>.</span><span>Collect</span><span>();</span>
    <span>}</span>
<span>}</span>
```

And here is the output:

```
Error: Could not find file 'C:\Users\seteplia\source\repos\DisposableTrickiness\DisposableTrickiness\bin\Debug\net9.0\source.txt'.
Press 'Enter' to exit.
Running ~FileCopier
Unhandled exception. System.NullReferenceException: Object reference not set to an instance of an object.
   at DisposableTrickiness.FileCopier.Dispose(Boolean disposing) in C:\Users\seteplia\source\repos\DisposableTrickiness\DisposableTrickiness\Program.cs:line 35
   at DisposableTrickiness.FileCopier.Finalize() in C:\Users\seteplia\source\repos\DisposableTrickiness\DisposableTrickiness\Program.cs:line 30
   at System.GC.RunFinalizers()

```

The application prints that the source file is missing, but then it crashes with a NullReferenceException. The call stack shows that the `NullReferenceException` is coming from the finalizer. WAT?!?

Let’s dive deeply into how finalizers work and why they’re probably trickier than you might think!

## Why the application crashes?

Finalizers are executed in a dedicated thread controlled by the Garbage Collector. Like a regular thread, an unhandled exception from the finalizer thread causes the application to crash.

Since there is a single dedicated thread to run all the finalizers, it’s also not a great idea to make blocking calls from it. I don’t want to even think about what “strange” things would happen in a long-running application when its finalizer thread is blocked. Just don’t call arbitrary code from finalizers, and don’t allow exceptions to escape from them.

## Why `NullReferenceException` when non-nullability is enabled?

[Nullable reference types](https://learn.microsoft.com/en-us/dotnet/csharp/nullable-references) is purely a compile-time feature with some known restrictions, and finalizers are one of them.

Finalizers are executed for every object regardless of whether their constructor finished successfully or not. Some people with a C++ background might think that finalizers are similar to C++ destructors, which are executed only for fully constructed instances. But unlike C++ destructors, finalizers will be executed even if the constructor fails with an exception.

This means that from the C# compiler’s point of view, the `_source` and `_destination` fields are non-nullable, but if the constructor fails before the fields are assigned, they’ll be null during finalization.

## Should you even touch `_source` and `_destination` from the finalizer?

The solution here is not to use `?.` like `_source?.Dispose();` and `_destination?.Dispose();`, but not to touch “managed resources” from the finalizer in the first place.

There are two reasons for that:

1.  The order of finalization is non-deterministic.
2.  Finalizers are designed for cleaning up “unmanaged” resources, and Stream is a “managed resource”.

Let’s clarify these points.

Again, if you have a C++ background, you might think that the order of finalization is the opposite of the construction order. But this is not the case. The CLR doesn’t track the object’s dependency chain and construction order; it just registers all instances in a global finalization queue. The queue has special treatment for “critical finalizable” objects (when a class derives from `CriticalFinalizerObject`) and provides a guarantee that finalization for “normal” objects happens before the finalization of “critical” objects. But there is no guarantee in which order finalization happens within the normal or critical finalization segment of the queue.

If object A references object B, the finalization for object A might happen before or after the finalization of object B. You just don’t have control over it.

But the second aspect explains why you should not be touching “managed” resources from finalizers in the first place: finalizers are designed to clean up “unmanaged” resources only!

## What’s the difference between “managed” and “unmanaged” resources?

In some cases, the terms are important, and understanding the concepts of “managed” and “unmanaged” resources is crucial in understanding how to deal with resources properly in .NET.

**TLDR; if it’s a disposable class - it’s a “managed resource”, if it’s `IntPtr` (or something similar) then it’s an “unmanaged resource”. Wrap `IntPtr` into a disposable class with a finalizer, and you’ll get a managed resource!**

The CLR automatically “manages” memory: when an instance “goes out of scope” and is not reachable from the application code, it becomes eligible for garbage collection. When the GC runs, the memory used by the object is reclaimed. There is quite a bit of complexity behind this, but all that complexity is built to deal with memory.

The CLR can’t automatically manage other resources. If a resource is allocated in an unmanaged heap via malloc or an opaque handler was obtained from the operating system, then the CLR can’t automatically free them when they become unreachable. The runtime needs help from the developer.

To make the resource “managed”, the underlying “unmanaged” resource needs to be wrapped in a class that implements the `IDisposable` interface for eager resource clean-up and should also have a finalizer for cleaning up the resource when the user forgets to clean it up and the instance is collected by the GC.

```
<span>// ManagedWrapper itself is a managed resource</span>
<span>public</span> <span>class</span> <span>ManagedWrapper</span> <span>:</span> <span>IDisposable</span>
<span>{</span>
    <span>// IntPtr represents an unmanaged resource.</span>
    <span>private</span> <span>readonly</span> <span>IntPtr</span> <span>_resource</span><span>;</span>
    <span>public</span> <span>ManagedWrapper</span><span>()</span>
    <span>{</span>
        <span>_resource</span> <span>=</span> <span>Allocate</span><span>();</span> <span>// PInvoke to allocate a resource</span>
    <span>}</span>
    
    <span>public</span> <span>void</span> <span>Dispose</span><span>()</span>
    <span>{</span>
        <span>Free</span><span>(</span><span>_resource</span><span>);</span> <span>// PInvoke to free a resource</span>
        <span>GC</span><span>.</span><span>SuppressFinalize</span><span>(</span><span>this</span><span>);</span>
    <span>}</span>
    
    <span>~</span><span>ManagedWrapper</span><span>()</span> <span>=&gt;</span> <span>Free</span><span>(</span><span>_resource</span><span>);</span>
<span>}</span>
```

In this case, `IntPtr _resource` is an unmanaged resource, and the instance of `ManagedWrapper` is a managed resource.

Don’t be surprised that you don’t see `Dispose(bool disposing)` here. We’re going to cover the “Dispose Pattern” in more detail in future posts.

## Conclusion

-   Unhandled exceptions from finalizers will cause an application to crash.
-   Nullable fields might be null in finalizers when the constructor throws an exception.
-   Finalizers should not touch managed resources. They’re designed to clean up unmanaged resources only.
-   Classes without unmanaged resources should not have finalizers at all.
-   The order of finalization is not guaranteed.
