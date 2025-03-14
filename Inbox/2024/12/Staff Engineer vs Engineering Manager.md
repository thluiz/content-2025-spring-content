---
created: 2024-12-04T10:03:56 (UTC -03:00)
tags: []
source: chrome-extension://pcmpcfapbekmbjjkdalcgopdkipoggdi/_generated_background_page.html
author: Alex Ewerlöf
---

# 

> ## Excerpt
> When do you need a Staff Engineers? What's the accountability model with or without? What's the difference between Staff Engineer and Engineering Manager? What should the staff engineer be accountable for? These are the questions that this article tries to answer with lots of colorful illustrations and examples.

---
## Staff Engineer vs Engineering Manager

### When do you need a Staff Engineers? What's the accountability model with or without?

[![](https://substackcdn.com/image/fetch/w_80,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2Fe2713990-da82-481b-b579-01a7aaa5b85b_560x560.jpeg)](https://substack.com/profile/87732486-alex-ewerlof)

These are actual questions I’ve got in the past few years:

-   An EM: _I don’t want to deal with people anymore. I want to focus on the tech. Is Staff Engineering for me?_
    
-   A Senior EM responsible for multiple teams: _I have too much admin work. Should I hire a Staff Engineer to take over the technical part of my job?_
    
-   A Staff Engineer at a team without an EM: _I’m practically acting as the team manager and like it. Should I just switch lanes to become an EM?_
    
-   A Senior Engineer frustrated with Ivory Tower Staff Engineers: _what is the accountability of the Staff Engineers? They act like retired devs._
    
-   A new Staff Engineer: _What are dotted reporting lines? I have my line manager, why should I care about other managers?_
    

So I thought it’s time to settle all these questions once and for all.

**🤖🚫 Note: No AI is used to generate this content. This essay is only intended for human consumption and is NOT allowed to be used for machine training including but not limited to LLMs.**

[](https://blog.alexewerlof.com/p/introduction-to-the-role-of-staff)

[

![Introduction to the role of Staff Engineer](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38b0377a-c0ee-4776-8509-0b9ddcb133da_1071x1020.png)

](https://blog.alexewerlof.com/p/introduction-to-the-role-of-staff)

## When do you need a Staff Engineer?

Let’s step back and build a model.

Engineers create and maintain the tech. But the tech doesn’t exist in vacuum. It’s a solution to a _problem_. The problem is defined by the _product_.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd57da0b5-2a0e-4890-ad5b-3f02f085131c_1307x187.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd57da0b5-2a0e-4890-ad5b-3f02f085131c_1307x187.png)

The product offers a [service](https://blog.alexewerlof.com/p/service) to the consumers, whether they are end users or internal consumers. It solves a problem for someone. It can be:

-   An app that’s used to measure running distance and sharing it with other runners
    
-   A website for booking hotels
    
-   An infrastructure platform that’s used by other teams
    
-   A time-report system
    
-   etc.
    

The engineers usually work in a team led by an EM.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff39f7f41-87d9-4e8b-8d7a-095f1bc3e5d2_1366x878.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Ff39f7f41-87d9-4e8b-8d7a-095f1bc3e5d2_1366x878.png)

The Engineering Manager is accountable for the engineers as well as the technical artifacts they produce:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ab29b84-c4cc-40ef-afc4-648e163a062f_978x401.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0ab29b84-c4cc-40ef-afc4-648e163a062f_978x401.png)

When the team is large or the tech is too complex (or both), the EM may not have enough bandwidth to do justice for both their people and technical accountability:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F214b311d-1fa7-40c0-a3d8-e31b1da46c30_966x403.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F214b311d-1fa7-40c0-a3d8-e31b1da46c30_966x403.png)

To increase their bandwidth, EM’s are faced with a choice:

-   **Delegate admin work:** To optimize the HR processes and tooling or hire an admin assistant to lift some of the people management load from their shoulder
    
-   **Delegate technical load:** To hire a Staff Engineer to lift some of the technical load from their shoulder.
    

You may argue if having a dedicated admin assistant is justified at a team level, but I have seen one admin assistant shared by multiple teams. AI can cover some of the mechanical activities like scheduling events, answering admin questions, and gathering continuous feedback. Good HR tooling and processes can reduce the admin load as well.

Nonetheless, the admin assistant or AI is not supposed to take over the EM’s people accountability like building a good team, mentorship, performance review, hiring, etc.

Previously we’ve argued that hiring a Staff Engineer to take over the technical aspects of Engineering Management [is an anti-pattern](https://blog.alexewerlof.com/p/when-staff-engineer-is-an-anti-pattern).

Particularly, Staff is not supposed to act as a translation layer between the tech and engineers for the EM:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cb40120-cac8-4e3a-b960-0c6e1fb2bf65_987x470.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F5cb40120-cac8-4e3a-b960-0c6e1fb2bf65_987x470.png)

Engineering manager needs to be technical and we have covered some ways to keep the technical skills sharp [here](https://blog.alexewerlof.com/p/how-can-engineer-leaders-stay-technical).

[](https://blog.alexewerlof.com/p/how-can-engineer-leaders-stay-technical)

[

![How can engineer leaders stay technical?](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fd336a654-5f43-4308-9642-afff3ed99764_1354x860.png)

](https://blog.alexewerlof.com/p/how-can-engineer-leaders-stay-technical)

Staff Engineer could be useful when:

-   The technical load is beyond the capacity of EM. For example:
    
    -   Legacy tech that requires intense babysitting/migration
        
    -   Overly complex solution that spans across teams or requires deep technical knowledge
        
    -   Simply because the EM isn’t technical (it’s an anti-pattern but they do exist)
        
-   A clear accountability can be established for a subset of technical load
    

Organizations that have Staff, should hold them accountable for the technology, not people:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F058c4006-a030-40c2-9e73-4415830ea721_1125x495.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F058c4006-a030-40c2-9e73-4415830ea721_1125x495.png)

Please don’t read too much into this. 🙂 People are different. Products are different. And both change over time.

There’s a small but important [difference between responsibility and accountability](https://blog.alexewerlof.com/p/accountable-vs-responsible) that we’ve covered in a separate article:

[](https://blog.alexewerlof.com/p/accountable-vs-responsible)

[

![Accountable vs Responsible](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F8d21e22a-ac34-4510-bc50-368c52f6608a_500x673.png)

](https://blog.alexewerlof.com/p/accountable-vs-responsible)

The Staff Engineer should be deeper in the technology and have higher technical literacy tied to an accountability. The lack of people responsibility opens up their schedule to be more invested in tech. But the Engineering Manager is not supposed to be completely off-hand either.

So we went from this:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F80d335ec-f4f6-4007-b1f2-e806676cd6ad_1030x664.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F80d335ec-f4f6-4007-b1f2-e806676cd6ad_1030x664.png)

To this:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8b9282c-2605-482d-ab15-69122bf4f04b_1119x710.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe8b9282c-2605-482d-ab15-69122bf4f04b_1119x710.png)

Now I have some bad news. Having Staff Engineer at the team level is an overkill. As we’ll discuss in a minute, the real value of the Staff Engineer comes from operating across teams. Besides, having two roles accountable for tech can create confusion and breaking one role into multiple titles:

[](https://blog.alexewerlof.com/p/breaking-role-to-titles)

[

![Breaking a role to multiple titles](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe0fa53a7-a072-4dfd-9002-5dfd0ae03424_941x940.png)

](https://blog.alexewerlof.com/p/breaking-role-to-titles)

However, there are cases when a Staff Engineer may operate in the scope of one team. For example:

-   Legacy tech stack with a steep learning curve when the EM is new
    
-   When [onboarding a new Staff Engineer](https://blog.alexewerlof.com/p/onboarding-staff-engineers) (starting from a smaller scope)
    
-   Too much [tech debt](https://blog.alexewerlof.com/p/tech-debt-day) piled up and hurting the tech health metrics
    
-   Complexity getting in the way of maintaining the tech
    

In all those examples, team-scoped Staff Engineer can be a temporary position.

## Staff Engineer for a system spread across multiple teams

In his seminal essay Will Larson, identified [4 archetypes for Staff Engineers](https://staffeng.com/guides/staff-archetypes/):

1.  Tech lead
    
2.  Architect
    
3.  Solver
    
4.  Right hand
    

What’s shown in the diagram above is a _Tech Lead_ operating within one team. Calling that role a Staff Engineer is a bit of a stretch.

[](https://blog.alexewerlof.com/p/introduction-to-the-role-of-staff)

[

![Introduction to the role of Staff Engineer](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F38b0377a-c0ee-4776-8509-0b9ddcb133da_1071x1020.png)

](https://blog.alexewerlof.com/p/introduction-to-the-role-of-staff)

That’s because most of the value of the Staff Engineer comes from operating across _multiple_ teams:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc87ab478-fcf0-4577-a90c-e7c8e4d28aa5_1123x1123.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fc87ab478-fcf0-4577-a90c-e7c8e4d28aa5_1123x1123.png)

Often, the tech that is owned by one team is just a piece of the puzzle. The larger system or domain may be spread across multiple teams or at least has tight dependencies across the org.

In this scenario it makes sense to have a dedicated role (Staff Engineer) for the entire system regardless of which team owns which piece of it.

Here’s the catch: the “tech” doesn’t talk or listen:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75e75d2b-7494-456b-8141-bd4d0ac45ef3_1123x942.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F75e75d2b-7494-456b-8141-bd4d0ac45ef3_1123x942.png)

The tech doesn’t exist in vacuum!

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F760dfa9d-4dd2-4df0-baa9-e53e643c5deb_1055x161.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F760dfa9d-4dd2-4df0-baa9-e53e643c5deb_1055x161.png)

For Staff engineers to deliver their true value, they have to go through both people (engineers) and product:

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F74b05af3-5874-4109-947a-db4bc5c7a7a3_1123x941.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F74b05af3-5874-4109-947a-db4bc5c7a7a3_1123x941.png)

And that’s why Staff Engineers have all those “dotted reporting lines” (the unofficial but critical collaborations and commitments across the org):

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa2a03ef9-1518-4faa-b781-596da86f7e4a_1122x1111.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fa2a03ef9-1518-4faa-b781-596da86f7e4a_1122x1111.png)

Those with the kin eyes may ask why the arrow is pointing from the Staff to “lower” positions (in the career ladder). The answer is two-fold:

1.  Good leadership is about collaboration not authority. See it as Staff going to a team-level EM or PM and asking for favors in a way that fits the engineers and product roadmap, not demanding to lift the state of tech in isolation
    
2.  Staff Engineer is an IC (individual contributor) and by definition does not have any direct reports. If you’re lucky to have a sync channel with EM/PM, please don’t waste it in regular report, but instead turn it to a 2-way dialogue about the state of tech (status quo) and where the tech needs to be to solve the emerging product problems (vision) as well as cohesive actions to take us there (organizational tech strategy).
    

All these reporting lines can be overwhelming. One tool to align product teams across the org is a [strategy document](https://blog.alexewerlof.com/p/strategy-basics):

[](https://blog.alexewerlof.com/p/strategy-basics)

[

![Strategy basics](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fbucketeer-e05bbc84-baa3-437e-9518-adb32be77984.s3.amazonaws.com%2Fpublic%2Fimages%2F5742d3bf-435f-4f93-a7d5-d0df2f5aa4e0_1150x539.png)

](https://blog.alexewerlof.com/p/strategy-basics)

There are other ways to reduce the span of tech to one team through deliberate [organizational architecture](https://blog.alexewerlof.com/p/organization-architecture). We’ve discussed one powerful alternative in Kebab vs Cake model using _consumer journeys_:

[](https://blog.alexewerlof.com/p/kebab-vs-cake)

[

![Kebab vs Cake organization](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fdf73693a-fa77-4abf-b997-a1e85e05becc_1039x1039.png)

](https://blog.alexewerlof.com/p/kebab-vs-cake)

Staff Engineers should have clear accountability over a system. That means they expose [3 elements of ownership](https://blog.alexewerlof.com/p/you-build-it-you-own-it):

1.  **Knowledge:** They know the tech stack, the product problem and can speak fluently about it as well as code when needed
    
2.  **Mandate:** They have a say in how the tech evolves and is maintained.
    
3.  Responsibility: They are held accountable for the health of the tech (e.g. incidents, tech debt, documentation, tech defragmentation, etc.)
    

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe385616c-7dbf-450d-ab11-68bed061e2a0_1018x855.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2Fe385616c-7dbf-450d-ab11-68bed061e2a0_1018x855.png)

As we [discussed before](https://blog.alexewerlof.com/p/senior-engineer-to-staff-engineer), Staff Engineer is not a pure technical role and heavily relies on soft-skills to influence the technical organization.

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F87ea54d9-2d33-4218-b287-e1cf481598f8_916x943.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F87ea54d9-2d33-4218-b287-e1cf481598f8_916x943.png)

Funny enough, focusing too much on soft-skills is one of the most dangerous pitfalls ahead of Staff Engineers. It turns them to [Ivory Tower Architects](https://blog.alexewerlof.com/p/ivory-tower-architect):

[

![](https://substackcdn.com/image/fetch/w_1456,c_limit,f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31ba8449-177c-4344-9aaa-11ecabda939c_924x947.png)

](https://substackcdn.com/image/fetch/f_auto,q_auto:good,fl_progressive:steep/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F31ba8449-177c-4344-9aaa-11ecabda939c_924x947.png)

## Conclusion

Not all organizations need Staff Engineers, especially when:

-   EMs are technical enough to manage their team’s tech
    
-   Tech stack is healthy enough that doesn’t need a dedicated technical steward
    
-   The engineering organization is mature enough to find the best way forward for the system they collectively own without the need for a dedicated owner
    
-   For orgs that have Staff Engineers, they should clarify the technical ownership and hold the Staff Engineer accountable for it
    
-   [Ivory Tower Architect](https://blog.alexewerlof.com/p/ivory-tower-architect) is bad
    
-   [Breaking a role to multiple titles](https://blog.alexewerlof.com/p/breaking-role-to-titles) is inefficient
    
-   [Non-technical EM](https://blog.alexewerlof.com/p/when-staff-engineer-is-an-anti-pattern) is inefficient
    

And just like any essay by some random person on the internet, please please please use your best judgement and don’t get stuck in [cargo culting](https://blog.alexewerlof.com/p/cargo-culting).

[](https://blog.alexewerlof.com/p/cargo-culting)

[

![Cargo culting](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F3f894754-a978-453f-a01b-8af410354ab0_1120x1121.png)

](https://blog.alexewerlof.com/p/cargo-culting)

I didn’t write some universal laws of physics. You know what is best for your particular [technology, product, operations, and people](https://blog.alexewerlof.com/p/t-pop).

[](https://blog.alexewerlof.com/p/t-pop)

[

![T-POP](https://substackcdn.com/image/fetch/w_140,h_140,c_fill,f_auto,q_auto:good,fl_progressive:steep,g_auto/https%3A%2F%2Fsubstack-post-media.s3.amazonaws.com%2Fpublic%2Fimages%2F0c32abcf-7618-42bf-83a5-77313adcf320_1024x1024.webp)

](https://blog.alexewerlof.com/p/t-pop)

If you have counter-arguments, feedback, corrections, or questions, let me know in the comments down below. 🙏

If you find this post insightful, please share it in your circles to inspire others. 🙌

[Share](https://blog.alexewerlof.com/p/staff-engineer-vs-engineering-manager?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjo4NzQ3Mjg5LCJwb3N0X2lkIjoxNTIxMDMyNjksImlhdCI6MTczMzMwNjgyNywiZXhwIjoxNzM1ODk4ODI3LCJpc3MiOiJwdWItMTAwMjI2NSIsInN1YiI6InBvc3QtcmVhY3Rpb24ifQ.xJPHVhz4TNkHjkwxMb-HBDQkg3yD1HX2xagE2NvUMC4)

You can also share your thoughts and [comments on HackerNews](https://news.ycombinator.com/item?id=42290142).

_[My monetization strategy](https://blog.alexewerlof.com/p/faq#%C2%A7payment) is to give away most content for free. However, these posts take anywhere from a few hours to a few days to draft, edit, research, illustrate, and publish. I pull these hours from my private time, vacation days and weekends._

_You can support my work by sparing a few bucks for a paid subscription. As a token of appreciation, you get access to the Pro-Tips sections as well as my online book [Reliability Engineering Mindset](https://blog.alexewerlof.com/p/rem). Right now, you can get 20% off via [this link](https://blog.alexewerlof.com/protipsdiscount). You can also [invite your friends](https://blog.alexewerlof.com/leaderboard) to gain free access._

### Subscribe to Alex Ewerlöf Notes

Launched 2 years ago

Technical Leadership, Reliability Engineering, Growth

#### Discussion about this post
