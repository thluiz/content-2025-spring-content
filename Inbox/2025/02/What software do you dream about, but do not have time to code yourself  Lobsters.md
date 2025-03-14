---
created: 2025-02-06T08:35:25 (UTC -03:00)
tags: []
source: https://lobste.rs/s/btjtkr/what_software_do_you_dream_about_do_not?utm_source=tldrnewsletter
author: amirouche
---

# What software do you dream about, but do not have time to code yourself? | Lobsters

> ## Excerpt
> 308 comments

---
I’ve heard this before. Quite a few times now in fact.

I’ve been urged to make a demo video for Youtube showing how I work in one.

I had a very brief look at org-mode. From this I concluded that it is not even remotely comparable. Big disclaimer: I don’t really know it, or use it, or understand it. It looks to me as if it is the beginning of the germ of something that could with a decade’s work become an outliner.

Let me give 2 trivial examples.

From the structural point of view, I need a tool that enables me to edit text in 3D. I need it to track at least 5-10 level hierarchies and let me view only of those levels and above, automatically hiding all text below that level. It needs to be trivial to promote or demote text: this is a single-key operation. It needs to store the hierarchy info as metadata, _not in the text itself_ and when text is promoted or demoted it automatically updates the metadata.

It needs to offer tools for very quickly, with a keystroke, move views. So I’m down at level 8 working on a complex multi-step procedure, say. All higher levels are invisible off the top and bottom of the screen. With a few keystrokes I can zoom out and see the overall structure of top-level chapters, find the comparable section I was working on 200 pages away, move there, zoom in, open up more hidden levels, zoom in more, until I find the paragraph I wanted, not by location and not by keywords but by structural location in the document, copy some text, zoom back out, go back to where I was, zoom back in, paste the text and have it adopt the hierarchical position of the destination not the source.

This is all in one pane in a single big document; if “leaf nodes” are sub documents or separate files it’s useless to me. Then I must do the mental work of tracking what is where when that’s the outliner’s job.

Then at a formatting level:

It should track all these levels by _style_ and can format them from a stylesheet automatically, so it knows the styles for heading levels 1-8 on its own, independently defined in a separate file, and for body text and other functional styles such as image captions, footnotes, endnotes, etc.

It should automatically generate _and update_ tables of contexts and an index from this.

No manual formatting: formatting is programmatic according to structure.

At the end of 2016 I wrote a ~350 page technical manual in 6 weeks in MS Word 97 this way, running under WINE on Ubuntu 64-bit. Producing the repro-ready print version took an hour or two on the last day: I took a marketing doc from the client, deleted all the actual text in it to make it a style sheet, saved it as a template, and applied that template to my text to make a final document.

Then there were some checks and fixes, sure, but it was a few steps.

Result, 350 pages of text reformatted in the client’s fonts and colours, with their logos, headers, footers, etc. In a couple of mouse clicks.

The formatting stuff is very handy but not essential.

It’s the adding an additional level of dimension to text, so it has a structure, one that can be manipulated by the program _not_ by the operator editing markup, that matters. If I had to do this with markup it would be so much more work it’s not worth doing it.

The points are:

-   a powerful logical structure editor for a large body of text
-   tools for automatically maintaining and editing that structure by the software, not the user
-   tools for quickly, instantly, jumping between different levels of view of that structure as a navigation method

It’s not markup, so at any moment you can just leave outline view and you have a plain text file, or a Word doc formatted with styles, so that someone can work with it who doesn’t know what an outline _is_.

It’s not markup so when you move stuff around, up or down, or in and out of the tree, the program updates the tree not you, taking a mental task away and automating it.

Org mode, by comparison, seems to me to be… a cute but clunky shopping list applet?

LibreOffice Writer’s headings view is… not even that. It’s a way of labelling print preview?

Both are useless toys to me.

As a comparison, think of MS-DOS 3 EDLIN compared to Vim 9.1.

Yes, both are text editors. Yes, both have a command mode. Both can let you edit text files and save them. There is a trivial resemblance in the commands: EDLIN

```
1,5 d
```

… to delete lines 1 through 5, which surely has some vaguely similar Vim equivalent. I don’t speak Vi.

But functionally, the smaller simpler tool is so trivial compared to the grown-up one that you’d insult a user of the pro tool if you expected them to move to the simple one on the basis of their superficial resemblance.

Does that help?
