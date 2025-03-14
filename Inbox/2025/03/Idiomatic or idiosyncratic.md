---
created: 2025-03-05T14:03:45 (UTC -03:00)
tags: []
source: https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/
author: Mark Seemann
---

# Idiomatic or idiosyncratic?

> ## Excerpt
> A strange programming construct may just be a friendly construct you haven't yet met.

---
_A strange programming construct may just be a friendly construct you haven't yet met._

Take a look at this fragment of JavaScript, and reflect on how you feel about it.

```
<span>var</span>&nbsp;nerdCapsToKebabCase&nbsp;=&nbsp;<span>function</span>&nbsp;(text)&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;result&nbsp;=&nbsp;<span>""</span>;
&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;(<span>var</span>&nbsp;i&nbsp;=&nbsp;0;&nbsp;i&nbsp;&lt;&nbsp;text.length;&nbsp;i++)&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;c&nbsp;=&nbsp;text.charAt(i);
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(i&nbsp;&gt;&nbsp;0&nbsp;&amp;&amp;&nbsp;c&nbsp;==&nbsp;c.toUpperCase())&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;<span>"-"</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;c.toLowerCase();
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;result;
};
```

Do you understand what it does? Does it feel appropriate for the language? Does it communicate its purpose?

Most likely, you answered yes to all three questions - at least if you know what [NerdCaps](http://c2.com/cgi/wiki?CapitalizationRules) and [kebab-case](http://c2.com/cgi/wiki?KebabCase) means.

Would your father, or your 12-year old daughter, or your English teacher, also answer _yes_ to all of these questions? Probably not, assuming they don't know programming.

Everything is easy once you know how to do it; everything is hard if you don't.

### Beauty is in the eye of the beholder [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#4f9be9fd77024195bacd70e45f197e9c "permalink")

Writing software is hard. Writing maintainable software - code that you and your team can keep working on year after year - is extraordinarily hard. The most prominent sources of problems seem to be accidental complexity and coupling.

During my career, I've advised thousands of people on how to write maintainable code: how to reduce coupling, how to simplify, how to express code that a human can easily reason about.

The most common reaction to my suggestions?

> "But, that's even more unreadable than the code we already had!"

What that really means is usually: _"I don't understand this, and I feel very uncomfortable about that."_

That's perfectly natural, but it doesn't mean that my suggestions are _really_ unreadable. It just means that you may have to work a little before it becomes readable, but once that happens, you will understand why it's not only readable, but also better.

Let's take a step back and examine what it means when source code is readable.

### Imperative code for beginners [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#c793fb5dc1dc40f2bc176eafa31683fa "permalink")

Imagine that you're a beginner programmer, and that you have to implement the _nerd caps to kebab case converter_ from the introduction.

This converter should take as input a string in _NerdCaps_, and convert it to _kebab-case_. Here's some sample data:

| Input | Expected output |
| --- | --- |
| Foo | foo |
| Bar | bar |
| FooBar | foo-bar |
| barBaz | bar-baz |
| barBazQuux | bar-baz-quux |
| garplyGorgeFoo | garply-gorge-foo |

Since (in this imaginary example) you're a beginner, you should choose to use a beginner's language, so perhaps you select Visual Basic .NET. That seems appropriate, as BASIC was originally an acronym for _Beginner's_ All-purpose Symbolic Instruction Code.

Perhaps you write the function like this:

```
<span>Function</span>&nbsp;Convert(text&nbsp;<span>As</span>&nbsp;<span>String</span>)&nbsp;<span>As</span>&nbsp;<span>String</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>Dim</span>&nbsp;result&nbsp;=&nbsp;<span>""</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>For</span>&nbsp;index&nbsp;=&nbsp;0&nbsp;<span>To</span>&nbsp;text.Length&nbsp;-&nbsp;1
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>Dim</span>&nbsp;c&nbsp;=&nbsp;text(index)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>If</span>&nbsp;index&nbsp;&gt;&nbsp;0&nbsp;<span>And</span>&nbsp;<span>Char</span>.IsUpper(c)&nbsp;<span>Then</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;<span>"-"</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>End</span>&nbsp;<span>If</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;c.ToString().ToLower()
&nbsp;&nbsp;&nbsp;&nbsp;<span>Next</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>Return</span>&nbsp;result
<span>End</span>&nbsp;<span>Function</span>
```

Is this appropriate Visual Basic code? Yes, it is.

Is it readable? That completely depends on who's doing the reading. You may find it readable, but does your father, or your 12-year old daughter?

I asked my 12-year old daughter if she understood, and she didn't. However, she understands this:

_Funktionen omformer en stump tekst ved at først at dele den op i stumper hver gang der optræder et stort bogstav. Derefter sætter den bindestreg mellem hver tekststump, og laver hele teksten om til små bogstaver - altså nu med bindestreger mellem hver tekststump._

Did you understand that? If you understand Danish (or another Scandinavian language), you probably did; otherwise, you probably didn't.

Readability is evaluated based on what you already know. However, what you know isn't constant.

### Imperative code for seasoned programmers [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#445a7f25a4fb478fbb65d19f9ca834fd "permalink")

Once you've gained some experience, you may find Visual Basic's syntax too verbose. For instance, once you understand the idea about _scope_, you may find End If, End Function etc. to be noisy rather than helpful.

Perhaps you're ready to replace those keywords with curly braces. Here's the same function in C#:

```
<span>public</span>&nbsp;<span>string</span>&nbsp;Convert(<span>string</span>&nbsp;text)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;result&nbsp;=&nbsp;<span>""</span>;
&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;(<span>int</span>&nbsp;i&nbsp;=&nbsp;0;&nbsp;i&nbsp;&lt;&nbsp;text.Length;&nbsp;i++)
&nbsp;&nbsp;&nbsp;&nbsp;{
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;c&nbsp;=&nbsp;text[i];
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(i&nbsp;&gt;&nbsp;0&nbsp;&amp;&amp;&nbsp;<span>Char</span>.IsUpper(c))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;<span>"-"</span>;
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;=&nbsp;result&nbsp;+&nbsp;c.ToString().ToLower();
&nbsp;&nbsp;&nbsp;&nbsp;}
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;result;
}
```

Is this proper C# code? Yes. Is it readable? Again, it really depends on what you already know, but most programmers familiar with the C language family would probably find it fairly approachable. Notice how close it is to the original JavaScript example.

### Imperative code for experts [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#4552f7e8bcfb4ee9a4d30ecf3f9a7a33 "permalink")

Once you've gained lots of experience, you will have learned that although the curly braces _formally_ delimit scopes, in order to make the code readable, you have to make sure to indent each scope appropriately. That seems to violate the DRY principle. Why not let the indentation make the code readable, as well as indicate the scope?

Various languages have so-called _significant whitespace_. The most widely known may be Python, but since we've already looked at two .NET languages, why not look at a third?

Here's the above function in F#:

```
<span>let</span>&nbsp;nerdCapsToKebabCase&nbsp;(text&nbsp;:&nbsp;<span>string</span>)&nbsp;=&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span>let</span>&nbsp;<span>mutable</span>&nbsp;result&nbsp;=&nbsp;<span>""</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>for</span>&nbsp;i&nbsp;=&nbsp;0&nbsp;<span>to</span>&nbsp;text.Length&nbsp;-&nbsp;1&nbsp;<span>do</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>let</span>&nbsp;c&nbsp;=&nbsp;text.Chars&nbsp;i
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(i&nbsp;&gt;&nbsp;0&nbsp;&amp;&amp;&nbsp;<span>Char</span>.IsUpper&nbsp;c)&nbsp;<span>then</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;&lt;-&nbsp;result&nbsp;+&nbsp;<span>"-"</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;result&nbsp;&lt;-&nbsp;result&nbsp;+&nbsp;c.ToString().ToLower()
&nbsp;&nbsp;&nbsp;&nbsp;result
```

Is this appropriate F# code?

Not really. While it compiles and works, it goes against the grain of the language. F# is a _Functional First_ language, but the above implementation is as imperative as all the previous examples.

### Functional refactoring [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#ce69ea31c6fb4f979699dc2930fb8ea8 "permalink")

A more Functional approach in F# could be this:

```
<span>let</span>&nbsp;nerdCapsToKebabCase&nbsp;(text&nbsp;:&nbsp;<span>string</span>)&nbsp;=&nbsp;
&nbsp;&nbsp;&nbsp;&nbsp;<span>let</span>&nbsp;addSkewer&nbsp;index&nbsp;c&nbsp;=
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>let</span>&nbsp;s&nbsp;=&nbsp;c.ToString().ToLower()
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>match</span>&nbsp;index,&nbsp;<span>Char</span>.IsUpper&nbsp;c&nbsp;<span>with</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;0,&nbsp;_&nbsp;<span>-&gt;</span>&nbsp;s
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;_,&nbsp;<span>true</span>&nbsp;<span>-&gt;</span>&nbsp;<span>"-"</span>&nbsp;+&nbsp;s
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;|&nbsp;_,&nbsp;<span>false</span>&nbsp;<span>-&gt;</span>&nbsp;s
&nbsp;&nbsp;&nbsp;&nbsp;text&nbsp;|&gt;&nbsp;<span>Seq</span>.mapi&nbsp;addSkewer&nbsp;|&gt;&nbsp;<span>String</span>.concat&nbsp;<span>""</span>
```

This implementation uses a private, inner function called `addSkewer` to convert each character to a string, based on the value of the character, as well as the position it appears. This conversion is applied to each character, and the resulting sequence of strings is finally concatenated and returned.

Is this good F#? Yes, I would say that it is. There are lots of different ways you could have approached this problem. This is only one of them, but I find it quite passable.

Is it readable? Again, if you don't know F#, you probably don't think that it is. However, the F# programmers I asked found it readable.

This solution has the advantage that it doesn't rely on mutation. Since the conversion problem in this article is a bit of a toy problem, the use of imperative code with heavy use of mutation probably isn't a problem, but as the size of your procedures grow, mutation makes it harder to understand what happens in the code.

Another improvement over the imperative version is that this implementation uses a higher level of abstraction. Instead of stating _how_ to arrive at the desired result, it states _what_ to do. This means that it becomes easier to change the composition of it in order to change other characteristics. As an example, this implementation is what is known as _embarrassingly parallel_, although it probably wouldn't pay to parallelise this implementation (it depends on how big you expect the input to be).

### Back-porting the functional approach [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#b2d9b0e6b2b6434ea890e7f0c7fd4623 "permalink")

While F# is a multi-paradigmatic language with an emphasis on Functional Programming, C# and Visual Basic are multi-paradigmatic languages with emphasis on Object-Oriented Programming. However, they still have some Functional capabilities, so you can back-port the F# approach. Here's one way to do it in C# using LINQ:

```
<span>public</span>&nbsp;<span>string</span>&nbsp;Convert(<span>string</span>&nbsp;text)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>String</span>.Concat(text.Select(AddSkewer));
}
 
<span>private</span>&nbsp;<span>static</span>&nbsp;<span>string</span>&nbsp;AddSkewer(<span>char</span>&nbsp;c,&nbsp;<span>int</span>&nbsp;index)
{
&nbsp;&nbsp;&nbsp;&nbsp;<span>var</span>&nbsp;s&nbsp;=&nbsp;c.ToString().ToLower();
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(index&nbsp;==&nbsp;0)
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;s;
&nbsp;&nbsp;&nbsp;&nbsp;<span>if</span>&nbsp;(<span>Char</span>.IsUpper(c))
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;<span>"-"</span>&nbsp;+&nbsp;s;
&nbsp;&nbsp;&nbsp;&nbsp;<span>return</span>&nbsp;s;
}
```

The LINQ Select method is equivalent to F#'s mapi function, and the AddSkewer function must rely on individual conditionals instead of pattern matching, but apart from that, it's structurally the same implementation.

You can do the same in Visual Basic:

```
<span>Function</span>&nbsp;Convert(text&nbsp;<span>As</span>&nbsp;<span>String</span>)&nbsp;<span>As</span>&nbsp;<span>String</span>
&nbsp;&nbsp;&nbsp;&nbsp;<span>Return</span>&nbsp;<span>String</span>.Concat(text.Select(<span>AddressOf</span>&nbsp;AddSkewer))
<span>End</span>&nbsp;<span>Function</span>
 
<span>Private</span>&nbsp;<span>Function</span>&nbsp;AddSkewer(c&nbsp;<span>As</span>&nbsp;<span>Char</span>,&nbsp;index&nbsp;<span>As</span>&nbsp;<span>Integer</span>)
&nbsp;&nbsp;&nbsp;&nbsp;<span>Dim</span>&nbsp;s&nbsp;=&nbsp;c.ToString().ToLower()
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>If</span>&nbsp;index&nbsp;=&nbsp;0&nbsp;<span>Then</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>Return</span>&nbsp;s
&nbsp;&nbsp;&nbsp;&nbsp;<span>End</span>&nbsp;<span>If</span>
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>If</span>&nbsp;<span>Char</span>.IsUpper(c)&nbsp;<span>Then</span>
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;<span>Return</span>&nbsp;<span>"-"</span>&nbsp;+&nbsp;s
&nbsp;&nbsp;&nbsp;&nbsp;<span>End</span>&nbsp;<span>If</span>
 
&nbsp;&nbsp;&nbsp;&nbsp;<span>Return</span>&nbsp;s
<span>End</span>&nbsp;<span>Function</span>
```

Are these back-ported implementations examples of appropriate C# or Visual Basic code? This is where the issue becomes more controversial.

### Idiomatic or idiosyncratic [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#b517cf44653e49238d38cafbaac6e995 "permalink")

Apart from reactions such as "that's unreadable," one of the most common reactions I get to such suggestions is:

"That's not idiomatic C#" (or Visual Basic).

Perhaps, but could it _become_ idiomatic?

Think about what an idiom is. In language, it just means a figure of speech, like "jumping the shark". Once upon a time, no-one said "jump the shark". Then Jon Hein came up with it, other people adopted it, and it became an idiom.

It's a bit like that with idiomatic C#, idiomatic Visual Basic, idiomatic Ruby, idiomatic JavaScript, etc. Idioms are adopted because they're deemed beneficial. It doesn't mean that the set of idioms for any language is finite or complete.

That something isn't idiomatic C# may only mean that it isn't idiomatic yet.

Or perhaps it is idiomatic, but it's just an idiom you haven't seen yet. We all have idiosyncrasies, but we should try to see past them. If a language construct is strange, it may be a friendly construct you just haven't met.

### Constant learning [#](https://blog.ploeh.dk/2015/08/03/idiomatic-or-idiosyncratic/#2ade085bb59942d19689179bdb9913d4 "permalink")

One of my friends once told me that he'd been giving a week-long programming course to a group of professional software developers, and as the week was winding down, one of the participants asked my friend how he knew so much.

My friend answered that he constantly studied and practised programming, mostly in his spare time.

The course participant incredulously replied: "You mean that there's _more_ to learn?"

As my friend told me, he really wanted to reply: “If that's a problem for you, then you've picked the wrong profession." However, he found something more bland, but polite, to answer.

There's no way around it: if you want to be (and stay) a programmer, you'll have to keep learning - not only new languages and technologies, but also new ways to use the subjects you already think you know.

If you ask me about advice about a particular problem, I will often suggest something that seems foreign to you. That doesn't make it bad, or unreadable. Give it a chance even if it makes you uncomfortable right now. Being uncomfortable often means that you're learning.

Luckily, in these days of internet, finding materials that will help you learn is easier and more accessible than ever before in history. There are tutorials, blog posts, books, and video services like [Pluralsight](http://bit.ly/1SVvTlS).
