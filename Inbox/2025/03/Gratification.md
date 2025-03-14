---
created: 2025-03-05T14:05:35 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2024/05/13/gratification/
author: Mark Seemann
---

# Gratification

> ## Excerpt
> Some thoughts on developer experience.

---
_Some thoughts on developer experience._

Years ago, I was introduced to a concept called _developer ergonomics_. Despite the name, it's not about good chairs, standing desks, or multiple monitors. Rather, the concept was related to how easy it'd be for a developer to achieve a certain outcome. How easy is it to set up a new code base in a particular language? How much work is required to save a row in a database? How hard is it to read rows from a database and display the data on a web page? And so on.

These days, we tend to discuss _developer experience_ rather than ergonomics, and that's probably a good thing. This term more immediately conveys what it's about.

I've recently had some discussions about developer experience (DevEx, DX) with one of my customers, and this has lead me to reflect more explicitly on this topic than previously. Most of what I'm going to write here are opinions and beliefs that go back a long time, but apparently, it's only recently that these notions have congealed in my mind under the category name _developer experience_.

This article may look like your usual old-man-yells-at-cloud article, but I hope that I can avoid that. It's not the case that I yearn for some lost past where 'we' wrote [Plankalkül](https://en.wikipedia.org/wiki/Plankalk%C3%BCl) in [Edlin](https://en.wikipedia.org/wiki/Edlin). That, in fact, sounds like a horrible developer experience.

The point, rather, is that most attractive things come with consequences. For anyone who have been reading this blog even once in a while, this should come as no surprise.

### Instant gratification [#](https://blog.ploeh.dk/2024/05/13/gratification/#cbc9752f754e40cc94267689f5dd87bf)

Fat foods, cakes, and wine can be wonderful, but can be detrimental to your health if you overindulge. It can, however, be hard to resist a piece of chocolate, and even if we think that we shouldn't, we often fail to restrain ourselves. The temptation of instant gratification is simply too great.

There are other examples like this. The most obvious are the use of narcotics, lack of exercise, smoking, and dropping out of school. It may feel good in the moment, but can have long-term consequences.

Small children are notoriously bad at delaying gratification, and we often associate the ability to delay gratification with maturity. We all, however, fall in from time to time. Food and wine are my weak spots, while I don't do drugs, and I didn't drop out of school.

It strikes me that we often talk about ideas related to developer experience in a way where we treat developers as children. To be fair, many developers also act like children. I don't know how many times I've <ins datetime="2024-06-17T08:26Z">heard</ins> something like, _"I don't want to write tests/go through a code review/refactor! I just want to ship working code now!"_

Fine, so do I.

Even if wine is bad for me, it makes life worth living. As the saying goes, even if you don't smoke, don't drink, exercise rigorously, eat healthily, don't do drugs, and don't engage in dangerous activities, you're not guaranteed to live until ninety, but you're guaranteed that it's going to _feel_ that long.

Likewise, I'm aware that doing everything right can sometimes take so long that by the time we've deployed the software, it's too late. The point isn't to always or never do certain things, but rather to be aware of the consequences of our choices.

### Developer experience [#](https://blog.ploeh.dk/2024/05/13/gratification/#ac2969093f264da092186fa0cb7196e5)

I've no problem with aiming to make the experience of writing software as good as possible. Some developer-experience thought leaders talk about the importance of documentation, predictability, and timeliness. Neither do I mind that a development environment looks good, completes my words, or helps me refactor.

To return to the analogy of human vices, not everything that feels good is ultimately bad for you. While I do like wine and chocolate, I also love [sushi](https://en.wikipedia.org/wiki/Sushi), white [asparagus](https://en.wikipedia.org/wiki/Asparagus), [turbot](https://en.wikipedia.org/wiki/Turbot), [chanterelles](https://en.wikipedia.org/wiki/Chanterelle), [lumpfish](https://en.wikipedia.org/wiki/Cyclopterus) roe [caviar](https://en.wikipedia.org/wiki/Caviar), [true morels](https://en.wikipedia.org/wiki/Morchella), [Norway lobster](https://en.wikipedia.org/wiki/Nephrops_norvegicus), and various other foods that tend to be categorized as healthy.

A good [IDE](https://en.wikipedia.org/wiki/Integrated_development_environment) with refactoring support, statement completion, type information, test runner, etc. is certainly preferable to writing all code in [Notepad](https://en.wikipedia.org/wiki/Windows_Notepad).

That said, there's a certain kind of developer tooling and language features that strikes me as more akin to candy. These are typically tools and technologies that tend to demo well. Recent examples include [OpenAPI](https://www.openapis.org/), [GitHub Copilot](https://github.com/features/copilot), [C# top-level statements](https://learn.microsoft.com/dotnet/csharp/fundamentals/program-structure/top-level-statements), code generation, and [Postman](https://www.postman.com/). Not all of these are unequivocally bad, but they strike me as mostly aiming at immature developers.

The point of this article isn't to single out these particular products, standards, or language features, but on the other hand, in order to make a point, I do have to at least outline why I find them problematic. They're just examples, and I hope that by explaining what is on my mind, you can see the pattern and apply it elsewhere.

### OpenAPI [#](https://blog.ploeh.dk/2024/05/13/gratification/#f7f676bf5a334b189b3c2baab18b1e6a)

A standard like OpenAPI, for example, looks attractive because it automates or standardizes much work related to developing and maintaining [REST APIs](https://en.wikipedia.org/wiki/REST). Frameworks and tools that leverage that standard automatically creates machine-readable [schema and contract](https://blog.ploeh.dk/2024/04/15/services-share-schema-and-contract-not-class), which can be used to generate client code. Furthermore, an OpenAPI-aware framework can also autogenerate an entire web-based graphical user interface, which developers can use for ad-hoc testing.

I've worked with clients who also published these OpenAPI user interfaces to their customers, so that it was easy to get started with the APIs. Easy onboarding.

Instant gratification.

What's the problem with this? There are clearly enough apparent benefits that I usually have a hard time talking my clients out of pursuing this strategy. What are the disadvantages? Essentially, OpenAPI locks you into [level 2](https://martinfowler.com/articles/richardsonMaturityModel.html) APIs. No hypermedia controls, no [smooth conneg-based versioning](https://blog.ploeh.dk/2015/06/22/rest-implies-content-negotiation), no [HATEOAS](https://en.wikipedia.org/wiki/HATEOAS). In fact, most of what makes REST flexible is lost. What remains is an ad-hoc, informally-specified, bug-ridden, slow implementation of half of [SOAP](https://en.wikipedia.org/wiki/SOAP).

I've [previously described my misgivings about Copilot](https://blog.ploeh.dk/2022/12/05/github-copilot-preliminary-experience-report), and while I actually still use it, I don't want to repeat all of that here. Let's move on to another example.

### Top-level statements [#](https://blog.ploeh.dk/2024/05/13/gratification/#f56e835825464650a86c557c7253f095)

Among many other language features, C# 9 got _top-level-statements_. This means that you don't need to write a `Main` method in a static class. Rather, you can have a single C# code file where you can immediately start executing code.

It's not that I consider this language feature particularly harmful, but it also solves what seems to me a non-problem. It demos well, though. If I understand the motivation right, the feature exists because 'modern' developers are used to languages like [Python](https://www.python.org/) where you can, indeed, just create a `.py` file and start adding code statements.

In an attempt to make C# more attractive to such an audience, it, too, got that kind of developer experience enabled.

You may argue that this is a bid to remove some of the ceremony from the language, but I'm not convinced that this moves that needle much. The [level of ceremony that a language like C# has is much deeper than that](https://blog.ploeh.dk/2019/12/16/zone-of-ceremony). That's not to target C# in particular. [Java](https://www.java.com/) is similar, and don't even get me started on [C](https://en.wikipedia.org/wiki/C_(programming_language)) or [C++](https://en.wikipedia.org/wiki/C%2B%2B)! Did anyone say _header files?_

Do 'modern' developers choose Python over C# because they can't be arsed to write a `Main` method? If that's the _only_ reason, it strikes me as incredibly immature. _I want instant gratification, and writing a `Main` method is just too much trouble!_

If developers do, indeed, choose Python or JavaScript over C# and Java, I hope and believe that it's for other reasons.

This particular C# feature doesn't bother me, but I find it symptomatic of a kind of 'innovation' where language designers target instant gratification.

### Postman [#](https://blog.ploeh.dk/2024/05/13/gratification/#b9ce02aa90074838bd7b8e2cec0189e2)

Let's consider one more example. You may think that I'm now attacking a company that, for all I know, makes a decent product. I don't really care about that, though. What I do care about is the developer mentality that makes a particular tool so ubiquitous.

I've met web service developers who would be unable to interact with the HTTP APIs that they are themselves developing if they didn't have Postman. Likewise, there are innumerable questions on [Stack Overflow](https://stackoverflow.com/) where people ask questions about HTTP APIs and post screen shots of Postman sessions.

It's okay if you don't know how to interact with an HTTP API. After all, there's a first time for everything, and there was a time when I didn't know how to do this either. Apparently, however, it's easier to install an application with a graphical user interface than it is to use [curl](https://curl.se/).

Do yourself a favour and learn curl instead of using Postman. Curl is a command-line tool, which means that you can use it for both ad-hoc experimentation and automation. It takes five to ten minutes to learn the basics. It's also free.

It still seems to me that many people are of a mind that it's easier to use Postman than to learn curl. Ultimately, I'd wager that for any task you do with some regularity, it's more productive to learn the text-based tool than the point-and-click tool. In a situation like this, I'd suggest that delayed gratification beats instant gratification.

### CV-driven development [#](https://blog.ploeh.dk/2024/05/13/gratification/#50ed56effb784c95a6f6de4967e883ef)

It is, perhaps, easy to get the wrong impression from the above examples. I'm not pointing fingers at just any 'cool' new technology. There are techniques, languages, frameworks, and so on, which people pick up because they're exciting for other reasons. Often, such technologies solve real problems in their niches, but are then applied for the sole reason that people want to get on the bandwagon. Examples include [Kubernetes](https://kubernetes.io/), mocks, [DI Containers](https://blog.ploeh.dk/2012/11/06/WhentouseaDIContainer), [reflection](https://blog.ploeh.dk/2023/12/04/serialization-with-and-without-reflection), [AOP](https://en.wikipedia.org/wiki/Aspect-oriented_programming), and [microservices](https://en.wikipedia.org/wiki/Microservices). All of these have legitimate applications, but we also hear about many examples where people use them just to use them.

That's a different problem from the one I'm discussing in this article. Usually, learning about such advanced techniques requires delaying gratification. There's nothing wrong with learning new skills, but part of that process is also gaining the understanding of when to apply the skill, and when not to. That's a different discussion.

### Innovation is fine [#](https://blog.ploeh.dk/2024/05/13/gratification/#10cc039f39ba4c2caab34f66f17f90b2)

The point of this article isn't that every innovation is bad. Contrary to [Charles Petzold](https://www.charlespetzold.com/), I don't really believe that Visual Studio rots the mind, although I once did publish [an article](https://blog.ploeh.dk/2013/02/04/BewareofProductivityTools) that navigated the same waters.

Despite my misgivings, I haven't uninstalled GitHub Copilot, and I do enjoy many of the features in both Visual Studio (VS) and Visual Studio Code (VS Code). I also welcome and use many new language features in various languages.

I can certainly appreciate how an IDE makes many things easier. Every time I have to begin a new [Haskell](https://www.haskell.org/) code base, I long for the hand-holding offered by Visual Studio when creating a new C# project.

And although I don't use the debugger much, the built-in debuggers in VS and VS Code sure beat [GDB](https://en.wikipedia.org/wiki/GNU_Debugger). It even works in Python!

There's even tooling that [I wish for](https://developercommunity.visualstudio.com/t/Test-Explorer:-Better-support-for-TDD-wo/701822), but apparently never will get.

### Simple made easy [#](https://blog.ploeh.dk/2024/05/13/gratification/#675450a5c0cf441fa433b928251de8a5)

In [Simple Made Easy](https://www.infoq.com/presentations/Simple-Made-Easy/) Rich Hickey follows his usual look-up-a-word-in-the-dictionary-and-build-a-talk-around-the-definition style to contrast _simple_ with _easy_. I find his distinction useful. A tool or technique that's _close at hand_ is _easy_. This certainly includes many of the above instant-gratification examples.

An _easy_ technique is not, however, necessarily _simple_. It may or may not be. [Rich Hickey](https://en.wikipedia.org/wiki/Rich_Hickey) defines _simple_ as the opposite of _complex_. Something that is complex is assembled from parts, whereas a simple thing is, ideally, single and undivisible. In practice, truly simple ideas and tools may not be available, and instead we may have to settle with things that are less complex than their alternatives.

Once you start looking for things that make simple things easy, you see them in many places. A big category that I personally favour contains all the language features and tools that make functional programming (FP) easier. FP tends to be simpler than object-oriented or procedural programming, because it [explicitly distinguishes between and separates](https://blog.ploeh.dk/2018/11/19/functional-architecture-a-definition) predictable code from unpredictable code. This does, however, in itself tend to make some programming tasks harder. How do you generate a random number? Look up the system time? Write a record to a database?

Several FP languages have special features that make even those difficult tasks easy. [F#](https://fsharp.org/) has [computation expressions](https://learn.microsoft.com/dotnet/fsharp/language-reference/computation-expressions) and [Haskell](https://www.haskell.org/) has [do notation](https://en.wikibooks.org/wiki/Haskell/do_notation).

Let's say you want to call a function that consumes a random number generator. In Haskell (as in .NET) random number generators are actually deterministic, as long as you give them the same seed. Generating a random seed, on the other hand, is non-deterministic, so has to happen in [IO](https://blog.ploeh.dk/2020/06/08/the-io-container).

Without `do` notation, you could write the action like this:

```
<span>rndSelect</span>&nbsp;<span>::</span>&nbsp;<span>Integral</span>&nbsp;i&nbsp;<span>=&gt;</span>&nbsp;[a]&nbsp;<span>-&gt;</span>&nbsp;i&nbsp;<span>-&gt;</span>&nbsp;<span>IO</span>&nbsp;[a]
rndSelect&nbsp;xs&nbsp;count&nbsp;=&nbsp;(\rnd&nbsp;-&gt;&nbsp;rndGenSelect&nbsp;rnd&nbsp;xs&nbsp;count)&nbsp;&lt;$&gt;&nbsp;newStdGen
```

(The type annotation is optional.) While terse, this is hardly readable, and the developer experience also leaves something to be desired. Fortunately, however, you can [rewrite this action](https://blog.ploeh.dk/2018/07/09/typing-and-testing-problem-23) with `do` notation, like this:

```
<span>rndSelect</span>&nbsp;::&nbsp;<span>Integral</span>&nbsp;i&nbsp;<span>=&gt;</span>&nbsp;[a]&nbsp;<span>-&gt;</span>&nbsp;i&nbsp;<span>-&gt;</span>&nbsp;<span>IO</span>&nbsp;[a]
rndSelect&nbsp;xs&nbsp;count&nbsp;=&nbsp;<span>do</span>
&nbsp;&nbsp;rnd&nbsp;&lt;-&nbsp;newStdGen
&nbsp;&nbsp;<span>return</span>&nbsp;$&nbsp;rndGenSelect&nbsp;rnd&nbsp;xs&nbsp;count
```

Now we can clearly see that the action first creates the `rnd` random number generator and then passes it to `rndGenSelect`. That's what happened before, but it was buried in a lambda expression and Haskell's right-to-left causality. Most people would find the first version (without `do` notation) less readable, and more difficult to write.

Related to _developer ergonomics_, though, `do` notation makes the simple code (i.e. code that separates predictable code from unpredictable code) easy (that is; _at hand_).

F# computation expressions offer the same kind of syntactic sugar, making it easy to write simple code.

### Delay gratification [#](https://blog.ploeh.dk/2024/05/13/gratification/#86e9a9bd6cfc4408bedace8acd330f64)

While it's possible to set up a development context in such a way that it nudges you to work in a way that's ultimately good for you, temptation is everywhere.

Not only may new language features, IDE functionality, or frameworks entice you to do something that may be disadvantageous in the long run. There may also be actions you don't take because it just feels better to move on.

Do you take the time to write good commit messages? Not just a single-line heading, but [a proper message that explains your context and reasoning](https://github.com/GreanTech/AtomEventStore/commit/615cdee2c4d675d412e6669bcc0678655376c4d1)?

Most people I've observed working with source control 'just want to move on', and can't be bothered to write a useful commit message.

I hear about the same mindset when it comes to code reviews, particularly pull request reviews. Everyone 'just wants to write code', and no-one want to review other people's code. Yet, in a shared code base, you have to live with the code that other people write. Why not review it so that you have a chance to decide what that shared code base should look like?

Delay your own gratification a bit, and reap the awards later.

### Conclusion [#](https://blog.ploeh.dk/2024/05/13/gratification/#630055ed606d43289d71232dd1ef1c25)

The only goal I have with this article is to make you think about the consequences of new and innovative tools and frameworks. Particularly if they are immediately compelling, they may be empty calories. Consider if there may be disadvantages to adopting a new way of doing things.

Some tools and technologies give you instant gratification, but may be unhealthy in the long run. This is, like most other things, context-dependent. [In the long run](https://blog.ploeh.dk/2023/01/16/in-the-long-run) your company may no longer be around. Sometimes, it pays to deliberately do something that you know is bad, in order to reach a goal before your competition. That was the original _technical debt_ metaphor.

Often, however, it pays to delay gratification. Learn curl instead of Postman. Learn to design proper REST APIs instead of relying on OpenAI. If you need to write ad-hoc scripts, [use a language suitable for that](https://blog.ploeh.dk/2024/02/05/statically-and-dynamically-typed-scripts).
