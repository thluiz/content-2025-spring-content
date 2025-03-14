---
created: 2024-09-12T18:49:16 (UTC -03:00)
tags: []
source: https://strangeleaflet.com/blog/game-of-life-functional-programming
author: 
---

# Conway's Game of Life: A Functional Approach · Strange Leaflet

> ## Excerpt
> In this post I describe how to implement Conway's Game of Life using functional programming to be able to transform a set of cells into the next iteration.

---
## Conway's Game of Life: A Functional Approach

-   Author: Stephen Ball
-   Published: 2024-08-23

-   Permalink: [/blog/game-of-life-functional-programming](https://strangeleaflet.com/blog/game-of-life-functional-programming)

In this post I describe how to implement Conway's Game of Life using functional programming to be able to transform a set of cells into the next iteration.

## Context: What is Conway’s Game of Life?

It might help to play with a [working Game of Life implementation](https://strangeleaflet.com/games/life) first.

The Game of Life is a mathematical game / life simulation invented by John Conway in 1970.

It consists of an infinite 2d board made up of squares, called cells. The cells can be alive or dead (or populated/unpopulative, active/inactive, etc).

The state of every square cell is governed by its eight neighbors

![A Game of Life cell and its neighbors](https://strangeleaflet.com/images/blog/2024/cell-neighbors.png)

-   Any living cell with fewer than two living neighbors: dies
-   Any living cell with two or three living neighbors: remains alive
-   Any living cell with more than three neighbors: dies
-   Any dead cell with exactly three neighbors: becomes alive

The game is seeded with an initial pattern and from that point the pattern plays out according to the rules.

It might seem trivial but the simple system and rules give rise to surprisingly complex living ecosystems. It’s even possible to build patterns of such complexity that they can store data and perform calculations.

You can try out my own [LiveView based Game of Life](https://strangeleaflet.com/games/life). You can try filling out a seed pattern yourself by clicking on the dead cells and then either “step” to see the next interation or “play” to play out the board. If you want a fun way to get started you can click “random” and then “play” to see how a random pattern generates interestingly lifelike behavior.

## Functional programming?

Functional programming is an excellent fit for programming the Game of Life because the game is a series of data transforms where one state is the input and the result is that transformed state. Not only functional but **pure** functional. Joy!

## Modeling the Game of Life

When modeling the Game of Life it’s very common to reach for a list, array, or some other continuous data structure to hold ALL the cells, living and dead e.g. `0` for dead and `1` for alive and have the board structure match the data storage.

```
[
  [0, 0, 0],
  [1, 1, 1],
  [0, 0, 0],
]
```

That approach is certainly possible, but the supporting code is quickly bogged down in the limits of having to iterate through hundreds or thousands of empty cells depending on the size of your board.

A more flexible approach is to not model the grid itself, but only maintain a list of the living cells as a set of coordinates. With this data structure the code becomes quite simple to implement as you’ll see.

"The wrong data structure has to be supported by code. The right data structure supports the code." — Stephen Ball

If we agree that the “top” “left” of the board is row 0, column 0 then we can model the board above and its four living cells like this:

```
[{1,0}, {1,1}, {1,2}]
```

And instead of a list, since we know each cell coordinate value can only appear once, we can use a `Set` to store the values which allows for fast checking to see if any given coordinate values are present in the set of living cells.

With that approach modeling the Game of Life turns into creating some function that accepts a list/set of living cells, and returns the list/set of cells as transformed by the given rules.

Per the game rules our above example would transform (commonly called “step”) like so

```
step([{1,0}, {1,1}, {1,2}])
# =&gt; [{0,1}, {1,1}, {2,1}]
```

A classic “blinker” pattern because the cells flip back and forth between the two states.

From step 1:

-   {0,0} remains dead because it only has one living neighbor
-   {0,1} becomes alive because it has exactly three living neighbors
-   {0,2} remains dead because it only has one living neighbor
-   {1,0} dies because it only has one living neighbor
-   {1,1} remains alive because it has two living neighbors
-   {1,2} dies because it only has one living neighbor
-   {2,0} remains dead because it only has one living neighbor
-   {2,1} becomes alive because it has exactly three living neighbors
-   {2,2} remains dead because it only has one living neighbor

![A Game of Life blinker pattern step 1](https://strangeleaflet.com/images/blog/2024/blinker1.png)

![A Game of Life blinker pattern step 2](https://strangeleaflet.com/images/blog/2024/blinker2.png)

## Coding the Game of Life (Clojure)

There are many, many implementations of the Game of Life in every programming language.

But for my money, this [Game of Life implementation in Clojure by Christophe Grand](http://clj-me.cgrand.net/2011/08/19/conways-game-of-life/) is the most elegant.

```
(defn neighbours [[x y]]
  (for [dx [-1 0 1] dy (if (zero? dx) [-1 1] [-1 0 1])]
    [(+ dx x) (+ dy y)]))

(defn step [cells]
  (set (for [[loc n] (frequencies (mapcat neighbours cells))
             :when (or (= n 3) (and (= n 2) (cells loc)))]
         loc)))
```

One function to calculate neighbors from a given point, another function to step a list of cells to the next iteration according to the Game of Life rules. Wonderful!

Let’s see it work:

```
$ clj
```

```
user=&gt; (neighbours [2,3])
([1 2] [1 3] [1 4] [2 2] [2 4] [3 2] [3 3] [3 4])
```

Given a pair of coordinates we can calculate its neighbors.

Here’s our blinker setup in data.

```
#{[1 0] [1 1] [1 2]}
```

If our `step` function is correct we should end up with `#{[0 1] [1 1] [2 1]}`

And we do (in a HashSet so the printing order is arbitrary)

```
user=&gt; (step #{[1 0] [1 1] [1 2]})
#{[1 1] [2 1] [0 1]}
```

And we can complete a blink cycle by stepping once more to return to our original cells.

```
user=&gt; (step #{[1 1] [2 1] [0 1]})
#{[1 0] [1 1] [1 2]}
```

How does that compact bit of code handle all the rules? Let’s break it down!

## Understanding the Clojure implementation

```
(defn step [cells]
  (set (for [[loc n] (frequencies (mapcat neighbours cells))
             :when (or (= n 3) (and (= n 2) (cells loc)))]
         loc)))
```

`defn` is defining a named function called `step` which will accept a single argument of a list of `cells`

Now let’s unpack the operations from right to left: ultimately we’ll reach the `set` function that will generate the return value from its argument.

We have `cells` `#{[1 1] [2 1] [0 1]}`

We generate a concatenated map of all the cells neighboring each of those cells

```
(mapcat neighbours #{[1 0] [1 1] [1 2]})
([0 -1] [0 0] [0 1] [1 -1] [1 1] [2 -1] [2 0] [2 1] [0 0] [0 1] [0 2] [1 0] [1 2] [2 0] [2 1] [2 2] [0 1] [0 2] [0 3] [1 1] [1 3] [2 1] [2 2] [2 3])
```

That is, we’ve applied the `neighbours` function to each cell `[1 0]` `[1 1]`, and `[1 2]` in turn and concatenated the results. If we were to call `map` instead of `mapcat` then we’d get a lists of neighbors for each cell in three separate results (formatted for clarity)

```
user=&gt; (map neighbours #{[1 0] [1 1] [1 2]})
(
  ([0 -1] [0 0] [0 1] [1 -1] [1 1] [2 -1] [2 0] [2 1])
  ([0 0]  [0 1] [0 2] [1 0]  [1 2] [2 0]  [2 1] [2 2])
  ([0 1]  [0 2] [0 3] [1 1]  [1 3] [2 1]  [2 2] [2 3])
)
```

`mapcat` returns that same data, but joined. Again formatted for clarity

```
(mapcat neighbours #{[1 0] [1 1] [1 2]})
(
  [0 -1] [0 0] [0 1] [1 -1] [1 1] [2 -1] [2 0] [2 1]
  [0 0]  [0 1] [0 2] [1 0]  [1 2] [2 0]  [2 1] [2 2]
  [0 1]  [0 2] [0 3] [1 1]  [1 3] [2 1]  [2 2] [2 3]
)
```

Why do we want a single list? Because that’s all the data we need to generate the next step of the game!

That might be a surprising result, don’t we need to iterate through each cell and make a decision about its next state?

Well, no. We need to iterate through every neighbor of each of our cells and make a decision about _its_ next state. That way we also determine cells that need to become alive and not just cells that remain alive or that become dead.

If it’s still confusing lets look at the frequencies data which is the next function call.

```
user=&gt; (frequencies (mapcat neighbours #{[1 0] [1 1] [1 2]}))
{[2 2] 2, [0 0] 2, [2 -1] 1, [1 0] 1, [2 3] 1, [1 1] 2, [1 3] 1, [0 3] 1, [1 -1] 1, [0 2] 2, [2 0] 2, [2 1] 3, [0 -1] 1, [1 2] 1, [0 1] 3}
```

Formatted and ordered

```
{
  [0 -1] 1,
  [0 0] 2,
  [0 1] 3
  [0 2] 2,
  [0 3] 1,
  [1 -1] 1,
  [1 0] 1,
  [1 1] 2,
  [1 2] 1,
  [1 3] 1,
  [2 -1] 1,
  [2 0] 2,
  [2 1] 3,
  [2 2] 2,
  [2 3] 1,
}
```

So we’ve taken our `mapcat` of all the neighbors and then counted each individual occurence of each cell.

Overlaying those results on our graphical representation might be helpful

![A Game of Life blinker pattern step 2](https://strangeleaflet.com/images/blog/2024/blinker1-neighbor-counts.png)

See how the accumulated counts of all the neighbors of all the cells that are living in the current step turns into exactly the data we need to generate the next step?

If you aren’t quite there consider one cell, say `[0,0]`. It has two neighbors in our first step as we can see. In other words: it was present in the list of neighbors for two of our living cells!

For each cell in consideration (neighboring a currently living cell) we’re counting how many times it appears in the neighbors of currently living cells. Beautiful!

For `[0,0]` it appears in two lists so it _must_ have two living neighbors itself:

-   living cell `[1,0]`: `[0,0]` is its neighbor
-   living cell `[1,1]`: `[0,0]` is its neighbor
-   living cell `[1,2]`: `[0,0]` is not its neighbor

So with the data structure coming from `frequencies` where we have a list of each cell and its count of neighbors we can simply apply the rules!

Let’s take another look at that `step` function:

```
(defn step [cells]
  (set (for [[loc n] (frequencies (mapcat neighbours cells))
             :when (or (= n 3) (and (= n 2) (cells loc)))]
         loc)))
```

We `for` loop over the `[loc n]` from the `frequencies` function where `loc` is the coordinates and `n` is the neighbors count.

In the for loop we have two conditions that will include the cell in the results:

-   `n` equals `3` (exactly three neighbors brings a cell to life)
-   `n` equals `2` and the cell is already in the list of cells (two neighbors keeps a living cell alive)

That’s it! We don’t need to explicitly express the rules for when a cell dies because, again, our elegant choice of data structure means we’re **only** tracking living cells. If a cell is not in our resulting list of living cells then it is by definition a dead cell.

That list of living cells is then sent through `set` to return the same type of data structure we started with. A set of coordinates where each coordinate can explicitly only appear once.

## Translating the Clojure approach to Elixir

### Failing to for loop

Clojure is great and all, but let’s see how we can express this code in Elixir! Because both languages are functional the result is largely the same, although not quite as concise in the Elixir form because Elixir `for` loops don’t allow the same level of expression.

First off, the `neighbors` calculation

```
<span>def</span><span> </span><span>neighbors</span><span data-group-id="6194909048-1">(</span><span data-group-id="6194909048-2">{</span><span>x</span><span>,</span><span> </span><span>y</span><span data-group-id="6194909048-2">}</span><span data-group-id="6194909048-1">)</span><span> </span><span data-group-id="6194909048-3">do</span><span>
  </span><span>for</span><span> </span><span>dx</span><span> </span><span>&lt;-</span><span> </span><span>-</span><span>1</span><span>..</span><span>1</span><span>,</span><span> </span><span>dy</span><span> </span><span>&lt;-</span><span> </span><span>-</span><span>1</span><span>..</span><span>1</span><span>,</span><span> </span><span data-group-id="6194909048-4">{</span><span>dx</span><span>,</span><span> </span><span>dy</span><span data-group-id="6194909048-4">}</span><span> </span><span>!==</span><span> </span><span data-group-id="6194909048-5">{</span><span>0</span><span>,</span><span> </span><span>0</span><span data-group-id="6194909048-5">}</span><span> </span><span data-group-id="6194909048-6">do</span><span>
    </span><span data-group-id="6194909048-7">{</span><span>dx</span><span> </span><span>+</span><span> </span><span>x</span><span>,</span><span> </span><span>dy</span><span> </span><span>+</span><span> </span><span>y</span><span data-group-id="6194909048-7">}</span><span>
  </span><span data-group-id="6194909048-6">end</span><span>
</span><span data-group-id="6194909048-3">end</span>
```

Excellent, a straightforward port from Clojure. We loop over a set of delta calculations to find the eight neighbors for a given cell coordinates.

Now `step`.

We can _almost_ write this in Elixir, but in Elixir `when` expressions are a special type of calculation called a **guard** and guards cannot execute remote functions such as that `MapSet.member?/2` but only kernel level functions.

```
<span>def</span><span> </span><span>step</span><span data-group-id="3485761611-1">(</span><span>cells</span><span data-group-id="3485761611-1">)</span><span> </span><span data-group-id="3485761611-2">do</span><span>
  </span><span>for</span><span> </span><span data-group-id="3485761611-3">{</span><span>cell</span><span>,</span><span> </span><span>n</span><span data-group-id="3485761611-3">}</span><span> </span><span>when</span><span> </span><span>n</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="3485761611-4">(</span><span>n</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>and</span><span> </span><span>MapSet</span><span>.</span><span>member?</span><span data-group-id="3485761611-5">(</span><span>cells</span><span>,</span><span> </span><span>cell</span><span data-group-id="3485761611-5">)</span><span data-group-id="3485761611-4">)</span><span> </span><span>&lt;-</span><span>
        </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="3485761611-6">(</span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="3485761611-7">(</span><span>cells</span><span>,</span><span> </span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="3485761611-7">)</span><span data-group-id="3485761611-6">)</span><span> </span><span data-group-id="3485761611-8">do</span><span>
    </span><span>cell</span><span>
  </span><span data-group-id="3485761611-8">end</span><span>
</span><span data-group-id="3485761611-2">end</span>
```

We’re a little closer if we try to use the `in` expression which is available in guards. But that doesn’t work because guards checks must work against compile-time data and `cells` is runtime data.

```
<span>def</span><span> </span><span>step</span><span data-group-id="7273602545-1">(</span><span>cells</span><span data-group-id="7273602545-1">)</span><span> </span><span data-group-id="7273602545-2">do</span><span>
  </span><span>for</span><span> </span><span data-group-id="7273602545-3">{</span><span>cell</span><span>,</span><span> </span><span>n</span><span data-group-id="7273602545-3">}</span><span> </span><span>when</span><span> </span><span>n</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="7273602545-4">(</span><span>n</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>and</span><span> </span><span>cell</span><span> </span><span>in</span><span> </span><span>cells</span><span data-group-id="7273602545-4">)</span><span> </span><span>&lt;-</span><span>
        </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="7273602545-5">(</span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="7273602545-6">(</span><span>cells</span><span>,</span><span> </span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="7273602545-6">)</span><span data-group-id="7273602545-5">)</span><span> </span><span data-group-id="7273602545-7">do</span><span>
    </span><span>cell</span><span>
  </span><span data-group-id="7273602545-7">end</span><span>
</span><span data-group-id="7273602545-2">end</span>
```

If we could magically have compile-time data to check `cell` against then this works

```
<span>def</span><span> </span><span>step</span><span data-group-id="3646677764-1">(</span><span>cells</span><span data-group-id="3646677764-1">)</span><span> </span><span data-group-id="3646677764-2">do</span><span>
  </span><span>for</span><span> </span><span data-group-id="3646677764-3">{</span><span>cell</span><span>,</span><span> </span><span>n</span><span data-group-id="3646677764-3">}</span><span> </span><span>when</span><span> </span><span>n</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="3646677764-4">(</span><span>n</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>and</span><span> </span><span>cell</span><span> </span><span>in</span><span> </span><span data-group-id="3646677764-5">[</span><span data-group-id="3646677764-6">{</span><span>1</span><span>,</span><span> </span><span>0</span><span data-group-id="3646677764-6">}</span><span>,</span><span> </span><span data-group-id="3646677764-7">{</span><span>1</span><span>,</span><span> </span><span>1</span><span data-group-id="3646677764-7">}</span><span>,</span><span> </span><span data-group-id="3646677764-8">{</span><span>1</span><span>,</span><span> </span><span>2</span><span data-group-id="3646677764-8">}</span><span data-group-id="3646677764-5">]</span><span data-group-id="3646677764-4">)</span><span> </span><span>&lt;-</span><span>
        </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="3646677764-9">(</span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="3646677764-10">(</span><span>cells</span><span>,</span><span> </span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="3646677764-10">)</span><span data-group-id="3646677764-9">)</span><span> </span><span data-group-id="3646677764-11">do</span><span>
    </span><span>cell</span><span>
  </span><span data-group-id="3646677764-11">end</span><span>
</span><span data-group-id="3646677764-2">end</span>
```

But ah well. That code doesn’t look very idiomatic in Elixir anyway. The Elixir approach is to build data pipelines that are readable top to bottom starting from an initial data structure and ending with the result. Let’s do that!

### Winning with a pipeline

```
<span>def</span><span> </span><span>step</span><span data-group-id="2226151527-1">(</span><span>cells</span><span data-group-id="2226151527-1">)</span><span> </span><span data-group-id="2226151527-2">do</span><span>
  </span><span>cells</span><span>
  </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="2226151527-3">(</span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="2226151527-3">)</span><span>
  </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="2226151527-4">(</span><span data-group-id="2226151527-4">)</span><span>
  </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>reduce</span><span data-group-id="2226151527-5">(</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="2226151527-6">(</span><span data-group-id="2226151527-6">)</span><span>,</span><span> </span><span data-group-id="2226151527-7">fn</span><span> </span><span data-group-id="2226151527-8">{</span><span>cell</span><span>,</span><span> </span><span>neighbors</span><span data-group-id="2226151527-8">}</span><span>,</span><span> </span><span>acc</span><span> </span><span>-&gt;</span><span>
    </span><span>if</span><span> </span><span>neighbors</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="2226151527-9">(</span><span>neighbors</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>&amp;&amp;</span><span> </span><span>MapSet</span><span>.</span><span>member?</span><span data-group-id="2226151527-10">(</span><span>cells</span><span>,</span><span> </span><span>cell</span><span data-group-id="2226151527-10">)</span><span data-group-id="2226151527-9">)</span><span> </span><span data-group-id="2226151527-11">do</span><span>
      </span><span>MapSet</span><span>.</span><span>put</span><span data-group-id="2226151527-12">(</span><span>acc</span><span>,</span><span> </span><span>cell</span><span data-group-id="2226151527-12">)</span><span>
    </span><span data-group-id="2226151527-11">else</span><span>
      </span><span>acc</span><span>
    </span><span data-group-id="2226151527-11">end</span><span>
  </span><span data-group-id="2226151527-7">end</span><span data-group-id="2226151527-5">)</span><span>
</span><span data-group-id="2226151527-2">end</span>
```

Ahh lovely. Perhaps more readable than the Clojure version if not quite as mathematically elegant.

Let’s read the pipeline, each `|>` is a function that takes the data on its left and makes it the first argument to the function call on its right.

We start with our enumerable data structure of living cells

```
<span>cells</span><span>
</span><span># =&gt; MapSet.new([{1, 0}, {1, 1}, {1, 2}])</span>
```

Flat map over `neighbors` gives us the concatenated list of all neighbors of living cells.

```
<span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="4600838109-1">(</span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="4600838109-1">)</span><span>
</span><span># [</span><span>
</span><span>#   {0, -1},</span><span>
</span><span>#   {0, 0},</span><span>
</span><span>#   {0, 1},</span><span>
</span><span>#   {1, -1},</span><span>
</span><span>#   {1, 1},</span><span>
</span><span>#   {2, -1},</span><span>
</span><span>#   {2, 0},</span><span>
</span><span>#   {2, 1},</span><span>
</span><span>#   {0, 0},</span><span>
</span><span>#   {0, 1},</span><span>
</span><span>#   {0, 2},</span><span>
</span><span>#   {1, 0},</span><span>
</span><span>#   {1, 2},</span><span>
</span><span>#   {2, 0},</span><span>
</span><span>#   {2, 1},</span><span>
</span><span>#   {2, 2},</span><span>
</span><span>#   {0, 1},</span><span>
</span><span>#   {0, 2},</span><span>
</span><span>#   {0, 3},</span><span>
</span><span>#   {1, 1},</span><span>
</span><span>#   {1, 3},</span><span>
</span><span>#   {2, 1},</span><span>
</span><span>#   {2, 2},</span><span>
</span><span>#   {2, 3}</span><span>
</span><span># ]</span>
```

Count each occurence of the cells from the list of neighbors.

```
<span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="7533910331-1">(</span><span data-group-id="7533910331-1">)</span><span>
</span><span># %{</span><span>
</span><span>#   {0, -1} =&gt; 1,</span><span>
</span><span>#   {0, 0} =&gt; 2,</span><span>
</span><span>#   {0, 1} =&gt; 3,</span><span>
</span><span>#   {0, 2} =&gt; 2,</span><span>
</span><span>#   {0, 3} =&gt; 1,</span><span>
</span><span>#   {1, -1} =&gt; 1,</span><span>
</span><span>#   {1, 0} =&gt; 1,</span><span>
</span><span>#   {1, 1} =&gt; 2,</span><span>
</span><span>#   {1, 2} =&gt; 1,</span><span>
</span><span>#   {1, 3} =&gt; 1,</span><span>
</span><span>#   {2, -1} =&gt; 1,</span><span>
</span><span>#   {2, 0} =&gt; 2,</span><span>
</span><span>#   {2, 1} =&gt; 3,</span><span>
</span><span>#   {2, 2} =&gt; 2,</span><span>
</span><span>#   {2, 3} =&gt; 1</span><span>
</span><span># }</span>
```

Reduce that list into a `MapSet` and only include cells that have three neighbors or have two neighbors and are already in the list of living cells.

```
<span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>reduce</span><span data-group-id="6936232579-1">(</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="6936232579-2">(</span><span data-group-id="6936232579-2">)</span><span>,</span><span> </span><span data-group-id="6936232579-3">fn</span><span> </span><span data-group-id="6936232579-4">{</span><span>cell</span><span>,</span><span> </span><span>neighbors</span><span data-group-id="6936232579-4">}</span><span>,</span><span> </span><span>acc</span><span> </span><span>-&gt;</span><span>
  </span><span>if</span><span> </span><span>neighbors</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="6936232579-5">(</span><span>neighbors</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>&amp;&amp;</span><span> </span><span>MapSet</span><span>.</span><span>member?</span><span data-group-id="6936232579-6">(</span><span>cells</span><span>,</span><span> </span><span>cell</span><span data-group-id="6936232579-6">)</span><span data-group-id="6936232579-5">)</span><span> </span><span data-group-id="6936232579-7">do</span><span>
    </span><span>MapSet</span><span>.</span><span>put</span><span data-group-id="6936232579-8">(</span><span>acc</span><span>,</span><span> </span><span>cell</span><span data-group-id="6936232579-8">)</span><span>
  </span><span data-group-id="6936232579-7">else</span><span>
    </span><span>acc</span><span>
  </span><span data-group-id="6936232579-7">end</span><span>
</span><span data-group-id="6936232579-3">end</span><span data-group-id="6936232579-1">)</span><span>
</span><span># =&gt; MapSet.new([{0, 1}, {1, 1}, {2, 1}])</span>
```

All together now!

```
<span>defmodule</span><span> </span><span>GameOfLife</span><span> </span><span data-group-id="7000138375-1">do</span><span>
  </span><span>@moduledoc</span><span> </span><span>"""
  Christophe Grand's implementation of Conway's Game of Life
  (http://clj-me.cgrand.net/2011/08/19/conways-game-of-life)

  Translated from Clojure to Elixir by Stephen Ball.
  """</span><span>

  </span><span>@doc</span><span> </span><span>"""
  Returns the coordinates of the neighbors for a given cell: `{x, y}`
  """</span><span>
  </span><span>def</span><span> </span><span>neighbors</span><span data-group-id="7000138375-2">(</span><span data-group-id="7000138375-3">{</span><span>x</span><span>,</span><span> </span><span>y</span><span data-group-id="7000138375-3">}</span><span data-group-id="7000138375-2">)</span><span> </span><span data-group-id="7000138375-4">do</span><span>
    </span><span>for</span><span> </span><span>dx</span><span> </span><span>&lt;-</span><span> </span><span>-</span><span>1</span><span>..</span><span>1</span><span>,</span><span> </span><span>dy</span><span> </span><span>&lt;-</span><span> </span><span>-</span><span>1</span><span>..</span><span>1</span><span>,</span><span> </span><span data-group-id="7000138375-5">{</span><span>dx</span><span>,</span><span> </span><span>dy</span><span data-group-id="7000138375-5">}</span><span> </span><span>!==</span><span> </span><span data-group-id="7000138375-6">{</span><span>0</span><span>,</span><span> </span><span>0</span><span data-group-id="7000138375-6">}</span><span> </span><span data-group-id="7000138375-7">do</span><span>
      </span><span data-group-id="7000138375-8">{</span><span>dx</span><span> </span><span>+</span><span> </span><span>x</span><span>,</span><span> </span><span>dy</span><span> </span><span>+</span><span> </span><span>y</span><span data-group-id="7000138375-8">}</span><span>
    </span><span data-group-id="7000138375-7">end</span><span>
  </span><span data-group-id="7000138375-4">end</span><span>

  </span><span>@doc</span><span> </span><span>"""
  Returns the next iteration for a given list of cells.

  iex&gt; GameOfLife.step(MapSet.new([{1, 0}, {1, 1}, {1, 2}]))
  MapSet.new([{0, 1}, {1, 1}, {2, 1}])
  """</span><span>
  </span><span>def</span><span> </span><span>step</span><span data-group-id="7000138375-9">(</span><span>cells</span><span data-group-id="7000138375-9">)</span><span> </span><span data-group-id="7000138375-10">do</span><span>
    </span><span>cells</span><span>
    </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>flat_map</span><span data-group-id="7000138375-11">(</span><span>&amp;</span><span>neighbors</span><span>/</span><span>1</span><span data-group-id="7000138375-11">)</span><span>
    </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>frequencies</span><span data-group-id="7000138375-12">(</span><span data-group-id="7000138375-12">)</span><span>
    </span><span>|&gt;</span><span> </span><span>Enum</span><span>.</span><span>reduce</span><span data-group-id="7000138375-13">(</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="7000138375-14">(</span><span data-group-id="7000138375-14">)</span><span>,</span><span> </span><span data-group-id="7000138375-15">fn</span><span> </span><span data-group-id="7000138375-16">{</span><span>cell</span><span>,</span><span> </span><span>neighbors</span><span data-group-id="7000138375-16">}</span><span>,</span><span> </span><span>acc</span><span> </span><span>-&gt;</span><span>
      </span><span>if</span><span> </span><span>neighbors</span><span> </span><span>==</span><span> </span><span>3</span><span> </span><span>or</span><span> </span><span data-group-id="7000138375-17">(</span><span>neighbors</span><span> </span><span>==</span><span> </span><span>2</span><span> </span><span>&amp;&amp;</span><span> </span><span>MapSet</span><span>.</span><span>member?</span><span data-group-id="7000138375-18">(</span><span>cells</span><span>,</span><span> </span><span>cell</span><span data-group-id="7000138375-18">)</span><span data-group-id="7000138375-17">)</span><span> </span><span data-group-id="7000138375-19">do</span><span>
        </span><span>MapSet</span><span>.</span><span>put</span><span data-group-id="7000138375-20">(</span><span>acc</span><span>,</span><span> </span><span>cell</span><span data-group-id="7000138375-20">)</span><span>
      </span><span data-group-id="7000138375-19">else</span><span>
        </span><span>acc</span><span>
      </span><span data-group-id="7000138375-19">end</span><span>
    </span><span data-group-id="7000138375-15">end</span><span data-group-id="7000138375-13">)</span><span>
  </span><span data-group-id="7000138375-10">end</span><span>
</span><span data-group-id="7000138375-1">end</span>
```

```
<span>iex&gt; </span><span>GameOfLife</span><span>.</span><span>step</span><span data-group-id="5586617565-1">(</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="5586617565-2">(</span><span data-group-id="5586617565-3">[</span><span data-group-id="5586617565-4">{</span><span>1</span><span>,</span><span> </span><span>0</span><span data-group-id="5586617565-4">}</span><span>,</span><span> </span><span data-group-id="5586617565-5">{</span><span>1</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-5">}</span><span>,</span><span> </span><span data-group-id="5586617565-6">{</span><span>1</span><span>,</span><span> </span><span>2</span><span data-group-id="5586617565-6">}</span><span data-group-id="5586617565-3">]</span><span data-group-id="5586617565-2">)</span><span data-group-id="5586617565-1">)</span><span>
</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="5586617565-7">(</span><span data-group-id="5586617565-8">[</span><span data-group-id="5586617565-9">{</span><span>0</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-9">}</span><span>,</span><span> </span><span data-group-id="5586617565-10">{</span><span>1</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-10">}</span><span>,</span><span> </span><span data-group-id="5586617565-11">{</span><span>2</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-11">}</span><span data-group-id="5586617565-8">]</span><span data-group-id="5586617565-7">)</span><span>

</span><span>iex&gt; </span><span>GameOfLife</span><span>.</span><span>step</span><span data-group-id="5586617565-12">(</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="5586617565-13">(</span><span data-group-id="5586617565-14">[</span><span data-group-id="5586617565-15">{</span><span>0</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-15">}</span><span>,</span><span> </span><span data-group-id="5586617565-16">{</span><span>1</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-16">}</span><span>,</span><span> </span><span data-group-id="5586617565-17">{</span><span>2</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-17">}</span><span data-group-id="5586617565-14">]</span><span data-group-id="5586617565-13">)</span><span data-group-id="5586617565-12">)</span><span>
</span><span>MapSet</span><span>.</span><span>new</span><span data-group-id="5586617565-18">(</span><span data-group-id="5586617565-19">[</span><span data-group-id="5586617565-20">{</span><span>1</span><span>,</span><span> </span><span>0</span><span data-group-id="5586617565-20">}</span><span>,</span><span> </span><span data-group-id="5586617565-21">{</span><span>1</span><span>,</span><span> </span><span>1</span><span data-group-id="5586617565-21">}</span><span>,</span><span> </span><span data-group-id="5586617565-22">{</span><span>1</span><span>,</span><span> </span><span>2</span><span data-group-id="5586617565-22">}</span><span data-group-id="5586617565-19">]</span><span data-group-id="5586617565-18">)</span>
```

The blinker works!

With that we have functionally implemented the Game of Life from a simple data structure paired with simple code.

If you’d like to play with a Game of Life implementation then you’re in luck because it’s a very popular thing to implement in impressive ways.

-   [My own implementation](https://strangeleaflet.com/games/life) is running the code from this very blog post
-   [https://oimo.io/works/life/](https://oimo.io/works/life/) is one of my favorite things on the Internet: an infinitely zoomable Game of Life implementing itself.
-   [https://playgameoflife.com/](https://playgameoflife.com/) has an extensive lexicon of patterns you can load

Enjoy!

___

-   Author: Stephen Ball
-   Published: 2024-08-23

-   Permalink: [/blog/game-of-life-functional-programming](https://strangeleaflet.com/blog/game-of-life-functional-programming)

In this post I describe how to implement Conway's Game of Life using functional programming to be able to transform a set of cells into the next iteration.
