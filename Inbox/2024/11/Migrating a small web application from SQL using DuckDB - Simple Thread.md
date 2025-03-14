---
created: 2024-09-27T07:39:19 (UTC -03:00)
tags: []
source: https://www.simplethread.com/migrating-a-small-web-application-from-sql-using-duckdb/
author: Greg Kontos
---

# Migrating a small web application from SQL using DuckDB - Simple Thread

> ## Excerpt
> My name is Greg Kontos. I owned the most expensive cookbook on the planet. Recently I cut those data storage costs by over 99%. The Problem About two years ago, I built a small hobby site to track recipes. Since then, month after month, I’ve received a billing statement from Google. Google bills with the […]

---
![Migrating a small web application from SQL using DuckDB](https://www.simplethread.com/wp-content/uploads/2024/09/Greg-Fare-Bear.jpg)

My name is Greg Kontos. I owned the most expensive cookbook on the planet. Recently I cut those data storage costs by over 99%.

## The Problem

About two years ago, I built a small hobby site to track recipes. Since then, month after month, I’ve received a billing statement from Google. Google bills with the ruthless efficiency you’d expect from a multi-billion dollar company. And that efficiency started to sting. The whole site was costing me about $68/mo to operate. Most of that money, a full $67 each month, went to a Cloud SQL database.

Starting with a SQL instance was convenient for the initial development, but I grew tired of owning the most expensive cookbook on the planet. So I decided to make a change.

## The Solution

The solution was clearly to drop the database. In order to do that, I wanted to write the recipe information to a bucket. My serverless application could retrieve the information at startup. I could leverage dataframes to make it searchable. It was conceptually simple, it would be fun to see how well it worked, and what could be cheaper than bucket storage?

There are many other options available for dropping a permanent database instance. For instance, I could have leveraged some of the purpose built, off the shelf, serverless database options available, like Firestore. It can scale to handle heavy traffic. And it’s practically free with low traffic. But I was curious about the dataframe approach.

After some research into the various options, I moved from GCP Cloud SQL to DuckDB. This reduced my monthly data storage and retrieval bill from $67/mo to around $0.25/mo, over 99% savings.

## Persistent Database Alternatives

For those looking to drop their DB instance and go serverless, there are a handful of different alternative solution categories. The each fit specific use cases and provide different benefits. I would divide the solutions up into the following categories:

-   Serverless Databases
-   Dataframes
-   Local databases

A serverless database is like any normal database, but the compute is allocated on demand. This means that there are no servers provisioned and billed for constant use. This makes the serverless approach much less expensive. This is especially true when you have a low traffic site.

Typical serverless databases are NoSQL stores like DynamoDB or Firestore. Billing is generally based on the data transfer and a measure of the compute used. From the perspective of the application they are similar to a persistent database instance. Your application will connect to the server, send a query, and receive a response. However, these serverless databases abstract the infrastructure away from the user and leverage various on demand resource allocation for processing.

The second category of solution for replacing a persistent database is to leverage dataframes like Pandas or Polars. The use case generally falls more into data analytics or ETL approaches than as a database replacement. But with this approach, we can persist data in an object store bucket Google Cloud Store or S3, copy that data onto a serverless compute instance, run whatever queries we need, and serve data to users. Retrieving all of a users data for any query is not the most efficient approach, but it is cost effective. And thanks to the proliferation of data science, there are also lots of tools available within this space.

The third category is to use a local database. Some solutions in this space are SQLite or DuckDB. These approaches allow the application to interact with the data like a SQL instance with the data stored locally. But leveraging a serverless application server, the local storage is only temporary and a persistent storage bucket needs to be used. Due to the need to retrieve the data from a bucket this option is very similar to the dataframe approach, but with a SQL interface.

## My Requirements

The first step in narrowing down the solution was to outline the requirements for the application.

1.  Available in Google Cloud Platform
    -   My site runs on App Engine in GCP. I did not want to replatform.
2.  Available for Golang
    -   My site leverages a Golang backend. Any solution should have a Go client.
3.  Ability to search
    -   What good is an online recipe book that isn’t searchable. My site searches by title, ingredients, and directions.
4.  Small storage requirements
    -   The site stores ‘text only’ recipes. Storing recipes as text amounts to about 500K to 3M recipe books per TB. The current storage needs are under 1GB.
5.  JSON column storage
    -   My recipes leverage a json column which stores each step as an array of ingredients and directions. I wanted to maintain this basic structure without normalizing the information across multiple tables.

## Options

Given those requirements I went through some solutions on the market.

### Serverless Databases

-   Firestore
    -   Firestore is a document db for GCP. It is JSON friendly, GCP and Go compatible. This use case would fall into the free tier.
-   DynamoDb
    -   Dynamo is a key/value DB from AWS. This option is not available in GCP; but someone asked me why I didn’t take this direction so I figured I’d write down the reason.
-   CockroachDB
    -   Cockroach is a ‘SQL’ type serverless database option that can be installed from the GCP marketplace. Similar to Firestore it would have been free for this use case.
-   Upstash
    -   This option uses Redis and requires pushing data to a third party account. When I got to third party account options I figured I had enough options to reasonably make a decision.

### Dataframes ( and similar )

-   Pandas
    -   To enable a more elegant search, a dataframe approach made sense. Pandas is a very popular dataframe processing framework. Unfortunately, there isn’t a well maintained Pandas library for Golang.
-   Polars
    -   It’s a faster and more memory efficient data manipulation library than Pandas, but unfortunately there is zero Golang support
-   Apache Arrow
    -   Arrow is a solid contender. Arrow is the underlying interface for PyArrow ( which Pandas 2+ uses ). Arrow provides a columnar format definition for analytic operations, can read/write feather files or parquet, and has Go libraries available.
-   DuckDB
    -   DuckDB isn’t exactly a dataframe processor. It leverages Apache Arrow formats and standards and offers a SQL interface. Essentially it creates a local database by leveraging the same data format and processing standards as Pandas. The primary use case, that I can gather, is providing a SQL wrapper around a dataframe for analytics work.

## Choosing DuckDB

-   Firestore seems like slam dunk option. At a price of zero, it would have been an excellent choice. But using Firestore would have involved making the effort of moving to a document db structure; which seem like it would complicate the migration effort.
-   CockroachDB seemed like an excellent option. It offered a SQL-like interface that would have made the migration simpler. But I didn’t want to complicate the application administration by leveraging a third party service.
-   Arrow would have also been a good choice. It’s a performant data processing specification and it has support for Golang. The rising popularity of Arrow as a standard, wide variety of client libraries, and utilization of industry standard persistence formats makes this look like an excellent future friendly choice.
-   DuckDB allowed me to simplify the migration from a SQL based interface. It used a standard parquet based storage to allow for a future pivot. It did not require any additional services. And it seemed like a fun project to try out. This is the solution that I went with.

## Making the Switch

Leveraging DuckDB made the migration very simple from a code perspective. For the most part I was able to create a copy of the DB layer of the application and point it towards duckDB tables rather than my Postgres instance.

The queries for leveraging Postgres SQL vs DuckDB SQL are almost identical.

Before with Postgres:

After with DuckDB:

The two differences are 1) I am using a variable table name in DuckDB and 2) I had to cast the JSON column to duckDB’s JSON type.

The variable table name was a design decision. Storing user’s data in a separate table allows the application to pull the content from GCP to grab a much smaller subset of the data.

## Bumps in the Road

For some reason, that I may dig into in the future, the setup is pretty slow when a new instance starts and a user logs in for the first time. The process of logging in and loading the first page data takes about 2s. Once the user has logged in, the page response times are excellent. They are roughly the same speed as the SQL driven application. That first page load could use some attention though.

An issue I saw implementing DuckDB was how it unmarshalled a JSON column. For some reason I need to unmarshall the data twice to get it into my stuct. The first time from the db response into a string and then again from the string to my struct. This was annoying to troubleshoot, but simple to fix.

## Next Steps

There are several ways that I could improve my implementation.

1.  I could limit the retrieval of a user’s recipe information by checking if the data in GCP has changed. Currently the data is pulled from GCS for each request. But, unless a user is updating recipes from two separate devices, the server should have the most recent information.
2.   I could probably drop my table of users by leveraging an account id or some hash function of the account/provider.

## Wrapping Up

Getting rid of my Postgres instance has been a smashing success. I no longer own the most expensive recipe book on earth. DuckDB made the migration easy, and it works well for my needs. There may be better options available for your specific needs. But for now I can keep enjoying my easy-to-read recipes without worrying about the cost.

Loved the article? Hated it? Didn’t even read it?

We’d love to hear from you.

[Reach Out](https://www.simplethread.com/contact)
