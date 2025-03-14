---
created: 2024-10-27T21:35:22 (UTC -03:00)
tags: []
source: https://newsletter.systemdesigncodex.com/p/eventual-consistency-is-tricky?ref=dailydev
author: Saurabh Dashora
---

# Eventual Consistency is Tricky - by Saurabh Dashora

> ## Excerpt
> But there are great patterns to handle it...

---
What is eventual consistency?

> **The concept of eventual consistency refers to a system condition where all parts of the system reach the same state, even though they may be temporarily inconsistent due to delays or failures.**

Here’s the bad news:

_You can’t escape eventual consistency in a distributed system._

But there’s also good news:

_Eventual consistency helps you scale distributed systems, provided you follow good patterns to build your applications._

Let’s look at some of the most useful patterns you can use:

## 1 - Event-Based Eventual Consistency

In this pattern, services emit events when something changes in their state, and other services listen to these events to update their data.

The services don't directly communicate with each other, making them loosely coupled, which is great for scalability. However, this approach introduces a delay before all the services reflect the latest state, resulting in eventual consistency.

Here’s an example:

Consider an e-commerce platform where a user’s account profile is managed by one service, and a separate service handles the recommendation engine.

-   When a user updates their preferences in the profile, an event like “profile-updated” is emitted.
    
-   The recommendation service listens to this event and updates its database to reflect the new preferences.
    
-   There is a short delay between the profile update and the updated recommendations, leading to eventual consistency.
    

## 2 - Background Sync Eventual Consistency

In this pattern, a background process or job periodically synchronizes data across different databases.

The background job runs on a scheduled basis, checking for inconsistencies and making the data consistent. This method often results in slower eventual consistency because the synchronization only happens at set intervals. However, the user update is performant.

Here’s an example:

Imagine a social media app where user posts and user analytics data are stored in different databases.

-   A background sync job runs every few minutes to update the analytics database based on new posts.
    
-   This allows the analytics service to eventually reflect all user posts, but users might see slight delays in the analytics update (e.g., a post count increasing), as synchronization only happens after the background job runs.
    

## 3 - Saga-based Eventual Consistency

The saga pattern breaks down a distributed transaction into a series of local transactions, each handled by a single service.

After each service completes its local transaction, the next one starts. If something goes wrong, compensating transactions are triggered to roll back the changes, making the system eventually consistent.

Sagas are used for long-running, complex business processes that need to ensure eventual consistency across multiple services.

Here’s an example:

Think of a travel booking system where a user books a vacation package that includes a flight, hotel, and car rental. Let’s assume a weird requirement that if car rental booking fails, the entire vacation plan needs to be canceled.

-   Each of these is managed by a different service.
    
-   The booking process is broken down into multiple steps (or transactions) handled by the respective service: the flight is booked, then the hotel, and finally the car.
    
-   If the car rental fails, the saga initiates compensating actions to cancel the flight and hotel bookings, maintaining eventual consistency across the system.
    

## 4 - CQRS-Based Eventual Consistency

CQRS (Command Query Responsibility Segregation) separates the system into two models: one for handling write operations (commands) and another for handling read operations (queries).

Each model is optimized for its specific task, and the data between the two models is synchronized asynchronously, resulting in eventual consistency.

The write model processes changes immediately, while the read model is eventually updated to reflect those changes.

Here’s a possible example:

Consider a banking system where the account balances are stored in a highly optimized database for read operations, allowing for quick access to balance inquiries.

-   The write operations, such as deposits or withdrawals, are handled by a different database optimized for transactional integrity.
    
-   When a customer withdraws money, the write model is updated instantly, but it takes a moment for the read model (the balance inquiry system) to reflect this change, achieving eventual consistency between the two.
    

## Takeaway

Each pattern has its advantages and disadvantages.

-   **Event-Based**: Good for building loosely coupled systems that prioritize scalability and flexibility, but consistency takes time due to asynchronous updates. This is more of a foundational pattern.
    
-   **Background Sync**: Best for systems where data doesn’t need to be immediately consistent and can be synchronized periodically, leading to slower updates.
    
-   **Saga-Based**: Great for managing complex, long-lived transactions with multiple services, ensuring consistency via compensating transactions.
    
-   **CQRS-Based**: Allows optimization for both read and write operations, but consistency between the two models is achieved asynchronously.
    

**So - have you seen any other eventual consistency pattern?**

[Leave a comment](https://newsletter.systemdesigncodex.com/p/eventual-consistency-is-tricky/comments)

Here are some interesting articles I’ve read recently:

-   [Leetcode is Unavoidable so Make It Count](https://open.substack.com/pub/akoskm/p/leetcode-is-unavoidable-so-make-it?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    
-   [Queues, Topics, and Their Trade-offs](https://newsletter.systemdesignclassroom.com/p/queues-topics-and-their-trade-offs?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    
-   [8 effective debugging strategies to find and fix bugs like a pro](https://thetshaped.dev/p/8-effective-debugging-strategies?r=1m1f9z&utm_campaign=post&utm_medium=web) by
    

**That’s it for today! ☀️**

Enjoyed this issue of the newsletter?

Share with your friends and colleagues.

[Share](https://newsletter.systemdesigncodex.com/p/eventual-consistency-is-tricky?utm_source=substack&utm_medium=email&utm_content=share&action=share&token=eyJ1c2VyX2lkIjo4NzQ3Mjg5LCJwb3N0X2lkIjoxNTA0OTk1NDksImlhdCI6MTczMDA3NTcwOCwiZXhwIjoxNzMyNjY3NzA4LCJpc3MiOiJwdWItMjE0ODExMSIsInN1YiI6InBvc3QtcmVhY3Rpb24ifQ.Ffp0ADEd-ImuUwyF1-NLsMCvc0pwsniM77-rbGqmsaE)

_**See you later with another edition — Saurabh**_
