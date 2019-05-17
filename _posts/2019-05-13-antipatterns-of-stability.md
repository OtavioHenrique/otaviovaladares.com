---
layout:       post
title:        "Antipatterns of stability"
subtitle:     "Let's talk about some antipatterns"
date:         2019-05-20 02:04:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/antipatterns-stability/mym.jpeg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
  - Microservices
  - Patterns
  - Applications Architecture
---

On the last few weeks, I was reading the exceptional book Release It! Design and Deploy Production-Ready Software by Michael T. Nygard, this book is fantastic and I pretty recommend the reading for everyone, one of the chapters that have caught my attention was the part that he talks about patterns and antipatterns of stability.

System stability and availability is always a recurrent topic, it's a mistake to think that errors will not occur, it always occurs and on the worst time. Based on this, you need to defend your system of common events that when occurs kill your application and user experience.

At this post, I'll write my points of view about some systems stability antipatterns, some of these presented by Nygard at his book.

It's important to note that I don't show all the antipatterns presented by the author and that I made changes in some patterns to adapt to my point of view and explanation.

## Antipatterns

We'll talk about the antipatterns, they can fuck up your app if you don't pay attention to them. Each of these antipatterns will create, accelerate or multiply or system failure.

### Integration Points

All integration points is a risk to your systems, everyone can kill your operations or cause a headache to your team. And it’s important to remember that database call is an integration point too.

Let’s see an example, suppose that you have an application that needs to communicate with a 3rd party API that will give you essential information to your customer sign up, your customer can’t continue the registration flow while the external service doesn’t respond.

![Integration Point Failure](https://s3.amazonaws.com/garagelabio/antipattern-stability/integration_point_failure.png)

You can find similar scenarios across multiple companies, on this image we have a lot of possible events that may cause an error in your operation. Most developers when first look at this image, will think “Ok, there’s a problem! If the external application goes down, I’ll have a 404 response, my system will crash and I’ll not have users register while they don’t come back.”. This assumption is true but isn’t the only problem. How your system will behave if...

* The provider receives your request but never respond? If you don’t handle it, your user can wait forever!

* The provider receives your request and responds after 60 seconds? It can occur if the 3rd API is facing a DDoS attack, an exceptional number of requests or database high percentage of use.

* The response comes with a status code that you don’t know how to handle, like “423 Locked”. Probably you’ll get an error at your error tracking service.

* The response comes with a content type that you don’t know how to handle. It will go to your error tracking service too.

* And finally, you can get your response. But the response doesn’t come with the information that you expects. Imagine that you’re waiting for a JSON with the key “name”, but the JSON came without this key, what will happen on your application? If you don’t handle it, probably you’ll get an error trying to access this key. And again you’ll have a new error at your exception tracker.

Integration points are the number one risk of your application stability, but it’s unavoidable, today most applications are composed of microservices architecture and SaaS APIs, this is the main reason that I don’t consider it as an antipattern, for me the antipattern is each problem that came with each integration point.

Each of the problems described can give rise to a different antipattern, and you need to treat and don’t let it warm your system, the antipatterns can be named as:

* The endless request
* The long request
* The unexpected response code
* The unexpected content type.
* The unexpected content.

A lot of patterns that I’ll describe above (at “Patterns of Stability” section), can help you against each of these problems, for example, the famous circuit breaker pattern.

Every integration point will fail, and you need to be prepared.

### Cascading Failure

A cascading failure occurs when an error in one system affects others, with the initial failure walking down into your systems layer causing other errors.

Today majority of our applications is composed of microservices communicating between each one, and each service has its own dependencies, like a database, Redis and other services, each service has it's callers too, that probably depends directly on its stability.

![Microservices communicating between each other](https://s3.amazonaws.com/garagelabio/antipattern-stability/cascading_failure.png)

 What will happen if one of these services goes down or have a bug that makes the service unable to respond to one endpoint incoming requests?

![Fail communication between services](https://s3.amazonaws.com/garagelabio/antipattern-stability/cascading_failure_2.png)

Of course, the callers of this endpoints will have problems too, but how they handle it will define its future, or it will fail and start a cascading of failures or it will handle it correctly and don't start a crisis in your system.

This problem is similar to problems related to integration points, but it’s more about the mash of your systems itself. Circuit Breakers and failure modes can defend your system against this kind of problem, you shouldn't trust your system stability, always be ready for any issue, because it will always occur.

### Users

Don't store users sessions in memory, if you have a lot of users and crawlers navigating on your website, it can start an out of memory problem on your application.

### Blocked Threads

Blocked Threads is a point of attention, many failures like the chain of reactions and cascading failure start with a blocked thread. Like cascading failures, it usually occurs around a resource pool and integration points.

Use timeouts, especially on database connection to avoid problems with blocked thread. If you use any ORM it should already implement a friendly way to set timeout and threads pools on databases, ActiveRecord does it.

And obviously, use circuit breaker timeout on callers too.

### Self-denial Attacks

Self-denial Attacks occurs when someone tries to kill your own website, it's like a suicide!

Let's see an example: Your marketing pays U$5M for LeBron James tweet something about your company, but they don't tell anything to technology guys… What will happen when he sends the tweet for 40M people? Of course, your website will be a burst of visitors, and how it will behave with this? And if I tell you that your marketing team asked LeBron to tweet a deep link to a promotion, your application will still available?

Good marketing can kill your system, to prevent this kind of situation communication is the key factor, create static landing pages for users first interaction is recommended, and remember, never send deep links that bypass your CDN.

### Shared Resource

If you have a many-to-one or many-to-few resource relationship you probably will have a side effect when scaling. Let’s see a famous example, you have three applications using the same Redis:

![Three services using the same redis](https://s3.amazonaws.com/garagelabio/antipattern-stability/scaling-efects-img1.jpg)

If you increase by 2,3,4...x the number of services using this Redis instance, probably you’ll start to have a bottleneck.

![Seven services using the same redis](https://s3.amazonaws.com/garagelabio/antipattern-stability/scaling-efects-img2.png)

To avoid this kind of problem, on the book Michael T. Nygard propose a “shared-nothing” architecture by reducing the shared resources, reducing the number of services calling the shared resource.

![Each service with a unique resource](https://s3.amazonaws.com/garagelabio/antipattern-stability/scaling-efects-img3.png)

### Dogpile

Let's suppose that you are working for a big newspaper and the homepage executes a big query to list all the latest news by relevance, this query takes 6 seconds on average. You’re a good engineer and decides to use Memcached to stores this query result, after this the query is only executed to the first user to hit the homepage, after this your application stores the result on Memcached and the next users will have an instant response from cache, it’s working.

But what will occur when this cache expires? If your application is expiring the cache every five minutes, your query will be executed again every 5 minutes. If you have only one concurrent user it’s not a problem, but let’s imagine if you have four concurrent users, 24 requests will hit your server and executes the same query that consumes high CPU usage. Depending on the size of your application and the number of concurrent users, it can be harmful.

It is the most famous kind of dogpile and the solution is to code a “smart cache system”, you can find this solution on the internet for your favorite language.

Another common type of dogpile is with cached assets on your CDN, pay attention when you invalidate it, if you have all your user base hitting the CDN to view a common image or asset and you invalidate it, all these users will hit your server directly.

### Slow Responses

Slow responses are worse than a quick failure, it can be a result of excessive demand, or can also be a symptom of a problem like memory leaks or lack of a server. Slow responses can trig a cascading failure on the caller services (if they don’t implement timeout) or result in more traffic if you have users waiting for responses on your frontend.

To avoid this kind of problem, fail fast is the solution, every time that your system is slow it’s preferable to send an error. Don’t forget to use timeouts on the callers too.

### Unbounded Result Sets

Most applications don’t limit a query on the database, sometimes you can be surprised by a query that would return hundreds of results returning a million and your application crashing, this is a small antipattern but pays attention to your query, always limit them and use pagination.

This pattern avoid slow responses and together with timeouts, it can help avoid cascading failures Improves overall system stability.

## Conclusion

Stability and resilience are essential nowadays, the antipatterns that I wrote at this post is not all, and they don't prevent all possible failures, this topic is a vast ocean of knowledge and you can't stop studying.

No system is perfect, all systems have failures and you need to work to prevent it, but don't fall into the trap of perfection, you as a developer need to be pragmatic, and sometimes some solutions are over-engineering for your objective.

## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/opvaladares) or comment on this post!



