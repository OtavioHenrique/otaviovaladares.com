---
layout:       post
title:        "Types of storage on AWS S3"
subtitle:     "Understanding the particularities of each type"
date:         2018-12-23 01:10:00
author:       "Octos"
header-img:   "img/in-post/types-of-memory-allocation/amazonia.jpg"
header-mask:  0.5
catalog:      true
multilingual: true
tags:
  - AWS
  - Tutorial
  - Beginners
  - Cloud
---


Amazon web services aka AWS provides a few ways of storage your files using the popular service S3(Simple Storage Service), the most common is the called S3 Standard that I think most people use on normal day to day workflow, other common storage class is the famous S3 Glacier, but I don't think most people knows what is Standard-IA storage class, or One Zone-IA, on this post I want to write about each one and its uses cases.

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

Like almost all services provided, S3 has a long table of pricing separated by store amount, requests, data transfer, management and transfer acceleration. You can check the full table [here](https://aws.amazon.com/s3/pricing/).

The S3 Standard Storage is the cheaper storage class when talking about request pricing and this is the main reason why you should use default S3 storage when you're working with heavy access, you pay a nice price like $0.0004 per 1.000 GET requests.

*At the end of the text I'll make a better comparison between each type and pricing*

### Amazon S3 Intelligent-Tiering

This is the most interesting and newer one, it was released at November 2018, we saw that for high-usage files is better to use S3 Standard and to infrequent access data use Standard-IA, this is nice but we have one problem that some users already know, what we do if we don't know the access pattern of the object? or if we don't feel confident to change one object of standard to Standard-IA?

Now imagine if Amazon creates something to do that for you? And they did it, and the problem has gone, this creation is the new Amazon S3 Intelligent-Tiering, it is designed for who want to optimize storage costs automatically when data access patterns change or you don't know it.

It works with two objects tiers, one tier is similar do S3 Standard and another tier that is similar to Standard-IA, all object when uploaded is moved to the first tier that is for frequent access data, if the object have not been accesses for 30 consecutive days, amazon moves it to the second tier that is for infrequent access data. Amazing, isn't it?

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

As you can see the key features are almost the same, except for the 99.9% availability (comparing with 99.99% of standard S3) and the feature of automatically moves object between access tiers.

#### Pricing

The price is awesome too, when talking about storage pricing we have the same price of standard storage on frequent access tier, and the same price of Standard-IA to infrequent access tier.

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

For requests we have the same price of standard tier.

| Requests | Price |
|-------------|---------------|
| Data Returned by S3 Select | $0.0007 per GB |
| Data Scanned by S3 Select | $0.002 per GB |
| PUT, COPY, POST or LIST Request | $0.0005 per 1,000 requests |
| GET, SELECT and all other requests | $0.0004 per 1,000 requests |

### Amazon S3 Standard-Infrequent Access (S3 Standard-IA)
