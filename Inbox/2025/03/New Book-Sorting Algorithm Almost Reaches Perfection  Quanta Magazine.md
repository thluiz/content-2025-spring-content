Computer scientists often deal with abstract problems that are hard to comprehend, but an exciting new algorithm matters to anyone who owns books and at least one shelf. The algorithm addresses something called the library sorting problem (more formally, the “list labeling” problem). The challenge is to devise a strategy for organizing books in some kind of sorted order — alphabetically, for instance — that minimizes how long it takes to place a new book on the shelf.

Imagine, for example, that you keep your books clumped together, leaving empty space on the far right of the shelf. Then, if you add a book by Isabel Allende to your collection, you might have to move every book on the shelf to make room for it. That would be a time-consuming operation. And if you then get a book by Douglas Adams, you’ll have to do it all over again. A better arrangement would leave unoccupied spaces distributed throughout the shelf — but how, exactly, should they be distributed?

This problem was introduced in a [1981 paper (opens a new tab)](https://link.springer.com/chapter/10.1007/3-540-10843-2_34), and it goes beyond simply providing librarians with organizational guidance. That’s because the problem also applies to the arrangement of files on hard drives and in databases, where the items to be arranged could number in the billions. An inefficient system means significant wait times and major computational expense. Researchers have invented some efficient methods for storing items, but they’ve long wanted to determine the best possible way.

Last year, in [a study (opens a new tab)](https://arxiv.org/abs/2405.00807) that was presented at the Foundations of Computer Science conference in Chicago, a team of seven researchers described a way to organize items that comes tantalizingly close to the theoretical ideal. The new approach combines a little knowledge of the bookshelf’s past contents with the surprising power of randomness.

“It’s a very important problem,” said [Seth Pettie (opens a new tab)](https://web.eecs.umich.edu/~pettie/), a computer scientist at the University of Michigan, because many of the data structures we rely upon today store information sequentially. He called the new work “extremely inspired \[and\] easily one of my top three favorite papers of the year.”

## **Narrowing Bounds**

So how does one measure a well-sorted bookshelf? A common way is to see how long it takes to insert an individual item. Naturally, that depends on how many items there are in the first place, a value typically denoted by _n_. In the Isabel Allende example, when all the books have to move to accommodate a new one, the time it takes is proportional to _n_. The bigger the _n_, the longer it takes. That makes this an “upper bound” to the problem: It will never take longer than a time proportional to _n_ to add one book to the shelf.

The authors of the 1981 paper that ushered in this problem wanted to know if it was possible to design an algorithm with an average insertion time much less than _n_. And indeed, they proved that one could do better. They created an algorithm that was guaranteed to achieve an average insertion time proportional to (log _n_)<sup>2</sup>. This algorithm had two properties: It was “deterministic,” meaning that its decisions did not depend on any randomness, and it was also “smooth,” meaning that the books must be spread evenly within subsections of the shelf where insertions (or deletions) are made. The authors left open the question of whether the upper bound could be improved even further. For over four decades, no one managed to do so.

However, the intervening years did see improvements to the lower bound. While the upper bound specifies the maximum possible time needed to insert a book, the lower bound gives the fastest possible insertion time. To find a definitive solution to a problem, researchers strive to narrow the gap between the upper and lower bounds, ideally until they coincide. When that happens, the algorithm is deemed optimal — inexorably bounded from above and below, leaving no room for further refinement.

In 2004, a team of researchers found that the [best any algorithm could do (opens a new tab)](https://epubs.siam.org/doi/abs/10.1137/S0895480100315808?journalCode=sjdmec) for the library sorting problem — in other words, the ultimate lower bound — was log _n_. This result pertained to the most general version of the problem, applying to any algorithm of any type. Two of the same authors had already secured a result for a more specific version of the problem in 1990, showing that for any smooth algorithm, [the lower bound is significantly higher (opens a new tab)](https://link.springer.com/chapter/10.1007/3-540-52846-6_87): (log _n_)<sup>2</sup>. And in 2012, another team [proved the same lower bound (opens a new tab)](https://dl.acm.org/doi/abs/10.1145/2213977.2214083), (log _n_)<sup>2</sup>, for any deterministic algorithm that does not use randomness at all.

These results showed that for any smooth or deterministic algorithm, you could not achieve an average insertion time better than (log _n_)<sup>2</sup>, which was the same as the upper bound established in the 1981 paper. In other words, to improve that upper bound, researchers would need to devise a different kind of algorithm. “If you’re going to do better, you have to be randomized and non-smooth,” said [Michael Bender (opens a new tab)](https://www.cs.stonybrook.edu/people/faculty/michaelbender), a computer scientist at Stony Brook University.

![Michael Bender in a blue shirt wearing black glasses.](https://www.quantamagazine.org/wp-content/uploads/2025/01/MichaelBender_coMichaelBender-2-edit2.jpg)

Michael Bender went after the library sorting problem using an approach that didn’t necessarily make intuitive sense.

Courtesy of Michael Bender

But getting rid of smoothness, which requires items to be spread apart more or less evenly, seemed like a mistake. (Remember the problems that arose from our initial example — the non-smooth configuration where all the books were clumped together on the left-hand side of the shelf.) And it also was not obvious how leaving things to random chance — essentially a coin toss — would help matters. “Intuitively, it wasn’t clear that was a direction that made sense,” Bender said.

Nevertheless, in 2022, Bender and five colleagues decided to try out a randomized, non-smooth algorithm anyway, just to see whether it might offer any advantages.

## **A Secret History**

Ironically, progress came from another restriction. There are sound privacy or security reasons why you may want to use an algorithm that’s blind to the history of the bookshelf. “If I had _50 Shades of Grey_ on my bookshelf and took it off,” said [William Kuszmaul (opens a new tab)](https://csd.cmu.edu/people/faculty/william-kuszmaul) of Carnegie Mellon University, nobody would be able to tell.

In a 2022 paper, Bender, Kuszmaul and four co-authors created just such an algorithm — one that was “history independent,” non-smooth and randomized — which finally [reduced the 1981 upper bound (opens a new tab)](https://arxiv.org/abs/2203.02763), bringing the average insertion time down to (log _n_)<sup>1.5</sup>.

Kuszmaul remembers being surprised that a tool normally used to ensure privacy could confer other benefits. “It’s as if you used cryptography to make your algorithm faster,” he said. “Which just seems kind of strange.”

[Helen Xu (opens a new tab)](https://www.cc.gatech.edu/people/helen-xu) of the Georgia Institute of Technology, who was not part of this research team, was also impressed.  She said that the idea of using history independence for reasons other than security may have implications for many other types of problems.

## **Closing the Gap**

Bender, Kuszmaul and others made an even bigger improvement with last year’s paper. They again broke the record, lowering the upper bound to (log _n_) times (log log _n_)<sup>3</sup> — equivalent to (log _n_)<sup>1.000…1</sup>. In other words, they came exceedingly close to the theoretical limit, the ultimate lower bound of log _n_.

Once again, their approach was non-smooth and randomized, but this time their algorithm relied on a limited degree of history dependence. It looked at past trends to plan for future events, but only up to a point. Suppose, for instance, you’ve been getting a lot of books by authors whose last name starts with N — Nabokov, Neruda, Ng. The algorithm extrapolates from that and assumes more are probably coming, so it’ll leave a little extra space in the N section. But reserving too much space could lead to trouble if a bunch of A-name authors start pouring in. “The way we made it a good thing was by being strategically random about how much history to look at when we make our decisions,” Bender said.

The result built on and transformed their previous work. It “uses randomness in a completely different way than the 2022 paper,” Pettie said.

These papers collectively represent “a significant improvement” on the theory side, said [Brian Wheatman (opens a new tab)](https://computerscience.uchicago.edu/people/brian-wheatman/), a computer scientist at the University of Chicago. “And on the applied side, I think they have the potential for a big improvement as well.”

Xu agrees. “In the past few years, there’s been interest in using data structures based on list labeling for storing and processing dynamic graphs,” she said. These advances would almost certainly make things faster.

Meanwhile, there’s more for theorists to contemplate. “We know that we can almost do log _n_,” Bender said, “\[but\] there’s still this tiny gap” — the diminutive log log _n_ term that stands in the way of a complete solution. “We don’t know if the right thing to do is to lower the upper bound or raise the lower bound.”

Pettie, for one, doesn’t expect the lower bound to change. “Usually in these situations, when you see a gap this close, and one of the bounds looks quite natural and the other looks unnatural, then the natural one is the right answer,” he said. It’s much more likely that any future improvements will affect the upper bound, bringing it all the way down to log _n._ “But the world’s full of weird surprises.”