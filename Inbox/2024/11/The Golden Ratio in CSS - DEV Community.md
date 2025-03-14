---
created: 2024-09-26T11:10:24 (UTC -03:00)
tags: [css,webdev,showdev,tutorial,software,coding,development,engineering,inclusive,community]
source: https://dev.to/madsstoumann/the-golden-ratio-in-css-53d0?context=digest
author: 
---

# The Golden Ratio in CSS - DEV Community

> ## Excerpt
> The golden ratio, also called the golden number, golden proportion, or even the divine proportion, is...

---
The golden ratio, also called the golden number, golden proportion, or even the divine proportion, is a special relationship between two numbers that equals approximately 1.618. It’s often symbolized by the Greek letter "phi." What’s fascinating is how closely this ratio is tied to the Fibonacci sequence—a series of numbers where each number is the sum of the two before it. The Fibonacci sequence starts with 0, 1, and then continues: 1, 2, 3, 5, 8, 13, 21, and so on. As you move further along in the sequence, the ratio between each number and the one before it gets closer and closer to 1.618, or phi. This unique ratio shows up in nature, art, and architecture, making it both mathematically intriguing and visually pleasing!

In this tutorial, we'll re-create the _Golden Ratio Diagram_ in CSS, using a few grid-declarations and some additional tricks.

Let's get started!

___

We'll use this part of the Fibonacci-sequence:  

That’s 8 digits, so our markup will consist of an `<ol>` with 8 `<li>` elements.

In CSS grid, we'll create a canvas with the dimensions derived from the sum of the last two digits, `13 + 21 = 34`, for the width, and the largest digit, `21`, for the height:  

```
<span>ol</span> <span>{</span>
  <span>all</span><span>:</span> <span>unset</span><span>;</span>
  <span>display</span><span>:</span> <span>grid</span><span>;</span>
  <span>grid-template-columns</span><span>:</span> <span>repeat</span><span>(</span><span>34</span><span>,</span> <span>1</span><span>fr</span><span>);</span>
  <span>grid-template-rows</span><span>:</span> <span>repeat</span><span>(</span><span>21</span><span>,</span> <span>1</span><span>fr</span><span>);</span>
  <span>list-style</span><span>:</span> <span>none</span><span>;</span>
<span>}</span>
```

Not much to see yet, but if we enable Dev Tool´s _Grid Inspector_, we can see the grid:

[![Grid inspector](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fl7w4ns1e5qks0r1jvl4p.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fl7w4ns1e5qks0r1jvl4p.png)

Next, we'll add some common styles for the `<li>`\-elements:  

```
  <span>li</span> <span>{</span>
    <span>aspect-ratio</span><span>:</span> <span>1</span> <span>/</span> <span>1</span><span>;</span>
    <span>background</span><span>:</span> <span>var</span><span>(</span><span>--bg</span><span>);</span>
    <span>grid-area</span><span>:</span> <span>var</span><span>(</span><span>--ga</span><span>);</span>
<span>}</span>
```

Still nothing to see, so let's fill out the `--bg` (`background-color`) and `--ga` (`grid-area`) variables with some content:  

```
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>1</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#e47a2c</span><span>;</span>
  <span>--ga</span><span>:</span> <span>1</span> <span>/</span> <span>1</span> <span>/</span> <span>22</span> <span>/</span> <span>22</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>2</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#baccc0</span> <span>;</span>
  <span>--ga</span><span>:</span> <span>1</span> <span>/</span> <span>22</span> <span>/</span> <span>23</span> <span>/</span> <span>35</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>3</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#6c958f</span><span>;</span>
  <span>--ga</span><span>:</span> <span>14</span> <span>/</span> <span>27</span> <span>/</span> <span>22</span> <span>/</span> <span>35</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>4</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#40363f</span><span>;</span>
  <span>--ga</span><span>:</span> <span>17</span> <span>/</span> <span>22</span> <span>/</span> <span>22</span> <span>/</span> <span>27</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>5</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#d7a26c</span><span>;</span>
  <span>--ga</span><span>:</span> <span>14</span> <span>/</span> <span>22</span> <span>/</span> <span>17</span> <span>/</span> <span>25</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>6</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#ae4935</span><span>;</span>
  <span>--ga</span><span>:</span> <span>14</span> <span>/</span> <span>25</span> <span>/</span> <span>17</span> <span>/</span> <span>27</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>7</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#e47a2c</span><span>;</span>
  <span>--ga</span><span>:</span> <span>16</span> <span>/</span> <span>26</span> <span>/</span> <span>17</span> <span>/</span> <span>27</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>8</span><span>)</span> <span>{</span>
  <span>--bg</span><span>:</span> <span>#f7e6d4</span><span>;</span>
  <span>--ga</span><span>:</span> <span>16</span> <span>/</span> <span>25</span> <span>/</span> <span>17</span> <span>/</span> <span>26</span><span>;</span>
<span>}</span>
```

And now, we get this:

[![Fibonacci](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjjx7xvt8p0zqncgxy96m.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fjjx7xvt8p0zqncgxy96m.png)

Cool! So what happened? We gave each `<li>` it's own `background-color` using the `--bg` custom property. Then we used `grid-area` to place and size the rectangles within the grid. `grid-area` is a shorthand for:  

```
row-start / col-start / row-end / col-end
```

Now, how about creating the spiral effect? While CSS can’t directly draw spirals, we can fake it by adding circles as `::after` pseudo-elements on each `<li>`:  

```
<span>li</span> <span>{</span>
  <span>/* as before */</span>
  <span>overflow</span><span>:</span> <span>hidden</span><span>;</span>
  <span>position</span><span>:</span> <span>relative</span><span>;</span>

  <span>&amp;::after</span> <span>{</span>
    <span>aspect-ratio</span><span>:</span> <span>1</span> <span>/</span> <span>1</span><span>;</span>
    <span>background-color</span><span>:</span> <span>rgba</span><span>(</span><span>255</span><span>,</span> <span>255</span><span>,</span> <span>255</span><span>,</span> <span>.3</span><span>);</span>
    <span>border-radius</span><span>:</span> <span>50%</span><span>;</span>
    <span>content</span><span>:</span> <span>''</span><span>;</span>
    <span>display</span><span>:</span> <span>block</span><span>;</span>
    <span>inset</span><span>:</span> <span>0</span><span>;</span>
    <span>position</span><span>:</span> <span>absolute</span><span>;</span>
  <span>}</span>
<span>}</span>
```

This gives us:

[![Circles](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyw4qyfxj5pbsc3mqxb6y.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fyw4qyfxj5pbsc3mqxb6y.png)

Not quite there yet, but if we **double** the size of the circles and **translate** them slightly, it starts looking better:  

```
&amp;::after {
  /* as before */
  scale: 2;
  translate: var(--tl);
}
```

However, not much changes until we update the `--tl` (translate) property for each `<li>`:  

```
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>1</span><span>)</span> <span>{</span>
  <span>--tl</span><span>:</span> <span>50%</span> <span>50%</span><span>;</span>
<span>}</span>
<span>&amp;</span><span>:nth-of-type</span><span>(</span><span>2</span><span>)</span> <span>{</span>
  <span>--tl</span><span>:</span> <span>-50%</span> <span>50%</span><span>;</span>
<span>}</span>
<span>/*  and so on for the rest */</span>
```

Now we get this:

[![Final](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fj8bbsnngd2yr0nhu0vqm.png)](https://media.dev.to/cdn-cgi/image/width=800%2Cheight=%2Cfit=scale-down%2Cgravity=auto%2Cformat=auto/https%3A%2F%2Fdev-to-uploads.s3.amazonaws.com%2Fuploads%2Farticles%2Fj8bbsnngd2yr0nhu0vqm.png)

### [](https://dev.to/madsstoumann/the-golden-ratio-in-css-53d0?context=digest#what-happened-here)What happened here?

We created a circle double the size of its element, then used translate to shift the circle in different directions to give the appearance of a continuous spiral. By adjusting the translation for each element, the circles "flow" into one another, mimicking a spiral.

Finally, because we added `overflow: hidden` to the `<li>` elements, the circles are "cut off" when they overflow their containers, giving us the illusion of a perfect spiral!

Here’s the finished result in a CodePen:
