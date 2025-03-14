---
created: 2025-01-20T17:50:53 (UTC -03:00)
tags: []
source: https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter
author: 
---

# Using LLMs to help LLMs build Encore apps – Encore Blog

> ## Excerpt
> How we used LLMs to produce instructions for LLMs to build Encore applications.

---
Here at Encore, we're passionate about making it easier to build great applications. This is a moving target, and as LLM powered tools get more powerful, we're seeing more and more people use them to build Encore applications. Stands to reason, we therefore need to ensure LLMs also have an easy time when building Encore applications.

## [The Challenge when you don't have 10 years of open-source projects for training data](https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter#the-challenge-when-you-dont-have-10-years-of-open-source-projects-for-training-data)

Encore is a framework designed to streamline backend development, offering unique features and conventions. However, without having the luxury of 10 years+ of mass adoption and the open-source training data that comes with it, mainstream LLMs often struggled to:

-   Fully utilize Encore’s built-in features.
-   Adhere to framework-specific conventions.
-   Generate code that aligns with best practices for Encore applications.

This makes sense since Encore is a relatively new backend framework, so it's not surprising that LLMs aren't as good at Encore as they are at React for instance. It's an important developer experience problem however, since developers are increasingly expecting to use LLMs to generate code.

## [Our Simple but Effective Solution](https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter#our-simple-but-effective-solution)

To address this, we created a tailored set of instructions specifically designed for LLMs. Here's how we approached the problem:

1.  **Rewriting Documentation for LLMs:** Using Claude from Anthropic, we rewrote our [framework documentation](https://encore.dev/docs) and SDK reference following Anthropic’s guidelines for formatting instructions for LLMs. This ensured clarity and precision, making it easier for models to understand and apply Encore’s principles.
    
2.  **Adding Conventions and Rules:** Beyond basic documentation, we developed a set of conventions and general rules that LLMs can follow. These include coding standards, recommended practices, and specific tips for leveraging Encore’s unique features.
    
3.  **Packaging the Instructions:** The result was a comprehensive file of approximately 1,500 lines that encapsulates everything an LLM needs to know about building with Encore. This file can be added directly to your repository, giving your LLM tool automatic context about Encore.
    
    For users of the [Cursor IDE](https://docs.cursor.com/context/rules-for-ai), simply rename the file to `.cursorrules` enables automatic integration.
    

## [The Result — LLMs are now MUCH better at building Encore apps](https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter#the-result--llms-are-now-much-better-at-building-encore-apps)

We're thrilled to share that **the LLM instructions for both Encore.ts and Encore.go are now open source and available on GitHub ([TypeScript instructions](https://github.com/encoredev/encore/blob/main/ts_llm_instructions.txt) | [Go instructions](https://github.com/encoredev/encore/blob/main/go_llm_instructions.txt))**.

This initial release has already shown a significant boost in LLM performance for generating Encore-compatible code. Most of the time when using tools like [Cursor](https://cursor.com/), you can now generate an Encore applications with multiple services and infrastructure, immediately on the first try.

However, we’re not stopping here. We invite the community to share feedback and contribute through pull requests to help us refine and expand these instructions further. Explore the instructions and contribute on [GitHub](https://github.com/encoredev/encore).

## [Add Encore LLM instructions to your app](https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter#add-encore-llm-instructions-to-your-app)

**1\. Download the instructions**

-   Encore.ts: [ts\_llm\_instructions.txt](https://github.com/encoredev/encore/blob/main/ts_llm_instructions.txt)
-   Encore.go: [go\_llm\_instructions.txt](https://github.com/encoredev/encore/blob/main/go_llm_instructions.txt)

**2\. Add the instructions to your app**

-   Cursor: Rename the file to `.cursorrules`.
-   GitHub Copilot: Paste content in `.github/copilot-instructions.md`.
-   For other tools, place the file in your app root.

## [Wrapping up](https://encore.dev/blog/llm-instructions?utm_source=tldrnewsletter#wrapping-up)

By giving LLMs this detailed guidance, we’ve bridged the gap between Encore’s powerful framework for building backends with declarative infrastructure and the growing potential of AI-powered development tools. We’re super excited to see how you will use these enhanced capabilities to create even better Encore applications.

Beyond providing instructions, we're also working on tools LLM powered code generation for Encore. If you're interested make sure to sign up for our newsletter below and follow us on [Twitter](https://x.com/encoredotdev) for updates.
