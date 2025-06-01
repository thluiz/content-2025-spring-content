---
created: 2025-04-15T22:07:11 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Gregor Ojstersek
---

# 

> ## Excerpt
> I am answering the question above + sharing the key challenges with AI adoption and my suggestions on how to approach it!

---
![User's avatar](https://substackcdn.com/image/fetch/w_64,h_64,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b7fdc30-d8c4-45f2-b0df-0b60baf9d4f4_1000x1000.jpeg)

Discover more from Engineering Leadership

Weekly newsletter for becoming a great engineering leader.

## Enforcing the use of AI in engineering teams - good or bad thing?

### I am answering the question above + sharing the key challenges with AI adoption and my suggestions on how to approach it!

[

![Gregor Ojstersek's avatar](https://substackcdn.com/image/fetch/w_36,h_36,c_fill,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F1b7fdc30-d8c4-45f2-b0df-0b60baf9d4f4_1000x1000.jpeg)

](https://substack.com/@gregorojstersek)

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F255a2cb9-8224-4e8e-9846-eda7a368de7a_800x391.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F255a2cb9-8224-4e8e-9846-eda7a368de7a_800x391.jpeg)

## [DevStats (sponsored)](https://devstats.com/?utm_source=newsletter&utm_medium=email&utm_campaign=eng-leadership)

I am happy to welcome back **[DevStats](https://devstats.com/?utm_source=newsletter&utm_medium=email&utm_campaign=eng-leadership)** as a sponsor. They were already a long-term sponsor last year and I am happy to have them back again.

[Phil Alves](https://www.linkedin.com/in/philalves/), founder of DevStats is an avid reader of this newsletter and I am also a fan of what they are building.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fb5d55dcb-f2ab-41c6-b724-9317a5c26015_800x327.jpeg)

](https://devstats.com/?utm_source=newsletter&utm_medium=email&utm_campaign=eng-leadership)

DevStats highlights bottlenecks, burnout risks, and delivery delays in real-time - so you can make continuous process improvements.

-   Visualize your sprints and focus on mission-critical work
    
-   Deploy faster with a clear view of flow metrics
    
-   Spot bottlenecks and risks earlier
    
-   Improve developer experience
    

I’ve checked DevStats thoroughly and I used it in one of my projects as well, so I highly recommend checking them out, if you are looking for such tool.

[Get started for free!](https://devstats.com/?utm_source=newsletter&utm_medium=email&utm_campaign=eng-leadership)

Let’s get back to this week’s thought.

## Intro

Most engineers are using AI tools, but they don’t necessarily trust them.

From [StackOverflow’s survey done in 2024](https://stackoverflow.blog/2024/09/23/where-developers-feel-ai-coding-tools-are-working-and-where-they-re-missing-the-mark/), we can already see that most engineers are using AI tools. 76% have answered that they are using or planning to use AI coding tools but they don’t necessarily trust them.

And I believe that at this time in April 2025, this percentage has just increased based on my conversations with many engineers and engineering leaders in the past month.

Especially with the increase in popularity of [Cursor](https://newsletter.eng-leadership.com/p/how-to-use-cursor-ai-to-build-side), it’s becoming very useful to speed up repetitive workflows and providing helpful suggestions when coding as [many engineering leaders have reported](https://newsletter.eng-leadership.com/p/how-to-use-ai-to-increase-software).

Also, Model Context Protocol (MCP) is a very exciting technology that allows AI applications to connect with external tools, data sources and systems, which will also help a lot with overall productivity in my opinion.

There have been claims like:  
\- no more developer hiring,  
\- AI replacing mid-level engineers and recently,  
\- AI increasing 100x the output and enforcing the usage of AI for everyone in the organization.

I have already responded to the claims of [mid-level engineers being replaced by AI in 2025](https://newsletter.eng-leadership.com/p/will-ai-replace-mid-level-engineers).

In today’s article, we'll focus on the enforcing part in engineering teams.

Is enforcing the use of AI the right way to go in engineering teams? I am answering this question in the article.

You can also expect to read the key challenges with AI adoption and my suggestions on how to approach the AI adoption process in engineering teams.

Let’s get straight into it!

## There has recently been shared publicly an internal memo by Shopify’s CEO Tobias Lütke

It has been quite an interesting read to see how companies like Shopify approach AI adoption across their teams. [You can read the full memo here](https://x.com/tobi/status/1909231499448401946).

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71ee5bb8-515e-46ee-8f63-83690ea8464a_800x768.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F71ee5bb8-515e-46ee-8f63-83690ea8464a_800x768.jpeg)

To be fair, I agree with a lot of the points mentioned and the use of AI provides a good productivity increase if used the right way.

Also the intention I believe is the right one → to inspire people to use AI in order to be productive and provide more value to the business.

But, there are nuances in there, that if not approached the right way, can be a downside for the culture and can inspire wrong behavior which can lead people to trying to game the system.

I’ll break down these nuances in the next sections.

## AI definitely improves productivity, but 100X? That’s not true for engineering teams

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d2142d6-4e5e-475a-afd2-19719d4a2d77_800x392.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5d2142d6-4e5e-475a-afd2-19719d4a2d77_800x392.jpeg)

As I have spoken to many different engineering leaders over the past month, I've heard about 5x productivity increase max. In most cases it's been from 0.3 - 1x increase.

Especially working with larger, more complex systems, AI doesn't help as much at this time.

It's great for prototyping and creating boilerplate or quick and simple projects as I did with the [Engineering Leadership Endless Runner Game](https://newsletter.eng-leadership.com/p/how-to-use-cursor-ai-to-build-side).

But everyone that has worked with larger more complex systems knows how important it is not to just blindly rely on AI to do things at this time.

You can read the insights from 11 engineering leaders on how they or their teams are using AI to increase Software Development productivity here: [How to use AI to increase Software Development productivity](https://newsletter.eng-leadership.com/p/how-to-use-ai-to-increase-software) (paid article).

## Including AI usage as a KPI in the performance review

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F677ca92a-0443-40e7-8556-f23c008563a4_800x607.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F677ca92a-0443-40e7-8556-f23c008563a4_800x607.jpeg)

I’ve heard of specific companies thinking about doing leaderboards of who is using the most LLM credits or who has commited the most AI generated code to the codebase.

It’s really important to understand that:

> Measuring the wrong things inspires people to try to game the system.

So, if you purely just measure the output and usage, people will naturally be prone to use that as much as possible, which would provide the wrong results for the business.

And it can also inspire pure individualism, which you don’t want in your organization.

These are the main things I value:

1.  Team productivity > Developer productivity
    
2.  Helping others > Completing your own tasks
    
3.  Building the RIGHT things > Amount of things being built
    

Using AI alone doesn’t mean much if you don’t provide value to the business or you don’t share your knowledge with others and your team is not working well together.

So, if you want to track AI usage, my advice is to map it to business results and how much the individual effort involves helping others and making the team and overall org more productive.

## Demonstrating that you can’t do what is needed with AI before asking for more Headcount

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8fc87a68-7251-4f99-b4b9-10929f156f5c_800x455.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8fc87a68-7251-4f99-b4b9-10929f156f5c_800x455.jpeg)

This framing makes sense from the business point of view, but from the people perspective it can be perceived differently.

There is a lot of fear from engineers being replaced by AI already and as proven many times, teams don’t operate well without psychological safety.

Of course, some form of pressure is fine, but one of the key ingredients of high-performing teams is that people are safe to make mistakes and learn from them, speak up and ask questions.

And this message can be interpreted in a way: “We wish to increase the usage of AI so we don’t need to hire new people and potentialy remove some of the existing ones as well”.

This hasn’t been said, but would be a logical step as we’ve seen many layoffs happening across the industry.

And as said above, this makes sense from the business perspective. Decrease the cost, while delivering more value, but may negatively impact the culture.

I really like [Nvidia’s approach](https://finance.yahoo.com/news/nvidia-ceo-jensen-huang-push-162320277.html) and the way they put people first. Which in the long-run provides great results for them.

## Is enforcing AI usage and checking their adoption in different ways the way to go?

Companies should definitely look for ways to include AI in all different levels as it helps a lot with productivity and will only get better over time.

But top-down direction and enforcing the usage may backfire if not realized correctly and having people in mind. Especially if the expectations regarding the AI productivity increase are unrealistic.

People may try to fake AI usage just to get “high scores” in performance reviews.

And also low psychological safety may result in people being reluctant to share knowledge, help each other, challenge assumptions and be motivated to keep getting better.

These are the things that need to be mitigated with an enforcing approach.

## Here is my suggestion to approach AI adoption

This is the way I suggest and it’s a bottom-up approach. Where you focus on creating a great high-performing culture and then share business problems and challenges and ask people and teams to solve them.

Naturally, people and teams will look for ways to be productive as much as possible and find the best solutions. That includes looking to use AI as much as possible.

> There is no better productivity hack for companies than a great culture. No AI can replace great culture.

There is a reason why I like to say that Software Development is a team sport and great teams build great software. The best and the most high-performing teams are self-driven to look for ways to be more productive.

So that is my advice as well. Create a great culture where people and teams help each other and share business problems and challenges with the teams.

I am often surprised with how creative the solutions are and what productivity increases happen when people are motivated and driven to provide as much business value as possible.

I’ll be sharing practical examples in the section “These are my suggestions for approaching AI adoption in engineering teams”.

But first, we’ll go into specific challenges that are important to understand with AI adoption.

## Key challenges with AI adoption

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fa6db6e-42b4-4534-b019-a4b94ee5cfe5_800x467.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9fa6db6e-42b4-4534-b019-a4b94ee5cfe5_800x467.jpeg)

These are the main challenges I have come across and seen/heard from various engineers and leaders:

-   Misalignment between leadership and employees on how to use AI
    

I’ve come across various examples where leadership haven’t truly understood the work that needs to be done on a daily basis, especially in engineering. And that misalignment can be problematic especially when doing top-down decisions.

That translates into the usage of AI as well. Where one side may think AI can be used in a certain way, but in reality that is not usable for the specific use case for certain reasons. That causes unnecesary friction.

-   Not truly understanding who is driving the AI adoption
    

This is also problematic, especially when doing top-down decisions. People may wait for directions, when instead the best way with AI adoption is that everyone is looking for ways to make their work easier and be more productive.

Of course, you need some key people or teams that will drive the adoption forward, but the mindset and direction should be clear.

-   The overall disconnect between leadership and employees on expectations regarding AI increasing productivity
    

We’ve mentioned the misalignment between leadership and employees on using AI above, the problem comes when the expectations are way different. In terms of leadership expect way more productivity gains than they are in reality.

Especially if that is connected with blaming or finger-pointing that AI is not utilized correctly. There may be a lot of tension because of that, where instead alignment and mutual understanding is key.

-   Lack of ROI
    

That is a reality in a lot of cases and is connected with expectations where leadership and stakeholders expect huge productivity improvements proportional to the time being invested.

AI is still in quite experimental phase where things are not fully ready for massive adoption. A good example is MCP, where there are a lot of upsides, but it’s still quite early for teams to fully embrace it.

-   Companies experiencing FOMO with AI
    

A lot of companies may force AI adoption due to the fear of missing out. They see what other companies are doing/saying and naturally they are prone to be doing something similar.

This may backfire as we mentioned above → just using AI because it’s a shiny new thing and everyone saying great stuff about it will not result in the best results.

It’s a similar comparison to using the shiny new JavaScript framework and immediately adopting that.

-   Some companies have problems using AI due to security reasons
    

I know a lot of companies have problems adopting AI due to the nature of their business → handling more sensitive data. Especially in the public sector, fintech and legal companies.

My recommendation is to take a look at LLMs that can be used on your own infrastructure → local/offline LLMs. Here are helpful tools: [LMStudio](https://lmstudio.ai/), [Jan](https://jan.ai/), [Ollama](https://ollama.com/), [GPT4All](https://www.nomic.ai/gpt4all), [Msty](https://msty.app/).

## These are my suggestions for approaching AI adoption in engineering teams

As mentioned above, my suggestion is the bottom-up approach and start by creating a great culture where people can thrive.

1.  Focus on creating a great culture
    

Start by defining the right core values of your organization. Here are my go-to values:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F32238080-f58d-4158-820f-1f8d4b0264b4_800x415.jpeg)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F32238080-f58d-4158-820f-1f8d4b0264b4_800x415.jpeg)

After that, focus on psychological safety, knowledge sharing, make sure the culture is blameless and inspire ownership and taking responsibility.

Especially the knowledge sharing aspect is crucial in AI adoption. More people sharing, means more people knowing and that’s where great culture blossoms, where everyone is selflessly sharing knowledge and helping others.

Here are more articles that would help you with creating a great culture in your engineering org:

[](https://newsletter.eng-leadership.com/p/how-to-create-a-culture-of-ownership)

[

![How to create a culture of ownership in your engineering team](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd9c86f02-0a0a-49cf-945e-5c72a9db7ee2_800x417.jpeg)

](https://newsletter.eng-leadership.com/p/how-to-create-a-culture-of-ownership)

[](https://newsletter.eng-leadership.com/p/blameless-culture-should-be-a-standard)

[

![Blameless culture should be a standard in the engineering industry](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe55c50c9-2383-4361-9c38-1437267de3c1_800x500.png)

](https://newsletter.eng-leadership.com/p/blameless-culture-should-be-a-standard)

[](https://newsletter.eng-leadership.com/p/how-to-create-a-great-engineering)

[

![How to create a great engineering organization where people can thrive](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F9c31efe3-d10e-4259-90d7-a873faeaa59e_800x327.jpeg)

](https://newsletter.eng-leadership.com/p/how-to-create-a-great-engineering)

2.  Start with WHY
    

It’s really important for everyone to understand what is the goal behind it all and that it’s clear for everyone why are we doing this. As mentioned above, if we are trying to adopt AI for the wrong reasons, it’s not going to be as effective.

Here are some key questions to ask yourself:

-   What problems are we trying to solve?
    
-   Where are the pain points or bottlenecks?
    
-   Is AI the right tool for this? Sometimes rule-based automation or process improvement is more appropriate.
    

3.  AI Champions e.g. go-to people
    

It’s natural that some people are more prone to innovating and working with latest tech like AI and these are the people that you should give more time to innovate and research.

The goal is to find different ways for everyone to use AI to make their day-to-day easier and share that knowledge with others via learning sessions.

My recommendation is to also have a space for these people (preferably a seperate channel on Slack) to share their findings with everyone.

4.  Specific AI team(s) focusing on AI adoption
    

Similar to AI Champions, it’s a team dedicated to researching and finding out how to utilize AI most effectively. Such teams are normally very small and these are definitely fun things to work on and a lot of engineers are interested in diving deeper into that.

The role of such teams is similar as with Platform teams → to unblock other teams and make their day-to-day work easier. With such teams, it’s really important to make it inclusive, so that everyone can contribute. They act as evangelists across the organization and curators of best practices.

Also transparency is very important as well → what specific things are they working on and what are the goals of such teams. Trying to make it a “secret” thing will make other teams wondering what actually is the reason behind creating these teams.

5.  Knowledge-sharing sessions and external training
    

With great culture, the overall knowledge-sharing aspect is there, but with AI adoption is really important to emphasize that. My recommendation is to encourage everyone to participate and share what they are learning.

A good way is to also start a company internal or even external blog where people are sharing their findings in a written way. As you know, I am a big advocate for writing and I believe good writing is a superpower for everyone.

With writing, people are sharing their knowledge and also improving their written communication.

Also, look for ways to provide external training to your people, there are many different available. I recommend checking out [Maven](https://maven.com/) (not sponsored), as I know a lot of the Maven’s team, how they operate and also the instructors there and I know their level of expertise.

## Last words

Let’s end the article with the following:

> AI adoption is very important, but culture is still key and most important when it comes to high performance.

With the right culture, possibilities are endless and AI adoption is one of them!

We are not over yet!

## Engineering is More About People than Tech

Check out my latest video. I am sharing how important people skills are in the engineering industry in order to provide value to the team and organization.

<iframe src="https://www.youtube-nocookie.com/embed/DBfJaHXO0TI?rel=0&amp;autoplay=0&amp;showinfo=0&amp;enablejsapi=0" frameborder="0" loading="lazy" gesture="media" allow="autoplay; fullscreen" allowautoplay="true" allowfullscreen="true" width="728" height="409"></iframe>

New video every Sunday. Subscribe to not miss it here:

[Subscribe to the channel!](https://yt.openinapp.co/exgpd)

Liked this article? Make sure to 💙 click the like button.

Feedback or addition? Make sure to 💬 comment.

Know someone that would find this helpful? Make sure to 🔁 share this post.

## Whenever you are ready, here is how I can help you further

-   Join the Cohort course Senior Engineer to Lead: Grow and thrive in the role [here](https://maven.com/gregor-ojstersek/senior-engineer-to-lead?promoCode=ENGLEADERSHIP).
    
-   Interested in sponsoring this newsletter? Check the sponsorship options [here](https://calico-cabinet-fbf.notion.site/Sponsor-Engineering-Leadership-fa0579535d6f4422a6da350580a54546).
    
-   Take a look at the cool swag in the Engineering Leadership Store [here](https://store.eng-leadership.com/).
    
-   Want to work with me? You can see all the options [here](https://calico-cabinet-fbf.notion.site/Work-with-Gregor-Ojstersek-1147b66fdc24809b86b1fb0467b60318).
    

## Get in touch

You can find me on [LinkedIn](https://www.linkedin.com/in/gregorojstersek/), [X](https://twitter.com/gregorojstersek), [YouTube](https://yt.openinapp.co/exgpd), [Bluesky](https://bsky.app/profile/gregorojstersek.bsky.social), [Instagram](https://www.instagram.com/gregor_ojstersek/) or [Threads](https://www.threads.net/@gregor_ojstersek).

If you wish to make a request on particular topic you would like to read, you can send me an email to info@gregorojstersek.com.

This newsletter is funded by paid subscriptions from readers like yourself.

If you aren’t already, consider becoming a paid subscriber to receive the full experience!

[Check the benefits of the paid plan](https://newsletter.eng-leadership.com/about#%C2%A7paid-subscribers-get)

You are more than welcome to find whatever interests you here and try it out in your particular case. Let me know how it went! Topics are normally about all things engineering related, leadership, management, developing scalable products, building teams etc.

#### Discussion about this post
