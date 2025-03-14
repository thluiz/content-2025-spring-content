---
created: 2025-01-21T14:31:57 (UTC -03:00)
tags: []
source: https://shopify.engineering/five-years-of-react-native-at-shopify?utm_source=tldrnewsletter
author: Mustafa Ali
---

# Five years of React Native at Shopify (2025) - Shopify

> ## Excerpt
> Five years ago, we announced that React Native (RN) is the future of mobile at Shopify. Today, we are excited to share the progress we've made, lessons learned, and what the future holds.
To recap, we decided to switch to RN for 3 main reasons:


Write it once - Stop building the same features twice, once on iOS and once on Android


Talent portability - Enable devs to work fluently across iOS, Android, and Web


Ship more value - Spend more time delivering value to users instead of chasing feature parity


We’re happy to share that our transition has been quite successful:

Not having to build the same features twice has given us a step change in productivity
Engineers are able to work across web and mobile allowing teams to do more with the same number of people and unlocked new growth opportunities
Maintaining feature parity between iOS and Android has become a non-issue, freeing up capacity to ship a lot more value
Our apps are blazing fast (<500ms screen loads) and stable (>99.9% crash-free sessions)
We continue to leverage native wherever it is the best tool for the job, giving us the best of both worlds

Over the past 5 years, we have migrated all our apps to React Native. Instead of using a one-size-fits-all approach to do so, each team chose when and how to migrate their app. This allowed them to continue shipping features while also aligning with our strategy of leveraging RN.

What did we learn? 
React Native apps are fast
We care very deeply about performance at Shopify. As our CEO Tobi Lutke says, “not all fast software is great, but all great software is fast”. The biggest question we had while switching to RN and the main reason we didn’t do it sooner was whether we’d be able to achieve our performance goals with it.
Before making the decision to switch, we did extensive prototyping which led to promising results. We also saw all the work that Meta

---
Five years ago, we [announced](https://shopify.engineering/react-native-future-mobile-shopify) that React Native (RN) is the future of mobile at Shopify. Today, we are excited to share the progress we've made, lessons learned, and what the future holds.

To recap, we decided to switch to RN for 3 main reasons:

1.  **Write it once** - Stop building the same features twice, once on iOS and once on Android
2.  **Talent portability** - Enable devs to work fluently across iOS, Android, and Web
3.  **Ship more value** - Spend more time delivering value to users instead of chasing feature parity

We’re happy to share that our transition has been quite successful:

-   Not having to build the same features twice has given us a step change in productivity
-   Engineers are able to work across web and mobile allowing teams to do more with the same number of people and unlocked new growth opportunities
-   Maintaining feature parity between iOS and Android has become a non-issue, freeing up capacity to ship a lot more value
-   Our apps are blazing fast (<500ms screen loads) and stable (>99.9% crash-free sessions)
-   We continue to leverage native wherever it is the best tool for the job, giving us the best of both worlds

Over the past 5 years, we have migrated all our apps to React Native. Instead of using a one-size-fits-all approach to do so, each team chose when and how to migrate their app. This allowed them to continue shipping features while also aligning with our strategy of leveraging RN.  

![](https://cdn.shopify.com/s/files/1/0779/4361/files/Shopify-ReactNative-App-Table.png?v=1736795681)

## What did we learn?  

### React Native apps are fast

We care very deeply about performance at Shopify. As our CEO Tobi Lutke says, “not all fast software is great, but all great software is fast”. The biggest question we had while switching to RN and the main reason we didn’t do it sooner was whether we’d be able to achieve our performance goals with it.

Before making the decision to switch, we did extensive prototyping which led to promising results. We also saw all the work that Meta was doing to eliminate performance bottlenecks and identified areas like lists where we could help. We predicted that RN will get much faster soon and went all-in.

Fast-forward 5 years, RN apps are fast. We’ve achieved sub-500ms (P75) screen loads in the Shopify app and you can learn how we did it [here](https://shopify.engineering/improving-shopify-app-s-performance), [here](https://www.youtube.com/watch?v=88Y1f6S7CGo), and [here](https://www.youtube.com/watch?v=bEfTgM6QL1E&ab_channel=InfiniteRed). We’ve achieved similar performance in all our apps. Just like native, you have to apply good patterns and techniques to eliminate performance bottlenecks.

After spending years building mobile apps at scale with both native and React Native, we’ve found that native doesn’t automatically mean fast, and React Native doesn’t automatically mean slow. RN has come a long way in the last few years and you can now use it to build world-class apps. 

We anticipate that this will continue to get easier as the framework matures and teams develop more expertise. We will continue to share our learnings with the community.

### Hot reloading is awesome

The ability to see your changes reflect instantly with RN has been a total game changer. This was one of our biggest pain points with native. Given the size of our code bases, it took several minutes for even the most trivial changes to be compiled and run on an emulator/physical device. This wastes time and breaks developer flow. React Native’s hot reloading completely eliminates this problem.

![](https://cdn.shopify.com/s/files/1/0779/4361/files/hot-reload_1.gif?v=1736794223)

### Typescript unlocks talent portability

Typescript has become ubiquitous, and we've seen great success with developers transitioning between React web and React Native. Web developers find it much easier to start working on mobile using RN, as opposed to native iOS and Android development. Similarly, mobile developers familiar with RN find it easy to work on the web. 

This flexibility not only opens up more growth opportunities for engineers but also increases staffing flexibility and enables teams to accomplish more with the same number of developers. It also opens up new opportunities to share code between web and mobile creating more leverage.

Having developers who can work on multiple platforms is incredibly valuable. It allows us to ship faster and enables developers to take good ideas from one technology and apply them to another in novel ways. It also creates a culture of viewing technologies as tools in a tool belt, broadens the team’s horizons, and encourages using the best tool for the job, instead of whatever you happen to be most familiar with. 

### Native devs are crucial

Mobile engineers who specialize in iOS and Android are essential to building great mobile apps. There is no replacing experience and taste that comes from having built many mobile products and deeply understanding conventions and usability. Being able to drop down to the platform layer, write bindings, master build & release, distribution, etc requires native expertise. 

They also play a vital role in optimizing app performance across the myriad of device models, ensuring a consistent user experience for all users. Additionally, native expertise is essential for managing React Native version updates, as well as adopting new features, APIs, and tooling changes that accompany new iOS and Android releases. You can't build a good product without these experts.

We invested in training our native mobile developers in React Native through a self-serve course that covered everything they needed to know to ship production-ready code. Additionally, we set up office hours with developers who were already proficient in React Native to provide support through Q&A sessions, pair programming, and code reviews.

We also supplemented our mobile teams with some web developers for their Javascript, Typescript, and React expertise. This ensured we had strong expertise in both native and React Native, and over time, it levelled up the entire team.

Having a good mix of native and web developers is the key to building great mobile apps using React Native in our experience. 

### Native code is crucial

100% React Native should be an anti-goal. It is great for building features just once, but is not the right technology for everything. 

Native is still the best way for building cutting-edge features that leverage device hardware like 2D / 3D scanning and running AI models on-device. It is also better suited for building features that have memory limitations like home and lock screen widgets, Apple Watch apps and complications, App Intents, and Siri Shortcuts.

Native is also the better choice for long-running background jobs. Our Point of Sale app, for instance, downloads and syncs a ton of data in the background so that it can process transactions even when offline. By leveraging native code, we were able to offload that work completely to the background, ensuring that it had no effect on app performance. Doing so is easy thanks to strong interoperability that RN provides out of the box.

Instead of thinking native _**or**_ React Native, think native _**and**_ React Native. 

We’ve found that you can save a ton of time by building most features just once using RN and then leverage native for things it is best suited for. This is also why having native expertise on the team is crucial.

### Debugging is worse

Debugging in React Native is flakey and configuring it correctly in VSCode takes some work. iOS and Android on the other hand have powerful debugging capabilities that just work. Meta recently did a complete [rewrite](https://reactnative.dev/docs/react-native-devtools)[](https://reactnative.dev/docs/react-native-devtools) of React Native’s debugger stack, which looks promising. We are collaborating with them to make it world-class.  

### React Native updates are not seamless

Updating an app to each new version of React Native takes a significant amount of work and often requires restructuring the code base. We have mitigated this by having a small group of rotating developers handle it while the rest of the team focuses on feature development. 

We expect this to get easier as the framework matures and the [New Architecture](https://reactnative.dev/architecture/landing-page)[](https://reactnative.dev/architecture/landing-page) is more widely adopted.  

### More reliance on 3rd party libraries

The React Native framework is not as comprehensive compared to native, so you end up having to use more 3rd party libraries. The ecosystem has matured a lot in recent years and it is easy to find well-maintained libraries for anything you might need. 

This, however, adds the overhead of keeping them up to date on an ongoing basis and increases the surface area of supply chain attacks. We have mitigated this by implementing automated dependency updates using [Dependabot](https://github.com/dependabot) and automated code scanning to catch malicious code. 

We’d love to see the framework get more opinionated and provide more capabilities out of the box.  

### Shared foundations unlock a ton of leverage

When we first started adopting React Native, we did not have years of experience of building mobile apps using RN like we did with native. We also couldn’t leverage the shared native foundations that we had built over the years, so each team built things their own way. 

It wasn’t worth investing in creating those shared foundations at the time either because we were still learning how to build apps in RN. We were beginners again and had to first put in the time and effort to develop expertise.

This approach was great to hit the ground running quickly and migrate our apps, but it also caused teams to solve the same problems multiple times, reinventing the wheel. This was a conscious tradeoff – we optimized for speed over consistency. 

By the end of 2023, all our apps were mature enough that we could start making them consistent. We have since started extracting common components like Identity, real-time monitoring, performance measurement, etc into shared libraries that all apps consume. 

This allows teams to avoid solving problems that have already been solved in other parts of the company. It also spreads knowledge and expertise, and every app automatically benefits from improvements that are made to these shared components. 

We will continue this work in 2025 to further increase the amount of code we share between our apps as it gives us tremendous leverage and frees up engineering bandwidth that can be applied to shipping more value to our users.  

## Future of React Native

The future of React Native is bright and we think Meta have been great stewards of the project. We are seeing regular improvements with every release and the roadmap is being heavily influenced by developer feedback. We anticipate that building fast apps will continue getting easier. We’re also stoked about the [New Architecture](https://reactnative.dev/architecture/landing-page)[](https://reactnative.dev/architecture/landing-page) and are committed to adopting it in our apps. 

The community is also thriving with several companies like Microsoft, Amazon, Tesla, SpaceX, and Coinbase using React Native and developers shipping high-quality 3rd party libraries and frameworks.

At Shopify, we believe that building software is a team sport and have a commitment to the open web, open source, and open standards. We do this by:

-   Making code contributions to React Native
-   Serving as co-release captains for RN releases
-   Sponsoring highly impactful open source projects like [React Native Skia](https://github.com/Shopify/react-native-skia)[](https://github.com/Shopify/react-native-skia) and Reanimated 
-   Publishing open source projects like [Flashlist](https://shopify.github.io/flash-list)[](https://shopify.github.io/flash-list), [Restyle](https://github.com/Shopify/restyle)[](https://github.com/Shopify/restyle), and [Tophat](https://github.com/Shopify/tophat)

Additionally, in 2025, we are restarting the React Native Working Group. This will bring together senior leaders who support RN in their organizations to identify key challenges in the ecosystem, prioritize investments, increase collaboration, and reduce duplicated efforts. Previously, members included companies like Meta, Twitter, Coinbase, and Microsoft. If you're interested in joining this effort, [get in touch](https://forms.gle/6aQQcfTKBj4fhyMHA)!

React Native has come a long way in the last 5 years and a lot of limitations that led people to not adopt it simply don’t exist anymore. If you haven’t tried using RN in a while, now would be a good time to revisit it. We are going to continue working with Meta and the rest of the community to make it better.

### About the author

Mustafa Ali is a Director of Engineering at Shopify

**X: [@mustafa01ali](https://x.com/mustafa01ali)**
