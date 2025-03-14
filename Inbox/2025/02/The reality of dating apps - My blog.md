---
created: 2025-02-06T08:37:20 (UTC -03:00)
tags: []
source: https://blog.luap.info/the-reality-of-dating-apps.html?utm_source=tldrnewsletter
author: Paul
---

# The reality of dating apps - My blog

> ## Excerpt
> I have been working inside a dating app for a few months and there are a lot of interesting things. This article is going to be without any structure as there are many things to say. I will mostly ignore homosexual interactions. Not because they don't exist, just because it …

---
I have been working inside a dating app for a few months and there are a lot of interesting things.

This article is going to be without any structure as there are many things to say.

I will mostly ignore homosexual interactions. Not because they don't exist, just because it works very differently than heterosexual ones. The likelihood to like and exchange inside homosexual groups is much higher than in heterosexual ones. Which makes creating a dating app for homosexuals something almost without product challenges (other challenges yes, but not product).

## User Ranking

There was a lot of discussion about ELO score and whether or not ELO is used at Tinder. Well, I don't think it is used at Tinder as it is not necessary; you don't need something complex to rank people, you can just look at the like-to-pass ratio on a profile. If 100 people see the profile of a user, how many like them? It is actually pretty reliable; a very fine ranking that the ELO score could provide doesn't bring anything more than this basic ratio. But this score, at least for women, is not a good representation of how attractive guys will find girls in real life. I often joke that the ratio of a girl is proportional to the amount of skin you see in her pictures (which is not actually true; it is the best strategy as a girl to increase ratio, to show more skin, but the best-ranked girls are not always full of skin).

This ratio is quite different for men and women. Here is the distribution below: the median for men is 5% and for women is 38%.

Men ![ratio users](https://blog.luap.info/static/dating/distribution_ratio_men.png)

Women ![ratio users](https://blog.luap.info/static/dating/distribution_ratio_women.png)

The other thing that interests you is the like ratio, or the openness, among 100 profiles that the user sees, how many of them does he like? (The median for men is 26% and for women is 4%.)

Men ![ratio users](https://blog.luap.info/static/dating/like_ratio_men.png)

Women ![ratio users](https://blog.luap.info/static/dating/like_ratio_women.png)

One interesting thing about like ratio:

-   The like ratio of a girl is almost independent of the profiles she sees. For example, if a girl has a like ratio of 5% and you remove 50% of the profiles, even if you remove only the profiles she will not like, her like ratio will still be 5% (you can do that by removing very unattractive people for a guy that is very attractive, for example). It is funny to observe, but it seems like a girl has internal reasoning on a dating app, and they know they can only like x% of profiles whatever she sees (of course, it doesn't work if you show only ugly people).
    
-   What that means is that the decision to like a guy is dependent on the profiles you have seen before. So if you are a guy and you want to get liked by hotter girls, then don't be around hotter guys (in general, and this one is my personal opinion, but I think physical appearance is a criteria much more important for men than women).
    
-   Like ratio and ratio have some correlation for girls; the most attractive have the lowest like ratio, but this is no rule either. There is also some correlation for men but lower. For men, this is usually difficult to make reliable stats because ratios are super unstable; one more like can change a lot of things, and that really depends on which girl sees your profile.
    

Men Like ratio (y) vs ratio (x)

![ratio users](https://blog.luap.info/static/dating/men_ratio_vs_like_ratio.jpg)

Women Like ratio (y) vs ratio (x)

![ratio users](https://blog.luap.info/static/dating/women_like_ratio_vs_ratio.jpg)

## User behavior

Match to talk was 35% in our case but was easy to move in one way or another. People were not speaking for very long conversations in general, they were happy to start a chat, but never really cared to continue it. Which always makes me say, that people registered on these apps are not really here for dating, but more for entertainment purposes.

Having a profile (Other information than pictures) doesn't impact the app experience (Even being one of the most requested features), most guys don't open the profiles of a girl, and girls open much more often but more for discarding a user than for looking for more information, so your like ratio will reduce if you have a profile. It makes starting a conversation easier, but people would have talked to each other anyway so.

But I believe people in general like to fill out a profile so it is important to have one for their experience (more than for the opposite user's experience).

Looks of users are more important to young girls than for older girls, older girls check the profile as much as they like, twice more than young girls.

Some young guys also have a ratio of more than 70, which doesn't exist for older (more than 25 years old) guys. Which makes more or less sense, we expect younger girls trying to be like the norm and all finding the exact same guy attractive.

Girls scroll about 2 times more than guys. (Fortunately). First, the like ratio of girls is 4%. So to be able to like 4 guys they have to see 100 users. And for a guy, it is 18 users. It also means the experience on a dating app for a girl is completely different than the one for guys. A girl spends a lot of time just searching for guys she will like, and guys spend a lot of time hoping a girl will like them back.

Dating fatigue is bullshit, of course, there was a big increase of users on dating apps during COVID, but I guess the market penetration of dating apps is at close to 100% in developed countries and as the population is not growing then there are not more users on these apps. (Yes there is a chunk of users that don't want to be on a dating app, but everyone knows the concept, and all people that are willing to be on that kind of app, are already on it). MATCH group actually has a stable number of users and revenue (well it varies by a few percent each year but overall it is the same).

## Feed algorithm and attention sharing

Recommendation of profiles that you may like is a solved technical challenge at Tinder level and at mostly any dating app today. The tech is based on your like/pass history (very similar to what TikTok does actually), and compute similar users from that, take a look at TinVec (There are other more recent versions after that but they follow the same logic). Recommendation of matches is therefore also a solved problem as you can compute the likelihood to like but also the likelihood to like back, and then you have the most probable match. But the algorithm is not everything.

An app that just recommends people as soon as it is comfortable knowing your taste is unlikely to work well.

-   These algorithms need your like/pass history, so you need to have some activity on the app before the algorithm being accurate
    
-   We can know which users you will like, but we can't know all the profiles you will like
    
-   People don't want to see only users that they like, girls actually adapt their behavior if they see too many guys that they like.
    
-   There is also a second cold start problem, as long as you didn't receive a few likes you can't get recommended.
    

You could base your algo on what people tell you they like, but in truth people don't know so that will not be a very solid recommendation engine, and that would actually not work better than a randomized algorithm (randomized algorithm with a few rules works very well actually)

The key to produce the best feed algorithm (if you target retention) is personalization more than recommendation. Of course seeing users that you really like every once in a while will increase your retention. But what really increases retention is understanding what each user wants. And we all want things different. Some people will want to see only a few people that they like a lot, some people will want to see a lot of users. Some people will want to see people that are far from them, some close, some can't stand seeing people they don't like, some can't stand seeing people they only like. Some want to receive a lot of likes, some want to receive a few likes. I believe it is possible to get almost 100% retention for any user on a dating app by creating a custom feed algorithm for each user (This is not technically possible of course but you get the point, the gold rule doesn't exist, because we all fundamentally search for different things)

The feed algorithm is the only thing that will impact the retention of users on your app. Everything else almost doesn't matter.

When you design a feed algorithm the major issue is attention sharing. You can't show one user more without showing other users less. For girls, this is not so much an issue; all girls will receive enough attention from guys, and so to satisfy that the algorithm is pretty simple:

Reserve about 20% of feed space for guys she will be okay to like on a good day, reserve 10% of feed space to guys that are hot according to society. With that, you have made the maximum you can do so that girls send the maximum of likes (a girl sending likes is your objective as a feed algorithm builder). You have then 70% of feed space left to do everything else. In the first 20%, you have to ensure that guys are somewhat active, that a proportion of them will like them back, and are not too geographically far from where the user is, because at the end of the day what matters for retention is that the guys and the girls talk to each other.

On the 70% left, there are a bunch of guys to push.

-   Guys that are not ranked yet (users that were not shown to a lot of people yet)
    
-   Guys that just signed up (You want them to have a peak of attention received when they just join the app, as the first 12 hours experience is proportional to the d30 retention)
    
-   Guys that are used to receiving likes and didn't receive any in the last 24-48 hours
    
-   Guys that paid
    
-   Random guys to not always push the same kind of guys to a girl
    
-   Guys that the girls think she wants to see but she will never like
    
-   Ugly guys (we didn't do it ourselves, but I've had discussions with other app feed builders that said that in some contexts pushing ugly guys will increase girl likelihood a few profiles after)
    

For guys, the algorithm is a bit different:

-   20% of feed space, you need to show in priority new girls, so that they receive likes very quickly after the end of the onboarding (they are used to receiving a lot of likes, so you have to show them they get liked; they don't need to receive a thousand likes, but 100 likes is a good point). You also need to get a ranking of them; you will have it after about 40 reactions received (so pretty quickly)
    
-   Then very importantly, if you are one of the first likes of a girl on the app (if a guy is one of the first 4 likes of a girl on the app, he needs to see the girl in his feed very quickly so that the girls get a match as soon as possible)
    
-   Then all the other feed space can be used for whatever you want. Show at least one girl that the guy will really like once in a while. But other than that, you can do whatever you want; it will not really impact the experience. It will mostly impact what the kind of girl the guy thinks he can match with on the app. So if you want guys to think you have only hot girls, show only hot girls, etc.
    

## Retention

Retention is the metric you look at when you analyze the performance of a consumer app. What share of users stay on the app 1 day after the install, 7 days, 30 days? The industry more or less has the same figures if you exclude apps that target only one set of users (Rayya for the upper class, Grindr, BLK all have superior retention), but in general, you look at 50-60% d1 retention, 30-35% d7 retention, and 15-25% d30 retention.

What impacts retention?

For girls, it is the number of likes sent; the number of likes received has no impact on retention, maybe a little bit but less than 1%. The number of likes sent has a huge impact; a user that liked no profile in her 100 first scrolls has a d30 of 12%, and 19% for girls that like 10 profiles and 16% for girls that liked 5 profiles. The d1 retention is almost 100% correlated to a girl sending 5 likes to active guys in the first 24 hours (the real thing is to get a match, but it is easy to get a match when a girl sends 5 likes). So to have the perfect d1 retention for girls, the only thing you should focus on is to get them to send 5 likes. And you have about 100 scrolls to do so.

For guys, the number of likes sent has no impact; the number of likes received doesn't either have a real impact. We have never found a rule for guys to increase retention (to decrease it, just make them have a very bad first 24 hours experience). Limiting the number of users a guy can see also doesn't have an impact on retention. Notifications received have a meaningful positive impact. No likes received also have a negative impact. But really, what to look for to increase guys' retention is very unclear to me. The only thing that really has a positive impact is a guy exchanging messages with a girl, but this is not really something you can control as some guys just don't want to talk, and as guys have very low ratios, it is not always possible to find girls to match them with. Likes received have a positive impact, but it is very light, which is a good thing from a monetization standpoint (you can make guys pay and not show them to anyone; they will keep paying, just a bit less than if they received likes. Not that I endorse this policy, but this is just the reality). Excluding girls that don't match at all criteria of guys usually has a positive impact, but not for all criteria. In general, profile recommendations don't work very well for guys, but filtering works better (but not always either; for example, if a guy says he doesn't like girls who are leftist, then taking it into account will increase retention, but if he says he doesn't like girls that are rightist, then taking it into account will reduce retention, and for girls, this is the contrary actually).

I suspect that user activity on a dating app is also super correlated to user activity on another dating app. Meaning most users are usually signed up on several apps at the same time, and when they check one app just after, they will check their other apps. Which makes retention more or less uniform across all dating apps.

Most things have negligible impact on retention, displaying the distance on the feed, showing profiles that are geographically far or close, showing more beautiful users, respecting users' criteria, no glitches on the app, ability to filter users, showing unread icons, loading speed of the feed, ...

Retention in itself can mean several things, there are 4 kinds of retention you can look at:

-   the investor retention, it is the one they see on their market analysis tool, or the metric you see on the Apple Store or Play Store, it is per app install.
    
-   the per-user global retention, it starts when the user creates their account, it is not the same as the investor retention because you get a big proportion of users that create several accounts, or finish the onboarding, do some activity, delete their account, and recreate their account.
    
-   the per-user after finishing onboarding retention, you exclude the conversation of the onboarding and the things that can happen during it. This is the retention you need to look for changes made inside the app.
    
-   the retention after a new release, this retention is not helpful because you can't compare users that are at different stages of usage of your app. Making a change to a user that is onboarded for 4 days will not have the same impact as for the user that is onboarded for 15 days. So this retention is not really usable.
    

But Tinder/Bumble dont look at these product metrics much, they look at retention of paying users, free users don't matter

Retention of men vs attractiveness (for women there is no impact).

![ratio users](https://blog.luap.info/static/dating/attractiveness_guys_vs_retention.png)

## User composition

In our case we had even acquisition in terms of male/female, but the retention of girls is lower than that of men, so you end up with 66% men and 34% women. You could say this is an issue to have more men than women, but in reality a dating app with 50/50 can't work well.

30% of our new users were users that had already registered in the app in the past.

65% of girls are looking for something serious vs 50% of men.

![where users look for](https://blog.luap.info/static/dating/look_for.png)

What users are looking for really depends on the age of the users. The younger, the less serious (which makes sense).

| users | sex | date |
| --- | --- | --- |
| men 30-35 | 15% | 18% |
| women 30-35 | 1% | 14% |
| men <25 | 32% | 17% |
| women <25 | 6% | 25% |

Whats interesting is that the more attractive the guys were ranked by girls the more they were looking for something not serious

![](https://blog.luap.info/static/dating/attractivity_vs_sex_men.jpg)

And no correlation for women:

![](https://blog.luap.info/static/dating/attractivity_vs_sex_women.jpg)

Among girls : 10% where lesbian, 84% were hetero and 12 % were interested in both genders

Among guys: 7% homo, 92% hetero and almost none were interested in both genders

## Monetization

During my time, we never implemented monetization inside the app, but we had people requesting it. I think monetization in a dating app has nothing to do with what existed 10 years ago. Today, this is the norm, and guys expect to pay to have access to likes, which I think is a good thing. There is no reason why we shouldn't pay for a service that helps us date other people, especially knowing how difficult it is to acquire and retain users on such an app.

Things have changed in such a way that I don't think monetization has a negative impact on retention too much. First, it has no impact on girls' retention as they will not pay, so you have to make the app non-paying for them, and retention is only more or less proportional to them sending a few likes per week. You have plenty of feed space to show them guys that they don't like as long as you show them guys they like every now and then.

As for guys, they really are used to not receiving a lot of likes; whether you pay or not, that doesn't change much.

Tinder has similar retention numbers to Hinge whereas on Hinge you don't have to pay to use the app and on Tinder you need to. Hinge has 1.5 million paying users when Tinder has 10 million (with a much bigger user base). Hinge is growing 40% year over year. I think dating apps are one of the best businesses in the world as long as you are good at marketing. What works on the product side is known by everyone, and you just need to replicate it, only Bumble is not able to do it, no surprise if you spend a few minutes on their app (Don't know who runs their product in the last 4 years but they should be fired). All dating apps that exist and work are just Tinder as a product PLUS a small marketing difference. Hinge is Tinder + "Oh we are nice we are really going to help you find love", Bumble is Tinder + "Oh we are empowering girls", BLK is Tinder + "Oh we are only for black", Grindr is Tinder + "Oh we are only for gay", Happn is Tinder + "Oh you are going to match with people close to you". The "+" is only a marketing message and doesn't influence the product experience. At the end it is just a like/match/talk with different CTA colors.

iOS users have more money and spend more money. So you would have to prioritize them. It is obvious Tinder/Hinge does it. Just sign up as an iOS user on those apps and then log in on an Android phone, the difference is big.

## Fake users/ bots/ verification

Moderation is a complex topic. First, you don't want some people that will negatively impact the experience of your users. So you don't want girls advertising their OnlyFans. If you don't detect them they will like and match every guy, and the only likes of most guys will be these girls and guys will think they are only bots on your app and won't want to pay. You also don't want guys scamming girls for money, because girls won't trust your app, and without girls, guys get less likes and pay less. So in general, as an owner of a dating app, you don't want obvious fake users/bots.

But what about subtle fake users? You have plenty of fake users that use the picture of someone else (usually a pretty person) and will use the app as they would. The major difference is their like ratio will be higher, so they will basically help you in your mission to give more likes to guys and more matches to girls. I don't know what Tinder does about them, I think they keep them active. We blocked all of them and didn't really see a negative impact on retention. But they are difficult to catch, they talk to users like normal people, they behave very similarly. They are usually ugly guys OR girls (yes, also girls) either trying to see what life is like as hot people or just people feeling alone that want to talk to other people (it was very visible because we had a feature where we requested verification for some profiles, and it requires sending a video of your face, and fake users would very often do it with their face, which was different from the ones on the pictures of their profile).

What techniques are being used to detect these users: Well, if you believe you can trick us, good luck. All the things we have at our disposal: IP (belonging to VPN), device ID, picture uploaded (with similarity search), user in-app behavior detection, age range preference deviation, GPS coordinates not moving. Also, Tinder/Hinge has phone number as a mandatory step (I don't think it is useless honestly but it is being used for the other feature to hide people you know, and that may help to create a graph of connections but I don't know if this is legal). There is also face detection that is now pretty advanced and can detect the face of celebrities, people on social media. If you manage to sign up with a fake account on a dating app, then this is because they know you are fake but they don't care to have fake users. The best I have seen is people using VPN to be located in the right country + fake position + emulator (to have different device ID) but they were more or less using the same picture so that was easy to spot. The most advanced bots were to promote OnlyFans or Telegram.

Verification is not mandatory on most dating apps, I don't think this is an important topic. We initially had a lot of criticisms for having too many fake profiles on the app, and it was completely wrong, we never had fake profiles, we just had too many pretty girls and guys would think they are fake. Today we don't have any people complaining of fake profiles etc., so I guess people trust the apps nowadays. The verify your profile thing has no interest for me unless you are Match Group and you share phone numbers between different apps (device ID on iOS are stable per app ID). Bots are much dumber than we think, there is no race to fight all mechanisms.

In our case, we almost had 0 fake users, of course, you can find one sometimes, but in general, we were able to block all of them, but for that, you need to have manual review every day, manual review is done after auto approval/verification. To me, it is worth the investment because you increase the quality of profiles and reduce frustration.

## Onboarding

There were not a lot of learnings in the onboarding. The only drop was on filling out your picture of your profile. For any other screen on the onboarding, when we give more info to the user, or ask for more information there was never a drop, we could add as many screens as we wanted and never impacted the onboarding conversion. But users were not willing to spend time on a screen, for example, if you would put a way to send information with voice, nobody would use it, if you put a free text input nobody would use it, if the free text input is mandatory most users would write the bare minimum but will not drop.

Conversion was 90% and whatever we added as an additional step this didn't change. The median time was 13 minutes a little bit for men than women.

People were willing to answer any kind of question that we would ask them. That would make these apps the apps that know the user the best. Fortunately (or unfortunately) this information is of no use for dating apps as the only important things are your picture and all other things almost don't matter. But this may feel user good about the app if it is able to show you that it knows you well, but it is more for the user that gives the information than for others to be able to find more interesting matches.

## User interview

Most people are not educated enough to go through a user interview, you wouldn't be able to get their mind about the app, about what to change, about what interests them. They would just say, yes I like the app, yes I don't like the app, I do that, I don't do that. You would need to be a pro at user interviews to really get interesting feedback. (Well maybe this is the norm in B2C but at the end of the day user interviews were of no help).

The only people that would really want to go through user interviews would be people that thought they knew how to create a dating app that works, and would tell you, "why don't you do that?", "I'm sure that will work", "Here is an idea for you, it is a game changer". Never helps.

## First session

For girls, the first session (the following 2 hours after the onboarding) is almost the only important thing in the life of the user. If you fail at it the user is lost. If a girl doesn't get a match in under 24 hours, which in our case was equivalent to them sending 5 likes, then the user is lost.

For guys, it is much less the case. It is not a problem if a guy sees no one he likes, or if he doesn't receive likes or if he doesn't see a lot of profiles. Almost nothing matters, I think that the only thing that matters is that the guy feels like the app has some potential (whatever that means for him). When we had bugs this had a much bigger negative impact on guys' retention than girls like x2 or x3 compared to girls. We had a time where we would show only a handful of profiles on the first session of guys, and show them an "end of feed" screen, and this vs show an endless feed didn't have any impact on anything.

On Tinder or Bumble, for example, you can't get a match in the first 24 hours if you don't pay. They both boost new profiles so that they receive likes just after their onboarding, but just can't see the likes. Which I think is a good thing as anyway not receiving likes on the first day will not have an impact on your retention, so you get used guys to paying to have access to likes without having retention decrease.

## Tech challenges

A dating app requires a lot of requests per user per day compared to any other B2C apps. Any action of the user has one HTTP request (like, pass, see profile, send message, list conversations, etc.) so you quickly get a lot of traffic even if you don't have a lot of users. In itself, it is not particularly hard to handle as the requests are always super simple and most of the time just a wrapper around a database call. The only exception being the feed.

The feed is probably the only tech challenge. First, it is very difficult to index because when you compute your feed, you compute it differently for each user. So you basically have to do full table scans every time you want to render a feed for users (which is quite often, we did a feed request every 10 profiles in our case). Second, you probably are going to use embeddings to represent your users, but embedding in addition with filtering doesn't work well. [Tinder has a famous post where they explained they clustered users depending on their geography](https://medium.com/tinder/geosharded-recommendations-part-1-sharding-approach-d5d54e0ec77a). This is not impossible to do, but this is not obvious tech.

The second tech challenge is user recommendation. Same thing that YouTube/Facebook/TikTok has, so you have some ML algorithm that needs to be created and works in almost real-time. The app can largely work without that at the beginning and add it later as it is not a game changer compared to a rule-based approach. But for example, if you want to push users, then you would need to have that in place.

## Product Analytics

I don't think this is possible to do product analytics in a dating app by using off-the-shelf product analytics tools alone like Mixpanel, Amplitude. They are too limited to measure what you want to do, share of users that send 10 likes in 1h after onboarding => not possible, share of users that remove their account and recreate it => not possible, how users that sent less than 3 likes under 1h after their onboarding behave => not possible, exclude users that deleted under 1h => not possible. You will need at least to combine Amplitude/Mixpanel with backend events containing computed data. Also, these software are super expensive and price per monthly active users so you should be extra cautious before sending one event to a user asynchronously, you will quickly need to pay for 3 or 4 times the number of monthly active users that you really have.

Except that, it is crucial to do product analytics, because as we have seen before this is pretty hard to do user interviews, and also again what people say is usually very far from what they do.

## Bootstrapping the user base

We didn't struggle too much at the beginning, we just removed the geolocation of users in the feed, so initially, you could meet anyone in the world, then when there were enough users in one country, then you could meet any user in your own country, and then we had enough users throughout the country then we implemented more restrictive geolocation filtering, but we never let users choose it. I think it is harder to have a good retention when you let users choose. Even Tinder, Hinge, Bumble don't really respect all the time the distance you gave them.

People will complain that there are no users around them, but at the end of the day, there was not a big jump in retention when we implemented closer geolocation, the complaints decreased though. Complaints don't always translate to improvement in app figures, even if a lot of users complain about it. But don't read me wrong, it does have an impact on retention, but you still need a few weeks of use to bootstrap your feed in a specific location.

It is the same thing with the match, the match works great when you have a big enough user base, before that you can release the match and let users talk to each other, users will have some interaction it is better than none. Then the game is a marketing game, you can buy your first users (do marketing campaigns, ads, pay influencers, etc.) it is not too expensive either.

## Perceived attractiveness

Perceived attractiveness is one of the few big problems of dating apps. To summarize, perceived attractiveness of guys is lower than their real attractiveness in real life. And perceived attractiveness of girls is higher than in real life. And it all comes down to your ability to have good pictures. And actually on a dating app as a user level it only comes down to the quality of your pictures.

To me, if you are a guy on a dating app and your pictures are not taken by a professional photographer then you are losing your time, and if you are paying you are also throwing your money.

The issue with this difference of perceived attractiveness from dating apps/real life is that girls will internally build a feeling that they can have any guy (even if they rationally know this is wrong). And will not accept to like at their level, which is a problem as they will be dissatisfied with their matches. For guys at the contrary they will receive less attention on a dating app than they receive in real life and will conclude that dating apps don't work. While the truth is just that their pictures suck.

One thing that has been suggested to standardize looks is to use video, and especially video on the go. But it doesn't work, we tried to make it work a lot, but at the end of the day, girls they don't feel pretty all the time. And also a lot of them don't accept how they look and will only accept a video where they have filters. So by forcing everyone to use videos, then you basically prevent girls that look pretty in pictures from signing up. Guys were much more willing to do a video, about double than women. The only videos that work are videos that you have on your phone, but they are usually taken in the exact right conditions to look good so they don't at all help to standardize looks.

## Conversation starter

We usually think that starting a conversation on a dating app is difficult. But this is not the case for everyone, there are plenty of people that just start with hello how are you, and the conversation like that and they don't care. Very often you can have a conversation like that: Hello how are you? - I'm good and you? - I'm good too what are you doing today? I'm chilling on my sofa - Oh great - Yes I like watching Netflix.

Every attempt to improve conversation starting will not be beneficial, some people will complain because they just want to talk to the user.

Finding common topics, or helping users talk to each other will actually really improve the fact that users will talk to each other, but this won't fix all the issues we see. At the end the predictor of two people meeting with each other is not a match or that the conversation started well. It is just the fact that the two really wanted to meet each other. Better conversation starters probably increase retention a little but don't increase better conversations and I don't think it translates into people meeting with each other.

## Profile pictures

This is the only thing that matters for a guy on a dating app, we had guys going to 0.03 ratio to 0.2 ratio just by changing their picture.

Displayed location had a significant impact, your job position too, height, the rest has no impact, people don't check other things than that before liking or passing a profile.

One thing that is difficult to believe for guys, but really is true, is that it is not because the picture of girls looks sexy that she is looking for sex. There is actually ZERO correlation with girls looking for something serious and how sexy their pictures are. I know that for guys this is impossible to believe, but this is true.

For girls even more than for guys, picture is really all that matters, the more sexy you are in your picture the better your ratio will be. But I wouldn't say this is critical for a girl to have sexy pictures. Because at the end whatever her picture there will be enough guys that like her (so not critical but important).

For guys this is the only thing that matters, really nothing else does. Most guys' pictures are horrible, and one big pain that we had is how to make the guys look good, most guys look better in real life than his dating profile (and this is the opposite for girls).

I don't think there is a big correlation between liking the pictures of guys and liking him in person. I think there is a strong correlation for men liking a girl in a picture and liking her in person because guys are more interested in physical appearance though. If you are not convinced of that you can look at this [Youtube video](https://www.youtube.com/watch?v=MQEszef9ev0) and especially this image

![jubilee](https://blog.luap.info/static/dating/jubilee.jpg)

The "Group ranking" is the guys ordered by number of likes on their profile pictures. The Women #1 ranking is the guys ordered by number of times being the most preferred after spending a little bit of time with them. So this is not the exact same ranking methodology (89 likes for 50 girls in the first method) but it still speaks. The final result is not correlated with the pictures feeling results.

## Does user know what they look for?

No, at rare exceptions. We tried everything in that area, asking them in free text format, in voice format, in a conversational format, in multiple choice format, in multiple choice + free text, by checking patterns in their like history. The truth is people don't know what they like and look for in a partner.

Which makes sense, how many of us can really say "Oh I don't like when a guy/girl is X" and then falling in love with someone that actually IS X? That's why relying on what users think they look for is not a good strategy in terms of product. Most people will contradict themselves when describing what they look for. Also, we also analyzed per user, what they said they were looking for and what they did like after that, and it was not corresponding. Girls would say, red flag if a guy has shirtless pictures and then liking profiles where guys were shirtless. Same thing for having tattoos, wearing eyeglasses, etc.

Girls specifically also for most of them just say the same things that are completely irrelevant. If you really asked girls "what kind of guy are you looking for?" and asked for a free text answer they would answer "I want a guy that makes me laugh, that is kind to others, that takes care of him and is faithful." In 70% of cases there were their answer, which yes is true, but if you think about it, saying that is not going to help anyone find you a relevant person.

What we tried to do at the end is only getting a few "no go/requirement" preferences. Which I think is the way to go, and have only like a maximum of 2 or 3 choices allowed. But we never managed to make a framework that is really able to get that information from users.

## AB testing

AB testing is important because it is almost impossible to know beforehand what people will like or dislike, sometimes small things have a big impact, and sometimes big things have a small impact. And usually, the most given feedback has no impact on retention. Most of the time we did what a lot of users asked, this didn't have any impact on retention.

It is difficult to AB test the algorithms of the feed because the change usually has an impact on the user that gets a specific variant but also users that he likes that are on any variant. Other than that, you can gain 10 points of retention just with AB testing and finding the best things that work.

It is actually pretty easy to create a dating app that works in terms of retention if you use extensive AB testing.

## Notification

If you send a notification to a guy that a girl has liked him, if he logs in and sees nobody, the user will have a higher retention than if you didn't send him a notification.

Notification of likes received by men or women had a huge impact on retention, about 4 points.

Notification open rate was close to 80%, except for non-contextual notifications which were closer to 30%.

Sending notifications to make users come back to the app works but is not a game changer; you can make jokes, push people to open the app, offer them something, as in the end, it slightly improves people opening the app. Transactional notifications (Someone liked you, you matched, you received a message) on the other hand have a major impact on retention.

Users that didn't activate notifications didn't have bad retention; it was just a bit lower. But they still logged into the app. This was still a big portion of users (about 30% didn't allow notification permission). You can request permission at the beginning or the end of the onboarding; it doesn't change much how many users will accept them (I find it really weird to go on a dating app and not authorize notifications, but well ...)

## Fun data

Non-binary doesn't exist; we had like 0.1% of users registering that said they were not a man and not a woman.

There are more girls that don't want children than men: 20% of girls didn't want children and 16% of guys.

10% of users that delete their account recreate it in the following 3 days, and it was 17% in the following month.

75% of girls want someone taller than them.

65% of young people wouldn't date someone that is too politically extreme.

Showing who likes you didn't have any impact on retention, but we weren't "locking" likes, so that's maybe the reason why.

Users with iOS are more attractive, have better retention, and are younger than users on Android. I strongly believe Tinder and co. boost users that have iOS vs Android.

The ratio peaks in the first 24 hours and then degrades, for men and women, because at the beginning you are highly exposed to people that have been on the app for a long time, which are usually people that like more.

1% of guys received 10% of the likes, 3% of the guys received 20% of the likes, 10% of the guys received 40% of the likes, and 20% of the guys received 55% of the likes.

More than 50% of men just never receive a like, and never means maybe 2 or 3 likes in the lifespan of several weeks

The best day of the week in terms of acquisition and user activity is Sunday by a large margin; Friday and Saturday are the worst.

Only 50% of girls sent 10 likes in their account lifespan.

25k new users per day in the US is a good performance for a dating app.

10% of girls that finish the onboarding never send any pass or like, after an onboarding of an average of 13 minutes.

## Shady idea

I think that if you are a marketing person and have a bit of a shady personality, you can create a dating app that has significant revenue easily. What you have to do is to create fake girls' profiles; you don't really have to make them talk or match, just be visible on the feed. Then you make guys pay for similar things that Tinder is doing (you can just copy their plans). The best would be to communicate around being an app for sex. When a guy pays, you would just not do anything different than non-paying users. You will probably end up with 90% real guys and 10% real girls, but I would be surprised if you don't make a lot of money with that. Probably a lot of not really popular dating apps you find on the Play Store/Apple Store are just that, and that have no girls.

## Retention as a metric for dating apps

I don't think retention is a good metric for a dating app, unfortunately that's how VC evaluate performance of B2C apps. When we think about dating it is more about quality than quantity.

-   The issue is that for some people they have pretty specific things that they are attracted to, for example if you like someone with piercings, and you have that passion. There are very few people that have a piercing, so you will scroll through the available people during the period you are using the app, and at one point you will stop using the app. In truth you want to be notified when someone with a piercing has registered into the app and you want to 100% be able to talk to him, not to hope he will like you back.
    
-   For other people it is only about focusing on one person, instead of accumulating a lot of matches. When you have a feed you are just unconsciously encouraged to keep scrolling and to not be satisfied with what you already have. The app should just handpick a few profiles for you, and make you focus on these ones.
    
-   For some people that are not super picky about their partner, and for these people usually dating apps work very well.
    

The problem with not using a feed is that it will inevitably get you a bad retention compared to the retention you have on Tinder for example. What makes you keep scrolling on TikTok or YouTube or Instagram? It is the feed, it is the same on a feed-based dating app. Pursuing retention will just bring you to recreate Tinder.

As a consequence if you really want to create a novel dating app, and not a dating app that looks like Tinder for example, then you mustn't follow retention too closely, of course it is important to follow retention because it is also a great marker that your users like your app. But optimizing for retention all the time is going to end you doing the exact same thing as Tinder is doing, and Tinder is a cash machine, a wonderful one, but not a dating app. So you will end up with a worse retention than Tinder so VC won't want to give you money, so you need to be self-funded. So you need to monetize your app almost from the start. It is not super expensive to acquire users on a dating app, because a lot of people are looking for love and are testing a lot of apps all the time. What is expensive is to acquire a high number of users all the time.

## MATCH group

I used to think Match Group was an evil monopoly. What now I think is that they have super knowledge on how to operate a dating app, how to market a dating app, how to be profitable doing user acquisition, how to optimize every single part of a funnel to extract as much money from users without losing them.

I don't think they are a monopoly, Bumble is a super big company that once had a lot of money to do whatever they wanted and compete with Match and they managed for some time, but stopped and now will become whatever happens.

I don't think they prevent the field from innovating, on the contrary they are super agile for their size to copy what other dating apps are doing when they feel it is interesting.

I don't think they bring anything interesting to the world either, they could crash and that wouldn't change a thing, other apps will gain more visibility and do the same things as them.

They are not hugely profitable, their profit margin is 20% (Google is 23%, Visa is 50%, Nvidia is 55%).

Operating a dating app is just about filling out a leaky bucket by nature, and so you need to be good in user acquisition, in monetization and then do anything you can to reduce the churn.

## Can dating apps work?

There are a lot of people saying that dating apps are evil, that Match Group exploits people's feelings. That's true on one hand and wrong on the other.

First the expensive resource on a dating app is girls, not girls because they will pay you, but because if you have girls it will attract guys, what impacts the retention of girls on a dating app? The number of likes they send, so the first goal of a dating app is to make girls like, clearly not something evil. There is no interest in making them pay as none will do it. (Hot girls can be replaced by bots, or by girls that promote their Instagram, they should just not like everyone but they don't need to be real for the system to work, in truth it is not super complex to acquire these girls, so no need to have bots.)

Second the most expensive resource on a dating app is hot guys, because without hot guys you won't get girls, well hot guys are the users that have the best retention, without effort they will have a 50% retention, so in fact you just have to wait some time and the app will fill up with those guys, need a specific strategy? No, do nothing. Is this evil? No.

Then the last most expensive resource on a dating app is guys that are willing to pay, what can you offer to guys that want to pay? More likes received. Well good news more likes received = more retention for these guys. So the things these apps do, is how to make a guy pay the most to access likes? It is very similar to any business, a football club has the same objective, a restaurant has the same objective, a gaming app has the same objective, a gym has the same objective. The ones that are left behind are actually the ones that are not paying. Tinder for example, they are just the best at converting you into a paying user and keeping you, and they will not keep you if you don't get likes.

The problem of dating apps is not the product in itself it is the users of these apps. People that complain about datings app are either:

MALE

-   Not willing to date (so they complain they cant accumulate matches as fast as they would like),
    
-   Make no effort to be attractive (everyday we would receive emails to the support of guys saying "im not receive likes and im not the less attractive", well yes you are ),
    

FEMALE

-   Not willing to like anyone (We have plenty of girls that can scroll through 300 profiles and not like anyone and deleting their account saying "I dont like anyone" well
    
-   Liking users too attractive that would only want to have sex with them
    

The hard truth is that if you are a guy and you dont go on a date with at least one new girl per week and dont have your picture taken by a professional photograph you are loosing your time on a Tinder-like dating app

And if you are a girl and you dont date at least one guy per week and you dont lower your appearances standards then you are also loosing your time on a Tinder like dating app

So for me dating apps currently work, and actually they are the biggest place people meet these days. But using them is very disappointing

## Can Dating apps be fixed ?

What I meant by "fixed", is an app where:

1.  It is possible for someone to reach out to anyone and get an answer, and discussion can be interesting from the start (at least as much as in real life)
    
2.  Your looks, how you behave, the tone of your voice,... reflect what you look like in real life
    
3.  It is easier to meet new people than attending local events
    

I don't think it can. I think our digital representation either via photo, video, text, voice doesn't represent us very well and as a result we will either over-hyperoptimize before meeting and be disappointed when meeting. To minimize that risk girls will always increase their standards (which are currently too high) and guys will lower them, which is a recipe for mismatch.

But I think dating apps can currently be used at each women and men advantage, it is just necessary to have the right strategy:

-   For girls you need to lower your standards and force you to go on a date with guys that you dont have the flame for (it is actually very hard to do that for a girl, very very hard)
    
-   For guys, you need to pay a photograph (to get liked) and pay the premium plan (so that your profile is shown to other users). If you think a dating app has no incentive to show paying users to girls, then you didnt read this article ^^
    

I think Tinder has reached peak business revenue, and it will not decrease much nor increase much. They are not really 100% in the dating business, their business is more to entertain you in the context of dating. There are other apps that help you meet with strangers like Timeleft, and they seem to do fine, but are not really apps, they just pair you with people without letting you choose who you will meet (which I believe is a very interesting take, and could work very well in the long term). But they are not a dating app. I wouldn't look into AI to find the next dating app, because AI has already been present there for a long time, and it doesn't increase meeting people and can't fix the fact that pictures are biased, people don't know what they look for, etc ...

The biggest advantage of a dating app is that it gathers only people that are actually looking to create intimate relationships with other people. And as a result, they are seen as a more effective way of meeting new people. Their biggest issue is that coming from that users expect the rest of the process to be seamless and to find a partner in hours without doing much work.

To me, the next revolution is really concepts that will make you meet other people without having much information about them. As a user, you will trust the algorithm to match you with the right people. And these concepts will be paying only.

-   Users would be able to fill out a maximum of 2 or 3 criteria in addition to the custom-made algorithm (the custom-made algorithm will try to create couples that have the same interests, the interesting variables for that are age, localization, job position (to infer social status), body type, ethnicity, religion, attractiveness level, kind of relationship searched), and the 2 or 3 criteria would be some things like (sporty, don't smoke, tall, small, white, black, no bald, high status, ambitious, ...) these ones are not easy to find and would not be entirely respected by the matching algorithm, but more a help
    
-   Users would have limited information on the other before meeting with each other, it could be either 1-1 meetings or meeting in a group setting, (group settings is a very interesting idea, because that directly lowers the expectation of the date and you are not afraid to be locked with someone that is awkward during xx minutes)
    
-   Users would need to pay to access the service from the start. Free dating apps are not efficient because they need to get something in return, which is your attention to monetize some features and need to keep you in the app, which is the opposite of what we want. (And yes, make girls pay, it will work, Timeleft has proved it)
    

So to summarize, a concept where users pay and commit that they will meet people without knowing them before. So yes, it will take a few dates to really find someone that you like, but so is going every day in dating apps and meeting people that you don't like either.

## Conclusion

I'm not a lover of Match Group, I don't own their stocks, I'm not, and never have been paid by them. I don't like Tinder, I find Bumble's product to be super annoying. I don't recommend to people to install dating apps (unless you are a hot guy and looking for sex).

But really I challenge anyone that tells me that they are shit what would they change to make the product better for users (and don't tell me to remove paying features, they are a business they need to make money and it is going either through ads or through user membership).

The core problem is users (YOU) don't want to go on a date and want to find a gf/bf staying in their bed with unrealistic criteria
