---
created: 2024-09-19T10:25:27 (UTC -03:00)
tags: []
source: https://verygood.ventures/blog/boring-code-part-1?ref=dailydev
author: Jorge Coca
---

# Are you saying that my code is boring? Thank you!

> ## Excerpt
> We break down why "your codebase is boring" is the best compliment an engineering team can receive.

---
  
I spend a lot of time thinking about code. Good code. Code that is so easy to read and follow that is impossible for the person reading it to think “Hey Jorge, the intent of this snippet is not clear to me, could you please explain it to me? Let’s jump on a Zoom call!” Because let’s be honest, we’re all Zoomed-out.   

So, that leaves me wondering: “What makes code **_good_**?” 🤔 Is it how clever it is? Or how efficient it is? Or is it because I can just write a one liner to run the entire project? This question has been haunting me for a long time, until one day, it hit me:

## **This codebase is boring! 💯**

This ability to be able to find standard and reproducible patterns sparked one of the most interesting conversations I have ever had with an engineering team: during a retrospective session, a team member left a sticky in the column _Things that went well_ that said **“This codebase is boring!”** 

And boom!💥 just like that, I realized that that was the answer I was looking for: _boring code!_

We work with a lot of engineering teams of all sizes: from early stage startups to big corporations, and our mentality is to always offer the same level of quality, attention to details and scalability to our partners. We aim to build apps that are scalable so that they can grow and change with the company.

You would say that we have a “repetitive” model: something that we can implement efficiently over and over again. We focus on finding standard solutions and patterns that we can replicate: if we already solved a problem in a project, we can modularize it and turn it into a general package that can be used when other projects are trying to do the same thing.  

## **Are you crazy!?🤪 How can _boring_ code be _good_ code?🤓**

Most of the things that we do in a mobile application are, let’s be honest, more or less the same: fetch data from somewhere, apply some transformations, cache it, and then present it on the screen. And many of the features included in an application are also the same, regardless of their business line: create an account, authenticate, show me terms and conditions, logout the user, etc. 

Producing **_boring code is the biggest compliment that an engineering team can receive_**; after all, “surprises” in a project tend to be _not so good_ surprises. No one likes receiving a 2a.m. emergency call, or spending 8 hours tracking down a bug that is almost impossible to reproduce and as engineers, we don’t like solving the same problem over and over again. 

Having a codebase that is predictable, easy to navigate, well tested and properly automated makes it **_boring. But pleasantly boring!_** And that’s great, teams love the feeling! It lets them focus on their **_real and business-related challenges_** without the distractions of other tasks.  

## **How can I make my codebase boring?👩💻👨💻**

From our point of view, there are two key components to make your codebase good… I mean _boring_ 😉:  

-   **Super declarative, crystal clear, zero-surprises APIs**: Each component of your project that is exposed publicly should not leave the consumer thinking “_Will this really work?_” Instead, the feedback you want them to have should be _“I can’t believe this was so easy to do!”_  
    

-   **Ask yourself: _“Does this code belong here”?_** This question goes beyond the classical _“Should this code be written in the backend, or in the client?”_ Within your client code, you should make a distinction between **_data acquisition_** components (network, database, GPS, bluetooth, camera), your **_business rules_** (e.g. I need to get my current location to fetch from the network all the open restaurants that serve Spanish tapas near me) and how to **_present that information_** to the user (in the case of Flutter, this is how you use your widgets and how to manage their state).  
    

With those two constraints guiding our development process, we could easily imagine any application in the world being modeled by those principles. For example, a food delivery app could be decomposed into components like this:



![Components of a boring code base](https://cdn.prod.website-files.com/5ee12d8e99cde2e20255c16c/5f4e7b8aa58b7eb9ecc147a7_image2.png)

Components of a boring code base

The only parts of this application that should be unique are the business rules and the presentation components: after all, it is in the business rules where products offer **_value_**, and it is on the presentation side where products are **_pleasant_** to use. Data acquisition components, however, could be reused by many applications over and over again. 

This approach can be applied to any Flutter application, from the simplest example you can think of to the enterprise platform with millions of users. We’ve repeated this pattern **_over and over again,_** so many times that it is a bit _boring_ to apply now! 🥳 And we are so happy to have found boredom in development, because now our teams can execute efficiently, be predictable, and there are no more surprises!

## **We want to help you be _very_ _boring!_**

**__**Part of our core values is to share with the community the challenges we already faced and solved so we can all keep working on more exciting and complex challenges. We are planning to release a series of articles detailing how we turn our code into **_good, boring code._** Stay tuned!
