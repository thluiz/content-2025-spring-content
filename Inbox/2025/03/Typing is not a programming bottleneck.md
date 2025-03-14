---
created: 2025-03-05T14:04:54 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/
author: Mark Seemann
---

# Typing is not a programming bottleneck

> ## Excerpt
> Some developers seem to think that typing is a major bottleneck while programming. It's not.

---
_Some developers seem to think that typing is a major bottleneck while programming. It's not._

I sometimes give programming advice to people. They approach me with a software design problem, and, to the best of my ability, I suggest a remedy. Despite my best intentions, my suggestions sometimes meet resistance. One common reaction is that [my suggestion isn't idiomatic](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic), but recently, another type of criticism seems to be on the rise.

The code that I suggest is too verbose. It involves too much typing.

I'll use this article to reflect on that criticism.

### The purpose of code [#](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/#d1f8398f96584ed09acce1b481b45cde "permalink")

Before we get into the details of the issue, I'd like to start with the big picture. What's the purpose of code?

I've discussed this extensively in my [Clean Coders](https://cleancoders.com/) video on [Humane Code](https://cleancoders.com/episode/humane-code-real-episode-1/show). In short, the purpose of source code is to _communicate_ the workings of a piece of software to the next programmer who comes along. This could easily include your future self.

Perhaps you disagree with that proposition. Perhaps you think that the purpose of source code is to produce working software. It's that, too, but that's not its only purpose. If that was the only purpose, you might as well write the software in machine code.

Why do we have high-level programming languages like C#, Java, JavaScript, Ruby, Python, F#, Visual Basic, Haskell, heck - even C++?

As far as I can tell, it's because programmers are all human, and humans have limited cognitive capacity. We can't keep track of hundreds, or thousands, of global variables, or the states of dozens of complex resources. We need tools that help us structure and understand complex systems so that they're broken down into (more) manageable chunks. High-level languages help us do that.

The purpose of source code is to be understood. You read source code much more that you write. I'm not aware of any scientific studies, but I think most programmers will agree that over the lifetime of a code base, any line of code will be read orders of magnitude more often that it's edited.

### Typing speed [#](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/#b82afa298f114b8c89113ea5f40fc278 "permalink")

How many lines of code do you produce during a productive working day?

To be honest, I can't even answer that question myself. I've never measured it, since I consider it to be irrelevant. [The Mythical Man-Month](http://bit.ly/mythical-man-month) gives the number as _10_, but let's be generous and pretend it's ten times that. This clearly depends on lots of factors, such as the language in which you're writing, the state of the code base, and so on. You'd tend to write more lines in a pristine greenfield code base, whereas you'll write fewer lines of code in a complicated legacy code base.

How many characters is a line of code? Let's say it's 80 characters, because [that's the maximum width code ought to have](https://blog.ploeh.dk/2019/11/04/the-80-24-rule). I realise that many people write wider lines, but on the other hand, most developers (fortunately) use indentation, so as a counter-weight, code often has some blank space to the left as well. This is all back-of-the-envelope calculations anyway.

When I worked in a Microsoft product group, we typically planned that a productive, 'full' day of coding was five hours. Even on productive days, the rest was used in meetings, administration, breaks, and so on.

If you write code for five hours, and produce 100 lines of code, at 80 characters per line, that's 8,000 characters. Your IDE is likely to help you with statement completion and such, but for the sake of argument, let's pretend that you have to type it all in.

8,000 characters in five hours is 1,600 characters per hour, or 27 characters per minute.

I'm not a particularly fast typist, but I can type ten times faster than that.

Typing isn't a bottleneck.

### Is more code worse? [#](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/#cad0abab133848d98b4aada5e31d35d6 "permalink")

I tend to [get drawn into these discussions from time to time](https://blog.ploeh.dk/2013/02/04/BewareofProductivityTools), but good programming has little to do with how fast you can produce lines of code.

To be clear, I entirely accept that statement completion, refactoring support, and such are, in general, benign features. While I spend most of my programming time thinking and reading, I also tend to write code in bursts. The average count of lines per hour may not be great, but that's because averages smooth out the hills and the valleys of my activity.

Still, increasingly frequent objection to some of my suggestions is that what I suggest implies 'too much' code. Recently, for example, I had to defend the merits of the [Fluent](https://martinfowler.com/bliki/FluentInterface.html) Builder pattern that I suggest in my [DI-Friendly Library](https://blog.ploeh.dk/2014/05/19/di-friendly-library) article.

As another example, consider two alternatives for modelling a restaurant reservation. First, here's a terse option:

```
<span>public</span>&nbsp;<span>class</span>&nbsp;<span>Reservation</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>DateTimeOffset</span>&nbsp;Date&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>&nbsp;Email&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>&nbsp;Name&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Quantity&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>bool</span>&nbsp;IsAccepted&nbsp;{&nbsp;<span>get</span>;&nbsp;<span>set</span>;&nbsp;}
}
```

Here's a longer alternative:

```
<span>public</span>&nbsp;<span>sealed</span>&nbsp;<span>class</span>&nbsp;<span>Reservation</span>
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;Reservation(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>DateTimeOffset</span>&nbsp;date,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>string</span>&nbsp;name,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>string</span>&nbsp;email,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>int</span>&nbsp;quantity)&nbsp;:
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>this</span>(date,&nbsp;name,&nbsp;email,&nbsp;quantity,&nbsp;<span>false</span>)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>private</span>&nbsp;Reservation(
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>DateTimeOffset</span>&nbsp;date,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>string</span>&nbsp;name,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>string</span>&nbsp;email,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>int</span>&nbsp;quantity,
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>bool</span>&nbsp;isAccepted)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Date&nbsp;=&nbsp;date;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name&nbsp;=&nbsp;name;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Email&nbsp;=&nbsp;email;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Quantity&nbsp;=&nbsp;quantity;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsAccepted&nbsp;=&nbsp;isAccepted;
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>DateTimeOffset</span>&nbsp;Date&nbsp;{&nbsp;<span>get</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>&nbsp;Name&nbsp;{&nbsp;<span>get</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>string</span>&nbsp;Email&nbsp;{&nbsp;<span>get</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>int</span>&nbsp;Quantity&nbsp;{&nbsp;<span>get</span>;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>bool</span>&nbsp;IsAccepted&nbsp;{&nbsp;<span>get</span>;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Reservation</span>&nbsp;WithDate(<span>DateTimeOffset</span>&nbsp;newDate)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(newDate,&nbsp;Name,&nbsp;Email,&nbsp;Quantity,&nbsp;IsAccepted);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Reservation</span>&nbsp;WithName(<span>string</span>&nbsp;newName)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(Date,&nbsp;newName,&nbsp;Email,&nbsp;Quantity,&nbsp;IsAccepted);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Reservation</span>&nbsp;WithEmail(<span>string</span>&nbsp;newEmail)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(Date,&nbsp;Name,&nbsp;newEmail,&nbsp;Quantity,&nbsp;IsAccepted);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Reservation</span>&nbsp;WithQuantity(<span>int</span>&nbsp;newQuantity)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(Date,&nbsp;Name,&nbsp;Email,&nbsp;newQuantity,&nbsp;IsAccepted);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>Reservation</span>&nbsp;Accept()
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(Date,&nbsp;Name,&nbsp;Email,&nbsp;Quantity,&nbsp;<span>true</span>);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>override</span>&nbsp;<span>bool</span>&nbsp;Equals(<span>object</span>&nbsp;obj)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(!(obj&nbsp;<span>is</span>&nbsp;<span>Reservation</span>&nbsp;other))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>false</span>;
 
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;Equals(Date,&nbsp;other.Date)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&amp;&amp;&nbsp;Equals(Name,&nbsp;other.Name)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&amp;&amp;&nbsp;Equals(Email,&nbsp;other.Email)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&amp;&amp;&nbsp;Equals(Quantity,&nbsp;other.Quantity)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&amp;&amp;&nbsp;Equals(IsAccepted,&nbsp;other.IsAccepted);
&nbsp;&nbsp;&nbsp;&nbsp;}
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>public</span>&nbsp;<span>override</span>&nbsp;<span>int</span>&nbsp;GetHashCode()
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Date.GetHashCode()&nbsp;^
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Name.GetHashCode()&nbsp;^
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Email.GetHashCode()&nbsp;^
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Quantity.GetHashCode()&nbsp;^
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;IsAccepted.GetHashCode();
&nbsp;&nbsp;&nbsp;&nbsp;}
}
```

Which alternative is better? The short version is eight lines of code, including the curly brackets. The longer version is 78 lines of code. That's ten times as much.

I prefer the longer version. While it takes longer to type, it comes with several benefits. The main benefit is that because it's immutable, it can have structural equality. This makes it trivial to compare objects, which, for example, is something you do all the time in unit test assertions. Another benefit is that such [Value Objects make better domain models](https://blog.ploeh.dk/2015/01/19/from-primitive-obsession-to-domain-modelling). The above `Reservation` [Value Object](https://martinfowler.com/bliki/ValueObject.html) only shows the slightest sign of emerging domain logic in the `Accept` method, but once you start modelling like this, such objects seem to attract more domain behaviour.

### Maintenance burden [#](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/#78654bcab6ee4c57a24a5a41adf6c0fa "permalink")

Perhaps you're now convinced that typing speed may not be the bottleneck, but you still feel that you don't like the verbose `Reservation` alternative. More code could be an increased maintenance burden.

Consider those `With[...]` methods, such as `WithName`, `WithQuantity`, and so on. Once you make objects immutable, such copy-and-update methods become indispensable. They enable you to change a single property of an object, while keeping all other values intact:

```
&gt; <span>var</span>&nbsp;r&nbsp;=&nbsp;<span>new</span>&nbsp;<span>Reservation</span>(<span>DateTimeOffset</span>.Now,&nbsp;<span>"Foo"</span>,&nbsp;<span>"foo@example.com"</span>,&nbsp;3);
&gt; r.WithQuantity(4)
Reservation { Date=[11.09.2018 19:19:29 +02:00],
              Email="foo@example.com",
              IsAccepted=false,
              Name="Foo",
              Quantity=4 }
```

While convenient, such methods can increase the maintenance burden. If you realise that you need to change the name of one of the properties, you'll have to remember to also change the name of the copy-and-update method. For example, if you change `Quantity` to `NumberOfGuests`, you'll have to also remember to rename `WithQuantity` to `WithNumberOfGuests`.

I'm not sure that I'm ready to concede that this is a prohibitive strain on the sustainability of a code base, but I do grant that it's a nuisance. This is one of the many reasons that I prefer to use programming languages better equipped for such domain modelling. In [F#](https://fsharp.org/), for example, a record type similar to the above immutable `Reservation` class would be a one-liner:

```
<span>type</span>&nbsp;Reservation&nbsp;=
&nbsp;&nbsp;&nbsp;&nbsp;{&nbsp;Date&nbsp;:&nbsp;DateTimeOffset;&nbsp;Name&nbsp;:&nbsp;string;&nbsp;Email&nbsp;:&nbsp;string;&nbsp;Quantity&nbsp;:&nbsp;int;&nbsp;IsAccepted&nbsp;:&nbsp;bool&nbsp;}
```

Such a declarative approach to types produces an immutable record with the same capabilities as the 78 lines of C# code.

That's a different story, though. There's little correlation between the size of code, and how 'good' it is. Sometimes, less code is better; sometimes, more code is better.

### Summary [#](https://blog.ploeh.dk/2018/09/17/typing-is-not-a-programming-bottleneck/#ec55edfbe2a245b0b56188d4e8c6fbfb "permalink")

I'll dispense with the usual Edsger Dijkstra and Bill Gates quotes on lines of code. The point that lines of code is a useless metric in software development has been made already. My point is a corollary. Apparently, it has to be explicitly stated: _Programmer productivity has nothing to do with typing speed._

Unless you're disabled in some way, you can type fast enough to be a productive programmer. Typing isn't a programming bottleneck.
