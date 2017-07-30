---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-29T09:00:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: Scaling to Your First 10 Million Users'
url: /2016/11/29/scaling-first-10mil-users/
---

This is a liveblog of the AWS re:Invent session titled "Scaling to Your First 10 Million Users." It's my first session of the week here at re:Invent; yesterday's sessions were full and I couldn't get into anything. (The crowds here at the event are pretty significant; I think I heard 32K attendees total.) The speaker for the session is Joel Williams, an AWS Solutions Architect.

Williams starts out with a brief blurb about how this session is a perennial favorite at re:Invent, and how the principles are fundamental to working in building solutions in/on AWS. Even if attendees don't have the sort of immediate scaling needs that Williams may be describing in this session, he believes that the lessons/fundamentals he discusses are applicable to lots of customers, lots of applications, and lots of use cases.

Williams starts out by saying that while Auto-Scaling is a destination on customers' scaling journey, it's _not_ where you want to start. It's not a "magic button" that fixes all problems. Williams puts up a map that shows AWS' 14 global regions, encompassing 38 different availability zones, and points out that availability zones are a fundamental building block for highly-available applications. The next map Williams shows depicts AWS' 60+ global points of presence (POPs), where you can employ services like Route 53 or CloudFront.

Looking at AWS' services, some of them are _inherently_ highly available and fault tolerant. Those include CloudFront, Route 53, S3, DynamoDB, ELBs, EFS, Lambda, SQS, etc. Other services, like EC2, can be made highly available with a little effort from the user/consumer.

One way to scale is by scaling to a bigger box, i.e., scaling to a larger instance type. This may help, but you _will_ eventually hit a wall---you can't scale vertically indefinitely. Further, scaling up doesn't address availability or redundancy.

Williams suggested that breaking out the database behind the example web application he's discussing would be a natural next step, since the database has different scaling needs than the web server/web tier. This leads into a disucssion of Amazon's various DBaaS offerings. One offering in particular that Williams calls out is Amazon Aurora, a MySQL-compatible offering that has automatic storage scaling, read replicas, continuous incremental backups to S3, and 6-way replication across availability zones.

If you're trying to decide between SQL or NoSQL, Williams suggests that users start with SQL databases. SQL is a well-established and well-worn technology, there's lots of existing knowledge/tools/communities, and there are clear patterns to scalability.

After a brief discussion of SQL vs. NoSQL, Williams goes back to the example web application that he's using to discuss scalability. In breaking out the database from the web tier, he's electing to use a relational database service from AWS. The next step is to add an Elastic Load Balancer (ELB) and distributing the application across two availability zones---this means 2 web instances and 2 instances of RDS (one active and one standby). This sort of architecture gets you greater scale as well as greater redundancy and fault tolerance. Further, the recently-announced Application Load Balancer (ALB) can offer content-based routing, container-based applications, WebSockets, and HTTP/2 support.

By moving to multiple instances together with an ELB/ALB, Williams points out that you can now scale vertically (larger instances) as well as horizontally. This architecture will get you to tens of thousands of users, but how do we go even farther?

First, you can use Amazon Aurora and read replicas to scale the data tier by offloading reads from the database master. Next, CloudFront and S3 can help "lighten the load" by offloading static assets from the web tier. Offloading assets to CloudFront reduces the load on the web tier, which further allows the web tier to scale even further.

Next, Williams discusses the use of DynamoDB---a managed NoSQL database---to store/manage session state for the web tier. He also mentions Amazon ElastiCache, which is a managed Redis/Memcached service. Using Memcached with ElastiCache will limit you to a single AZ; you'll need to use Redis for multi-AZ support.

At this point, Williams brings the session back to Auto Scaling. Auto Scaling enables "just-in-time provisioning," allowing users to scale infrastructure dynamically as load demands. He reminds users to be sure to create both scale-up policies _and_ scale-down policies, so that Auto Scaling Groups will both add instances when load increases and decommission instances when load decreases.

The next slide seems somewhat unrelated to the scaling discussion; Williams spends a few minutes talking about Elastic Beanstalk, OpsWorks, and CloudFormation. He further goes on to discuss CodeDeploy, CodeCommit, and CodePipeline, with only minimal references back to the discussion of scaling an application. To be honest, it felt more like a commercial than anything else.

Moving back to the application scaling discussion, Williams turns his attention to the pain points that will emerge as the number of users exceeds half a million users. Logging, monitoring, and metrics will be important here, as you seek to squeeze as much performance and efficiency out of every component.

SOA---or, dare I say, microservices---is the next step on scaling the application. Fortunately for AWS, there's a whole realm of managed services that can take the place of portions of the application as you break it down into smaller components. Amazon SQS (Simple Queueing Service) is one example, as is Lambda (event-triggered Functions as a Service [FaaS]). Williams hammers hard on the "loose coupling" approach.

By now, Williams says the architecture should support over a million users. How do we go further? More regions, more availability zones, of course, but we'll also want to consider constraints on the data tier. Users may need to consider federation (splitting into multiple DBs based on function; enables better distribution of resources at the cost of re-working the application), sharding (offers no practical limit on scalability but is more complex and requires re-working the application), or moving to a NoSQL data tier.

As a means of quick review:

* Use multiple regions and multiple availability zones.
* Use self-scaling services.
* Start with SQL.
* Cache data both inside and outside your infrastructure.
* Use automation tools in your infrastructure.
* Good monitoring/logging/metrics are important.
* Use Auto Scaling when you're ready for it.

What if you wanted to go past 10 million users? This will require deep analysis of your application and a lot of fine-tuning. Other services/products you may want to consider would include AWS EC2 Container Service (ECS) and greater use of Lambda.

At this point, Williams wraps up the session by offering some resources for additional reading and more information.
