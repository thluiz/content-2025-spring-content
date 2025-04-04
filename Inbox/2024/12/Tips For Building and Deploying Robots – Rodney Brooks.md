---
created: 2024-10-01T11:33:29 (UTC -03:00)
tags: []
source: https://rodneybrooks.com/tips-for-building-and-deploying-robots/?utm_source=tldrnewsletter
author: 
---

# Tips For Building and Deploying Robots – Rodney Brooks

> ## Excerpt
> This post is not about research or developing software for robots. Instead it is some tips on how to go about building robots for mass deployment and how to leverage those deployed robots for improving your product.

---
This post is not about research or developing software for robots. Instead it is some tips on how to go about building robots for mass deployment and how to leverage those deployed robots for improving your product.

The four tips are straightforward but I explain them more below.

1.  Use other people’s supply chain scale wherever possible.
2.  Changing infrastructure up front eases robot success but kills ROI for customers and their ease of buying decisions.
3.  New infrastructure already developed for people is often good infrastructure for robots.
4.  Deployed robots can collect real world data.

##### 1\. Other People’s Supply Chain Scale

When you start deploying a robot it is unlikely to be initially in large numbers unless it is a very low price. When we started selling Roombas at iRobot our first manufacturing batch was 70,000 robots, which is enormous by most robot deployment standards. We got to millions of robots per year. But these numbers are unusual for robots. Even so we got supply chain economies of scale at the millions we did not have at the tens of thousands.

How are you going to get economies of scale when you are shipping just a couple of robots per month, or even tens per week? And really how big an influence can you have over suppliers even when the volume is enormous, for you, at a small number of hundreds of robots per week?

My advice is to find someone else’s big juicy supply chain and grab a hold of that for yourself. Juicy in this context means three things. (1) Supply chains with significant downward price pressures. (2) Supply chains with enormous volumes. (3) Supply chains with many different suppliers, supplying standard parts, and all competing, and operating, in multiple geographies.

For example, you can use exactly the same motors that someone else is using in the millions per year, preferably a standard, and preferably ones that are built by multiple suppliers. Then buy those same motors from one or more of those standard suppliers, though first make sure they have adequate quality control and and quality guarantees. Ten years ago it made sense to get custom windings for motors in Eastern Europe. Now you should try to use existing motors from large scale consumer products.

Do the same for battery packs, or at least battery cells. Fifteen years ago it made sense to have unique battery chemistries both for robot performance and for the regulatory requirements at the geographical point of deployment. Now there are so many at scale different battery cells and chemistries available for at scale consumer products (including electric vehicles) it makes sense to pick the closest fit and use those in your robot.

Do the same for every other part of your robot that you can.

Doesn’t this make designing the production robot harder, getting standard parts rather than ones optimized for your robot? Yes, and no. It makes design a slightly different process, but one that rewards you handsomely in terms of lower BOM (Bill Of Materials) cost, and in stability of supply. Those of us (which is just about everyone) who went through the turbulence of COVID-era supply chains can testify to how that turbulence can kill the stability of your careful designs.

Oh, and by the way, you are already, to at least a limited extent, practicing this type of design. For your fasteners no manufacturing engineer is going to let you get away with deciding on an M5.3 bolt \[an M5 bolt is 5mm, and an M6 bolt is 6mm, and they are standard sizes, with no standard in between\], with a custom head, and a custom pitch, and a custom length of 10.27mm. No, they are going to insist that you use an M5 or an M6 bolt, with one of the many standard heads and pitches and one of the many standard lengths. And the reasons for that are precisely the same as the reasons I gave above for motors and battery cells, and processor boards, connectors, etc. There is an enormous supply chain for standard sized bolts, so they are way cheaper than custom sized bolts, by two or three orders of magnitude.

##### 2\. Changing Infrastructure

I have a rule when designing robots that are going to be deployed to real customers. No one in the company is allowed to say “the customer/end-user can just …”. If we are asking a customer or end-user to do something they wouldn’t naturally do already we are making it harder for them to use our product, compared to what they are already doing, or making it more expensive for them to install our product. This is true of both user interfaces and infrastructure.

As a business you need to decide whether you are selling infrastructure, or selling something that goes into an existing environment. A robot that can move about is a powerful thing to add to an existing environment because it provides mobility and mobile sensors without, in general, having to change the infrastructure.

The Roomba was an easy sale, not just for its price, but because it did not demand that people had to install some infrastructure in their home environment before they could have a Roomba clean their floors. All that a Roomba needed was a standard electrical outlet into which a human could plug its charger. Nothing else.

Since we have been working at [Robust AI](https://robust.ai/) on a new class of autonomous mobile robots for warehouses and factories, many people (mostly outside of the company, and not potential customers) have suggested all sorts of infrastructure changes that would make it easier to deploy our robots. These have included rubber floors, floors with a wireless recharging system underneath them so the robots can stay charged at all times, radio or laser beacons for localization of the robot, QR codes on every pillar, also for localization, structured lighting systems for the whole interior of the building, separate floors for human driven forklifts and the robots, etc., etc.

All these things would be additional expense for deploying the robots. It would have three bad impacts. (1) There would be a pre-phase for deployment where the infrastructure would have to be built into the existing facility, causing delay and in some cases downtime for the existing facility. (2) Lower ROI (Return On Investment) for the customer as one way or another they would end up paying for the change in infrastructure. (3) A much harder buying decision for the customer both because of a longer deployment delay and because of additional cost.

Your robot business will be much simpler if you don’t need changes in infrastructure.

##### 3\. But There Is Good News On Infrastructure

As technology advances we humans build new infrastructure for us. Often that same infrastructure turns out to be useful for robots.

When people first had autonomous vehicles driving on freeways (back in the 1980’s!! — see the work of Ernst Dickmanns) there was not much external infrastructure, besides white lines painted on the roads, to help the vehicles. But since then a lot has changed, with new infrastructure that has been installed to help human drivers. We now have GPS, and digital maps, so that our human driven cars know exactly where they are on those maps, and can display it to us, and search the maps for recommended routes. In addition there is congestion information that gets sent to our human driven cars and our phones, which let our route planners suggest faster routes. And the systems in some cars know the speed limits everywhere without having to read the speed limit signs. All these capabilities make it much easier for robot cars, autonomous vehicles, to be able to drive around. That human assist infrastructure has been a real boon towards getting to autonomous vehicles.

In the early days of robots for hospitals and for hospitality (i.e., hotels) and for managed care facilities for the elderly, it was first necessary to install Wi-Fi in the buildings as it was needed for the robots to know what to do next. That was a big barrier to entry for those robots. Now, however, the humans in those environments expect to have Wi-Fi available everywhere. It started to be deployed in hospitals to support the handheld devices supplied to doctors and nurses so they could record notes and data, but now patients and their visitors expect it too. The same with hotel guests. So requiring Wi-Fi for robots is no longer that much of a barrier to adoption.

In warehouses and factories the hand held bar code scanners have forced the introduction of Wi-Fi there. And in order to make it easy for humans to push around carts, as they do in both those environments, the floors are now generally trash free, and the slopes and lips in the floors are all quite small–infrastructure introduced for the benefit of humans. But this infrastructure, along with plenty of lighting for humans so they can see, make it much easier to introduce robots than it would have been a few decades ago, even if we had had robots back then.

Look for human driven infrastructure changes in whatever domain you want to have your robots work, and see if the robots can make use of that infrastructure. It will likely make them better.

##### 4\. deployed Robots Live In A Sea Of Real World Data

The last few years have seen a clear rise in the utility of machine learning. (Just for reference the term “machine learning” first appeared in 1959 in a paper on checkers written by Arthur Samuel; I wrote a really quite bad master’s thesis on machine learning back in 1977.) The web has provided massive amounts of text for machine learning which has given rise to LLMs. It has also supplied images and movies but they, and other images and movies explicitly collected for the purpose, have required human labeling. Recently people have had sensors attached to them and have been asked to do physical tasks to provide data for research projects (they are “research” projects even if done in well funded companies, as this data has not yet led to deployed robots) aiming to use machine learned control systems for robots (it is not clear to me that the type of data currently collected, nor the assumptions about the form of those control systems, will lead to useful results).

If you have deployed robots then data collection with labels may be available to you pretty much for free.

If your robots have cameras on them, then once they are deployed they are able to observe lots of real world things happening around them. If they are doing real work, then those video feeds are synchronized to what is happening in the world. and to external data interactions that the robot has with the work management system installed in the workplace. Thus the visual data is pre-labelled in some way, and can be used to feed learning algorithms with vast quantities of data over time.

Here is an example of some collected [data](https://www.linkedin.com/posts/robust-ai_ai-activity-7244703450171604993-8bKF) that is used to feed learning algorithms at Robust AI.

Get your robots deployed then have them learn from real world data to improve and extend their performance.
