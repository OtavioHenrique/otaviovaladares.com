---
layout:       post
title:        "Types of storage on AWS S3"
subtitle:     "Understanding the particularities of each class"
date:         2018-12-27 01:10:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/types-of-storage-class/amazon.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
  - AWS
  - Tutorial
  - Beginners
  - Cloud
---

Amazon web services aka AWS provides a few ways of storing your files using the popular service S3(Simple Storage Service), the most common is the called S3 Standard that I think most people use on a normal day to day workflow, another common storage class is the famous S3 Glacier, but I don't think most people know what is Standard-IA storage class, or One Zone-IA, in this post I want to write about each one and its uses cases.

*It's important to emphasize that this post is dated December 2018 and AWS is constantly changing and adding new features and probably, storage classes, if this post is outdated, please comment or check at my blog if you find a newer one.*

*All prices will be based on `us-east-1` region.*

### Amazon S3 Standard

This is the default storage class, it's designed for frequently accessed data, you can use this class for many things that will be accessed frequently, like, images that your user uploads on your website to use as profile image, or high-usage spreadsheets, other use cases can be: cloud applications, dynamic websites, content distribution, mobile and gaming applications, and big data analytics.

#### Key Features

As the [aws official website say](https://aws.amazon.com/s3/storage-classes/), this is the key features of standard class:

- Low latency and high throughput performance (This is why it's very recommended for high-usage data)
- Designed for durability of 99.999999999% of objects across multiple Availability Zones (If you have 100 billion objects, Amazon ensures that you lost only one a year)
- Resilient against events that impact an entire Availability Zone
- Designed for 99.99% availability over a given year (Almost 52m 26s downtime per year)
- Backed with the Amazon S3 Service Level Agreement for availability
- Supports SSL for data in transit and encryption of data at rest
- S3 Lifecycle management for automatic migration of objects to other S3 Storage Classes

#### Pricing

Like almost all services provided, S3 has a long table of pricing separated by store amount, requests, data transfer, management, and transfer acceleration. You can check the full table [here](https://aws.amazon.com/s3/pricing/).

The S3 Standard Storage is the cheaper storage class when talking about request pricing and this is the main reason why you should use default S3 storage when you're working with heavy access, you pay a nice price like $0.0004 per 1.000 GET requests.

*At the end of the text I'll make a better comparison between each type and pricing*

### Amazon S3 Standard-Infrequent Access (S3 Standard-IA)

S3 Standard-IA is for data that is accessed less frequently, but when required needs fast access. You have the minimum storage period of 30 days and a minimum object size of 128KB. It is recommended for backups or logs(depending on the size of your logs, of course!).

Summarizing: All data that will not be accessed once a month and when required needs to be recovered just in time, you use S3 Standard-IA. Obviously, the problem is that you need to know previously the access pattern of your object, but as we'll see later, this problem is already solved by Amazon S3 Intelligent-Tiering.

#### Key Features

- Same low latency and high throughput performance of S3 Standard
- Designed for durability of 99.999999999% of objects across multiple Availability Zones
- Resilient against events that impact an entire Availability Zone
- Data is resilient in the event of one entire Availability Zone destruction
- Designed for 99.9% availability over a given year
- Backed with the Amazon S3 Service Level Agreement for availability
- Supports SSL for data in transit and encryption of data at rest
- S3 Lifecycle management for automatic migration of objects to other S3 Storage Classes

#### Pricing

The key point about pricing of S3 Standard-IA is that: Storage is cheap if you compare S3 standard, but a request is expensive.

| Storage | Price |
|-------------|---------------|
| All storage / Month | $0.0125 per GB |

| Requests | Price |
|-------------|---------------|
| Data Retrievals | $0.01 per GB |
| Data Returned by S3 Select | $0.01 per GB |
| Data Scanned by S3 Select | $0.01 per GB |
| PUT, COPY, POST or LIST Request | $0.01 per 1,000 requests |
| GET, SELECT and all other requests | $0.001 per 1,000 requests |

### Amazon S3 One Zone-Infrequent Access (S3 One Zone-IA)

This is the younger brother of S3 Standard-IA and is almost the same service, the difference is that it stores your data at only one AZ (Availability Zone), unlike other S3 storage classes which store data in a minimum of three. With this trade-off, it costs 20% less than its older brother. It's ideal for customers who want a lower-cost option for infrequently accesses data.

#### Key Features

The key features are the same as S3 Standard-IA, with the difference in the availability of 99.5% (Compared with 99.9% of Standard-IA).

#### Pricing

The request price is the same as Standard-IA, and the storage price is 20% cheaper.

| Storage | Price |
|-------------|---------------|
| All storage / Month | $0.01 per GB |

### Amazon S3 Intelligent-Tiering

This is the most interesting and newer one, it was released at November 2018, we saw that for high-usage files is better to use S3 Standard and to infrequent access data use Standard-IA, this is nice but we have one problem that some users already know, what we do if we don't know the access pattern of the object? or if we don't feel confident to change one object of the standard to Standard-IA?

Now imagine if Amazon creates something to do that for you? And they did it, and the problem has gone, this creation is the new Amazon S3 Intelligent-Tiering, it is designed for who want to optimize storage costs automatically when data access patterns change or you don't know it.

It works with two objects tiers, one tier is similar do S3 Standard and another tier that is similar to Standard-IA, all object, when uploaded, is moved to the first tier that is for frequent access data, if the object has not been accessed for 30 consecutive days, Amazon moves it to the second tier that is for infrequent access data. Amazing, isn't it?

*Objects smaller than 128KB will never be transitioned to the infrequent access tier*
*It's important to point that we don't have retrieval fees on this storage class.*

#### Key Features

- Same low latency and high throughput performance of S3 Standard
- Small monthly monitoring and auto-tiering fee
- Automatically moves objects between two access tiers based on changing access patterns
- Designed for durability of 99.999999999% of objects across multiple Availability Zones
- Resilient against events that impact an entire Availability Zone
- Designed for 99.9% availability over a given year
- Backed with the Amazon S3 Service Level Agreement for availability
- Supports SSL for data in transit and encryption of data at rest
- S3 Lifecycle management for automatic migration of objects to other S3 Storage Classes

As you can see the key features are almost the same, except for the 99.9% availability (comparing with 99.99% of standard S3) and the feature of automatically moves the object between access tiers.

#### Pricing

The price is awesome too when talking about storage pricing we have the same price of standard storage on frequent access tier, and the same price of Standard-IA to infrequent access tier.

| Storage | Price |
|-------------|---------------|
|Frequent Access Tier|
|-------------|---------------|
| First 50 TB | $0.023 per GB |
| Next 450 TB | $0.022 per GB |
| Over 500 TB | $0.021 per GB |
|-------------|---------------|
|Frequent Access Tier|
|-------------|---------------|
| All storage | $0.0025 per 1,000 objects |

For requests, we have the same price as the standard tier.

| Requests | Price |
|-------------|---------------|
| Data Returned by S3 Select | $0.0007 per GB |
| Data Scanned by S3 Select | $0.002 per GB |
| PUT, COPY, POST or LIST Request | $0.0005 per 1,000 requests |
| GET, SELECT and all other requests | $0.0004 per 1,000 requests |

### S3 Glacier

Glacier is designed to data archive if you have some data that you think you'll never need again and if needed you can wait for almost 5 hours of the recovery process, go ahead and use Glacier, the price of the storage is the cheaper of the S3 classes. Example of things to store on S3 Glacier: Old logs that you can't delete, old database dumps or your wedding pictures(just kidding but is a great idea).

One common action when working with Glacier is to setup a lifecycle policy to archive on Glacier data from S3 Standard bucket after `n` time, for example, 360 days, this is an amazing feature and provides a nice power to you.

#### Key Features

- Designed for durability of 99.999999999% of objects across multiple Availability Zones
- Data is resilient in the event of one entire Availability Zone destruction
- Supports SSL for data in transit and encryption of data at rest
- The low-cost design is ideal for long-term archive
- Configurable retrieval times, from minutes to hours
- S3 PUT API for direct uploads to S3 Glacier and S3 Lifecycle management for automatic migration of objects

#### Pricing

Amazon S3 Glacier has an extremely low cost of storage, cheaper than all other services that we studied.

| Storage | Price |
|-------------|---------------|
| All storage / Month | $0.004 per GB |

AWS provides three retrieval options, Expedited, Standard, Bulk, that range from a few minutes to hours and obviously from the cheapest to the most expensive one.

| | Expedited | Standard | Bulk |
|-------------|--------|-------|
| Retrieval Time | 1-5 Minutes | 3-5 hours | 5-12 hours
| Data Retrievals | $0.03 / GB | $0.01 / GB | $0.0025 / GB
| Retrievals Requests | $0.01 / Request | $0.050 / 1k Request | $0.025 / 1k Request

### Conclusion

All storage class has its specific case of use, choose one used to be a problem when you don't know the access pattern of the object, but today with S3 Intelligent-Tiering isn't any more you can enable it and Amazon take a care for you, if add a lifecycle police to archive data from this bucket to Glacier after some time, you are with an amazing solution at your hands.

If you know the access pattern, just choose the better to your case and use it, S3 is amazing and cheap!

### Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/opvaladares) or comment on this post!

### Links

[AWS blog post about Intelligent Tiering](https://aws.amazon.com/blogs/aws/new-automatic-cost-optimization-for-amazon-s3-via-intelligent-tiering/)

[Annoucing S3 Intelligent-Tiering](https://aws.amazon.com/about-aws/whats-new/2018/11/s3-intelligent-tiering/)

[S3 Pricing list](https://aws.amazon.com/s3/pricing/)

[S3 Storage Classes](https://aws.amazon.com/s3/storage-classes/)

[Glacier Retrieval Pricing Explained](https://www.cloudberrylab.com/resources/blog/amazon-glacier-retrieval-pricing-explained/)

[How to Cut your S3 Cost in Half by Using the S3 Infrequent Access Storage Class](https://www.concurrencylabs.com/blog/save-money-using-s3-infrequent-access/)
