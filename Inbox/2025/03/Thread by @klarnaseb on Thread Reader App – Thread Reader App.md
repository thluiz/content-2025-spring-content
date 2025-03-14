---
created: 2025-03-04T09:15:58 (UTC -03:00)
tags: [twitter,twitter thread,unroll,threadreader,threads]
source: https://threadreaderapp.com/thread/1896698293759230429.html?utm_source=tldrnewsletter
author: 
---

# Thread by @klarnaseb on Thread Reader App – Thread Reader App

> ## Excerpt
> @klarnaseb: Yes, we did shut down Salesforce a year ago, as we have many SaaS providers—an internal estimate is about 1,200 SaaS shut down. No, I don't think it is the end of Salesforce; might be the...…

---
Yes, we did shut down Salesforce a year ago, as we have many SaaS providers—an internal estimate is about 1,200 SaaS shut down.

No, I don't think it is the end of Salesforce; might be the opposite.

Here is what actually happened and how/why we originally intended to NOT share it publicly:

At Klarna, we decided early to explore the potential of AI and LLMs—mostly ChatGPT—while being open to testing all things that seemed to be trending.

We encouraged all employees to do so and allowed them to pursue ideas organically rather than following "management direction" on exactly what they should be building.

In the early days of ChatGPT, we heard a lot:  
"this tool allows you to feed all your PDFs, all your data sources to a LLM!"  
However, the old universal truth of data scientists still holds true, even in AI: "shit in, shit out."

Feeding an LLM the fractioned, fragmented, and dispersed world of corporate data will result in a very confused LLM.

We started instead exploring a few key concepts: What of our data was actually valuable? What data was duplicative, incorrect, or contradicting? Why was it like that?

While people nowadays can criticize things like Wikipedia, we also reflected on the fact that it is a remarkable achievement—having over 20,000 people collaborate on the largest graph of knowledge that is still fundamentally of high quality, accessibility, and accuracy. What could we learn from this?

A Swedish company, @neo4j, and @emileifrem introduced us to the beautiful world of graphs.

We further explored data modeling, ontology, and, of course, vectors, RAGs, and many things.

Key to our explorations became the conclusion that the utilization of SaaS to store all forms of knowledge of what Klarna is, why it exists (docs), what it tries to accomplish (slides, tickets, kanban boards), how it is doing (sheets, analytics), who is it dealing with (CRM, supplier management), who works here (ERP, HR) and what it has learnt was fragmented over these SaaS—most of them having their own ideas and concepts and creating an unnavigable web of knowledge that required a tremendous amount of Klarna specific expertise to operate and utilize.

We also recognized that enterprise software has a standard set of features that are vital for it to operate—features such as audit, versioning, access and edit management, and similar universal needs. We need them as well, but that fragmentation again adds friction, admin overhead, and more.

So, we decided to start consolidating; to put things together, connect our knowledge, and remove the silos. The side consequence of this was the liquidation of SaaS—not all of them, but a lot of them. And not for the license fees, even though those savings have been nice, but for the unification and standardisation of our knowledge and data.

So no, we did not replace SaaS with an LLM, and storing CRM data in an LLM would have its limitations. But we developed an internal tech stack, using Neo4j and other things, to start bringing data=knowledge together.

Ultimately, we found this very interesting, but more importantly starting seeing serious productivity gains. We allowed our internal AI to use this knowledge, and we realised with the help of @cursor\_ai we could quickly deploy new interfaces and interactions with it.

So, I discussed with one of my board members: should we share this publicly?

We decided not to. We hold no grudge against SaaS (not true—I hate some of it, but won't tell you which one). But we are a payments company and a neo bank, there is limited value for us to share this externally.

However, Klarna, being a bank, holds quarterly calls with its investors, and in passing on of these calls, I mentioned that we had removed some SaaS software including Salesforce. It turns out that the recording was leaked to @SeekingAlpha, and they put out a news post about it. And from there, it went crazy.

Suddenly, @Benioff was asked on stage why Klarna was leaving Salesforce. I was tremendously embarrassed.

So, to summarise, what does this mean? Will all companies do what Klarna does? I doubt it. On the contrary, much more likely is that we will see fewer SaaS consolidate the market, and they will do what we do and offer it to others. Those are likely to be your next SaaS.

And it is very likely that Salesforce will be one of those companies. As highlighted many times, they do so much more than CRM today and hence have the opportunity to become that hub of knowledge that modern companies will seek.

But there are also risks for them and others; a lot of our large enterprise SaaS providers suffers from a fallacy. They started as companies with a clear opinion of how to do things, but over time, as they try to satisfy every whim of any random person working at any large enterprise, they become somewhat of a glorified database and lose their opinion. Opinionated software is worth something, as opinions represent an experience of what works, what produces results. And this is the ultimate value.

So I hope with sharing this we can clarify a lot of speculation and misunderstandings and in the end same thing as is always true, just like when mobile came along, we talked about mobile first, now you need to be AI first. Of course all SaaS companies will need to learn adopt and evolve. But if they do there is tremendous opportunity ahead.
