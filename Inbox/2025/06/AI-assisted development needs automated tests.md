---
created: 2025-05-29T15:45:44 (UTC -03:00)
tags: []
source: https://simonwillison.net/2025/May/28/automated-tests/?utm_source=tldrnewsletter
author: Simon Willison
---

# AI-assisted development needs automated tests

> ## Excerpt
> I wonder if one of the reasons I'm finding LLMs so much more useful for coding than a lot of people that I see in online discussions is that effectively …

---
I wonder if one of the reasons I'm finding LLMs so much more useful for coding than a lot of people that I see in online discussions is that effectively _all_ of the code I work on has automated tests.

I've been trying to stay true to the idea of a [Perfect Commit](https://simonwillison.net/2022/Oct/29/the-perfect-commit/) - one that bundles the implementation, tests and documentation in a single unit - for over five years now. As a result almost every piece of (non [vibe-coding](https://simonwillison.net/tags/vibe-coding/)) code I work on has pretty comprehensive test coverage.

This _massively_ derisks my use of LLMs. If an LLM writes weird, convoluted code that solves my problem I can prove that it works with tests - and then have it refactor the code until it looks good to me, keeping the tests green the whole time.

LLMs help write the tests, too. I finally have a 24/7 pair programmer who can remember how to use [unittest.mock](https://docs.python.org/3/library/unittest.mock.html)!

Next time someone complains that they've found LLMs to be more of a hindrance than a help in their programming work, I'm going to try to remember to ask after the health of their test suite.
