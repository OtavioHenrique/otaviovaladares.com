---
layout:       post
title:        "A tale about application infrastructure"
subtitle:     "From physical server to containers orchestration"
date:         2019-11-15 19:20:00
author:       "Otavio H. P. Valadares"
header-img:   "img/in-post/a-tale-about-application-infrastructure/waterhouse_decameron.jpg"
header-mask:  0.5
catalog: true
multilingual: true
tags:
  - Infrastructure
  - Docker
  - Kubernetes
  - Theory
---

Today we’re facing the container revolution but I feel like most of the people don’t know why we are using it and what problems existed before it, and the history behind the evolution of the application deployment.

In this post, I talk about the evolution of the infrastructure of the applications, obviously, this is a topic that has size and information to be a book, but I tried to summarize it in few lines to understand the key points and the background quickly.

## Physical Server

A long time ago when developing applications, companies usually run their applications on typical physical rack servers,  these servers were basically like your computer running an application on top of an operating system.

![Physical Server X-ray](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/physical_server_xray.png)

It was very common to find small-medium companies that have a small room inside their office with a classic 19inch-rack with a server running one application, large companies usually build buildings called “datacenters” with tons of racks only to host their applications.

It was only possible to scale as we know today as “vertical scaling”, you only need to add more hardware to the next floor of your rack and that’s it.

![Physical Server Vertical Scaling](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/physical_server_scaling.png)

This model has a lot of trouble, the first one is that one server usually runs only one application and if this application doesn’t fully use server resources it may leave unused resources.

It was very expensive too, and not all companies had a budget to buy it, it was a problem for small companies or recent-founded companies.

At the beginning of the internet it worked, but with the growth of the internet, this model was not viable anymore.

## Virtual Host

A few years later, [RFC 2616](https://tools.ietf.org/html/rfc2616) introduced us to HTTP/1.1 and with him, [as described in the RFC, the ability to send a “host” header in a request providing the host and port information from the target URI, enabling the origin server to distinguish among resources while servicing requests for multiple hostnames on a single IP address.](https://tools.ietf.org/html/rfc7230#section-5.4)

This technology is called virtual host, and its most famous application is shared web hosting, with this one server can host multi websites. The price of hosting a website decreased, many businesses started to offer website hosting for a few dollars.

[Apache HTTP server supported virtual hosts by default, and it was a killer feature.](https://httpd.apache.org/docs/2.2/vhosts/name-based.html)

The mechanism is simple, the server analyzes the host header of an incoming request, and on its configuration you configure something like this “request for this site, go to this system path”. With this, you have the same IP address resolving DNS to multiple websites.

![Virtual Host Working](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/virtualhost.png)

At this point host, a web application was easier than bare metal servers but stills to have problems, and it takes us to the next step...

## Virtual Machine

Time goes on and technology from lates the 60s started to being used massively, I’m talking about operating system virtualization, this concept allows single hardware to host many operation systems or application, each one with its own operating system and environment while still sharing the same hardware resources.

![Virtual Machine X-ray](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/virtual_machine.png)

Using virtualization, a company just needs to buy a server with strong hardware and boot VMs as want (and the hardware support). Another good ability is to build custom OS images with pre-installed system requirements.

This new way of build application infrastructure maximized resource utilization and simplified application architecture, the price for deploy an application decreased,  and the most important, the growth of virtualization comes with the first IaaS companies, offering virtual machines allocation with “one-click”, the most famous example is the Amazon Web Services (AWS) with his famous EC2 service.

The period of more growth in IT operations around the world comes at the same time, millions of users using your application was a reality, cloud computing comes and new ways of think about infrastructure comes, microservices architecture comes in response to large applications, and now engineers don’t need to deploy a single monolith application, they need to deploy a lot of small applications.

Fallowed by the giants IT operations and tech companies, DevOps emerged and now companies make dozens of deploys/day, and new requirements on infrastructure show up. Some problems of virtualization come to mind, like that it’s not totally optimized, images are large and sometimes the boot of a new “instance” was slow.

A new technology trend called “containers” comes with the promise of change the way we think about IT infrastructure, and resolve all the problems related to virtual machines.

## Containers

While virtual hosts, virtual machines and all that story happened, researchers around the world were working too, and they began to advance on an implementation of an old but gold UNIX system feature called chroot, the OS-Level virtualization forerunner.

With a great time skip, in 2013 a technology called LXC was announced and later turned into the famous Docker Containers. This technology drew the attention of engineers around the world because it solves a lot of virtual machine problems.

Linux containers are a group of processes that are isolated from the rest of the system, think it is like a box isolated from the world, inside this box, you can put your application and all its dependencies and it will run isolated from the rest of the operating system, but using the same kernel of other process.

![Image illustrating putting application dependencies inside container](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/Container.png)

But now you can ask me, what the difference between Linux containers and virtual machines? The difference is simple and powerful.

![Difference between containers and virtual machines](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/containervsvirtual.png)

Linux containers provide process-level virtualization and don’t need to emulate the hole OS like virtual machines, they share the same kernel with the host OS (you can see on the image that Linux containers don’t boot an entire OS to work), its made containers lightweight, they can boot in milliseconds (VM usually needs minutes to boot), the containers images are smaller than VM images, containers have better performance than virtual machines and they are very secure, because each container and its process are isolated from the rest of the system.

Containers are great, not only to infrastructure but it changed the way of developing too, now you don’t have the famous problem of “works on my machine” anymore, you can code your application locally using containers and the environment that your application is running on your machine, will be the same that it will run on your infrastructure.

Engineers started to deploy their applications using containers (and develop too), probably using the famous Docker containers, but it wasn't using 100% of the potential of containers, it seemed that something was missing and thankfully the missing “something great” didn't take long to appear.

## Container Orchestration

It was the missing thing to use 100% of Linux containers’ power, a simple and beautiful way of thinking about infrastructure and application management, now the way of think about it is totally different from the beginning when you only think about a simple server running your application.

Let’s think about its concept, you have something that we usually call “operator”, the operator it’s like a big brother and is watching everybody inside its cluster, the cluster is composed of N nodes that usually are different machines and inside each machine, we have N Linux containers (Here’s the magic!).

As the name already says, the operator is operating our cluster, and he knows something that is like a recipe that the developer writes to tell orchestrator about how the application should behave when deployed.

This recipe tells standard things (and sometimes peculiarities of the chosen orchestration technology), like:

* Number of containers that your application will use (number of replicas)
* Memory and CPU reservation
* Healthchecks
* Horizontal/Vertical scaling rules

![Orchestrator and its nodes running containers](https://garagelabio.s3.amazonaws.com/a-tale-about-application-infrastructure/container_orquestration.png)

___The image is illustrating a cluster of four nodes, each one running N containers and the central orchestrator.___

When orchestrator starts to do its job, your infrastructure will have some kind of life, it will behave like an organism, it will boot new containers, kill old containers, replace unhealthy containers, scale your application based on metrics, share traffic between your N replicas, your application will have resilience and performance, a universe of possibilities will exist in your infrastructure based on this basic concept that uses Linux containers with clustering, load balancing, and metrics.

Following this concept, a lot of technologies have emerged and become popular to orchestrate containers, like, ECS, Docker Swarm, and the most hyped, Kubernetes (K8s). Each one has its own peculiarities and properties (Kubernetes being the most complex of them).

Summarizing, container orchestration is the automation of all aspects of coordinating and managing your containers, it manages the lifecycle, scaling, redundancy and much more for you.

Today, millions of people are using your software and you need to think about resiliency, scalability, monitoring and much more, container orchestration solves a lot of problems related to it, but it’s hard to get it working, it is heavy coupled with clustering and load balancing and other concepts that have grown together technologies described in this text.

## What’s next

We’ve talked about some important steps in the evolution of applications infrastructure until we arrive today’s powerful container orchestration, that allowed us to deal with today’s problems, but what’s next? What’re the next problems we will face when developing? What technology will solve these problems?  These questions only the time will answer but the lesson we take is to always be studying and adapting to new tendencies.

## Final thought

If you have any question that I can help you, please ask! Send an email (otaviopvaladares@gmail.com), pm me on my [Twitter](https://twitter.com/valadaresotavio) or comment on this post!

### Reference

https://devopedia.org/container-orchestration
https://avinetworks.com/glossary/container-orchestration/
https://www.hpe.com/us/en/what-is/container-orchestration.html
https://geekflare.com/docker-vs-virtual-machine/
https://www.backblaze.com/blog/vm-vs-containers/
https://www.redhat.com/en/topics/containers/whats-a-linux-container
https://blog.britesnow.com/understanding-kubernetes-value-867c163d5ed2
https://www.toptal.com/kubernetes/what-is-kubernetes
https://www.infoworld.com/article/3268073/what-is-kubernetes-your-next-application-platform.html
https://blog.britesnow.com/understanding-kubernetes-value-867c163d5ed2
https://en.wikipedia.org/wiki/Multitier_architecture
https://www.quora.com/What-is-the-difference-between-virtual-hosts-and-virtual-servers
https://phoenixnap.com/blog/what-is-bare-metal-server
https://www.techradar.com/news/the-rise-and-rise-of-bare-metal-servers
https://www.google.com.br/search?q=single-tenant+servers&oq=single-tenant+servers&aqs=chrome..69i57&sourceid=chrome&ie=UTF-8
https://en.wikipedia.org/wiki/Bare-metal_server
https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol
https://en.wikipedia.org/wiki/Data_center
http://www.iasl.com/solutions/virtual-servers/vmfaq
https://www.atlantech.net/blog/virtual-servers-vs.-physical-servers-which-is-best
https://www.nakivo.com/blog/physical-servers-vs-virtual-machines-key-differences-similarities/
https://en.wikipedia.org/wiki/Virtual_hosting#cite_note-1
https://www.idkrtm.com/history-of-virtualization/
https://searchservervirtualization.techtarget.com/feature/Whats-the-difference-between-Type-1-and-Type-2-hypervisors
https://en.wikipedia.org/wiki/Shared_web_hosting_service
https://www.redhat.com/en/topics/containers/whats-a-linux-container
