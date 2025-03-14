Absolutely nobody likes the hiring process. Not the managers hiring, not the recruitment people, and **certainly** not the candidates.

Tech interviews are one of the worst parts of the process and are pretty much universally hated by the people taking them. We’ve all heard stories of people being asked comp sci questions about _O(n)_ efficiency, only to connect APIs with basic middleware in their day job. I think the image below pretty much sums it up:

![brewinterview](https://kanenarraway.com/posts/ai-killed-the-tech-interview-now-what/techinterview.jpg)

The last few years, though, have seen a whole bunch of advancements to counteract the interview process:

1.  Before AI, there were cases of pay-to-interview schemes with interviewees who kept their cameras off during remote interviews. If hired, they sounded \*suspiciously different \*from the person who interviewed.
2.  [We’ve had cases of North Korean workers](https://blog.knowbe4.com/how-a-north-korean-fake-it-worker-tried-to-infiltrate-us) trying to get jobs with tech companies using deepfake videos. Whether this was just for money or something more nefarious is to be seen.
3.  AI is, of course, the big one. Using apps like GitHub Co-pilot and Cursor to auto-complete code requires very little skill in hands-on coding. Asking Claude and OpenAI answers to technical questions will usually result in a right answer, and the failure rate is improving rapidly.

This doesn’t even include the AI implications of resume creation, mass applications, one-way video interviews, and other techniques that have caused a tug-of-war between employers and employees. Still, in this post, we’ll focus squarely on the tech interview.

## Tech interview basics

Skip this section if you know the process; almost every company hiring developers has some variation of the below:

**[Hackerrank](https://www.hackerrank.com/)** - Online coding assessments usually use a pre-interview screen. Commonly used for grads, interns, and junior devs. It allows people to code in their own time at home without an interviewer spending their time assessing it. Pass this step to get to the other interviews.

**Comp Sci Fundamentals**\- Focuses on computer science fundamentals and is, again, more typically asked of junior developers. It usually involves Big O notation, questions about data structures, loops etc.

**The Coding Interview** - Usually done by everyone, this focuses on hands-on coding ability and asks you to build something like a calculator, a messaging system, or some common design pattern. The interviewers usually look for general coding skills first but might also evaluate language specifics too.

**Architecture / Design**\- Usually not hands-on coding. This tends to be for more senior devs and focuses more on how you build. How do you design systems, how do you connect frontend and backend, what database and API schemas do you use, how do you scale up to 10x, 100x users etc.

## AI in interviews

AI straight-up kills hackerrank. The only thing that is a filter for now is if you have the know-how to use AI and prompt correctly. That can be useful, but hackerrank will need to adapt somehow. If it can’t, I can’t see it being used at all in five years.

AI also significantly reduces the effectiveness of comp sci fundamentals and the coding interview as they are today. The problems are too simple, and an LLM can quickly answer them in most cases.

Architectural interviews are likely safe for a few years yet. From talking to people who have run these, it’s evident that someone is using AI. They often stop with long pauses, do not quite explain things succinctly, and do not understand the questions well enough to prompt the correct answer. As AI gets better (and faster), this will likely follow the same fate as the rest but I would give it some years yet.

## What are our options?

1.  **Stop remote tech interviews:** Using AI when an interviewer is looking over your shoulder is extremely difficult. This could be an in-person coding interview, assuming they pass all the rest.
2.  **Require some Pearson Vue-type spyware**: If you’ve ever taken a professional certification, you know what I mean. Some software that you install gives the interviewer full access while they watch you with a camera. This software is also easily bypassable in many ways; [there’s a whole subreddit dedicated to it](https://www.reddit.com/r/cheatonlineproctor/).
3.  **Bury our heads in the sand:** We ask people pretty please not to use AI on coding tests. The moral ones who _actually_ don’t get worse scores than the ones who do, and we end up hiring the wrong candidates.
4.  **Change our interviews to allow AI**: We start testing prompting and refactoring rather than testing coding ability. Can [someone program LLMs to get the outcomes they want](https://ghuntley.com/stdlib/) during an interview? This is a huge shift change and would be uncharted territory. As we encourage more people to use IDEs like Cursor this is likely more the skills we are looking for though. This is a huge hiring risk in the short term, as we still need strong coding ability to correct the AI’s failures. In enterprises, [we often deal with more than 30 files after all](https://www.reddit.com/r/ChatGPTCoding/comments/1ibtjri/my_project_became_so_big_that_claude_cant/).
5.  **Hybrid Approach:** Mixing some of the options above. We do remote architectural and coding interviews ( could even be one-way Hackerrank style) that test a mix of prompting and coding ability. Assuming the candidate passes this, they might travel to the office for an in-person coding interview.

## Possible solutions

Option four and five are likely the only answers in the long term. A lot of companies are doing RTO, but even companies that are 100% in-office still interview candidates from other cities. Spending money to fly every candidate out without an aggressive pre-screen is too wasteful.

One of the things we can do, however, is change the nature of the interviews themselves. Coding interviews today are quite basic, anywhere from FizzBuzz, to building a calculator. With AI assistants, we could expand this 10x and have people build complete applications. I think a single, longer interview (2 hours) that mixes architecture and coding will probably be the way to go.

Build an application, scale it, and add functions as the interviewer requires. A longer interview will be required to ensure consistency across the codebase as it develops and go into deeper questions rather than focusing on surface-level stuff.

This way, you can evaluate:

-   Can the person cope with basic git and IDE usage?
-   Can they prompt the LLM effectively and potentially program the LLM to give better outputs?
-   Can they understand the code well enough to glue LLM outputs together and make manual edits without making the code a total unreadable mess?
-   Can they be consistent over time with delivery?
-   Can they build something of a reasonable size and scale within a limited timeframe.

## Summary

Regardless, the nature of interviews will change drastically in the next few years. Right now, it’s fairly obvious when someone is using AI because it’s slow, it gives very to-the-point answers, and you can ask particular edge cases to test the candidate. That won’t last though.

Until then I think we’ll see the following:

1.  More people pass interviews than get exited during their probation period. An LLM can help you pass an interview but can’t help you be good at your job. Dealing with incidents, technical designs, and consistent communication is a whole different thing.
2.  Interns, Grads and Junior Engineers will have an even greater chasm to cross. The reality is that you need senior engineering skills as soon as possible, and universities do not teach them.

Plus, things will change even faster if people keep posting TikToks about how they used AI to pass the FAANG interview steps with absolutely no coding knowledge.

![godzilla fighting](https://kanenarraway.com/posts/ai-killed-the-tech-interview-now-what/godzilla.png)