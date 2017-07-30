---
author: slowe
categories: Liveblog
comments: true
date: 2016-11-29T15:00:00Z
tags:
- AWS
- reInvent2016
title: 'Liveblog: Getting the Most Bang for Your Buck with EC2'
url: /2016/11/29/getting-bang-buck-ec2/
---

This is a liveblog of the AWS re:Invent session titled "Getting the Most Bang for Your Buck With #EC2 #Winning" (CMP202). The speaker for the session is Joshua Burgin, General Manager, EC2 Spot Business. According to the abstract, this session is supposed to focus on effectively using on-demand instances versus spot instances and reserved instances.

As a matter of quick introduction, there are three purchasing options for EC2:

* On-demand: "pay as you go"; no long-term commitments
* Reserved: good for steady-state workloads, used with 1 yr or 3 yr commitment
* Spot: pay market price for unused compute capacity

How do you choose which one to use? Burgin shares the "four pillars of performance and cost optimization":

1. _Right-sizing:_ choosing the cheapest instance available while meeting performance requirements
2. _Purchasing options:_ Burgin will discuss this in more detail; this is the primary focus of the discussion
3. _Increase elasticity:_ turning off ("scaling down") instances that don't need to be running (example: turn off development workloads when the developers aren't working) 
4. _Measure, monitor, and improve:_ tagging resources; identitying always-on instances; identifying instances that can be downsized; recommending Reserved Instances (RIs) where it makes sense; dashboards and reports

Burgin points out the key AWS pricing principles (no up-front investment; pay as you go; pay less when you reserve; pay less when AWS grows), and how that means that you aren't "locked in" based on decisions you make. (When I say "locked in", I mean that decisions can be changed/undone relatively easily; I'm not referring to vendor/techology lock-in.)

With correct optimization, costs can be dramatically reduced compared to building on-premises solutions. Burgin shares an example in which a customer saves $40 million by not having to build a large dedicated on-premises infrastructure. Burgin does admit that this solution isn't necessarily applicable to all instances, but is a great example.

Burgin now shifts back to a discussion of purchasing options, the main focus on this session. The first purchasing option he discusses is on-demand pricing. It's low cost and flexible, and is well-suited for short-term spiky workloads.

The second purchasing model is Reserved Instances (RIs). They are ideally suited for steady-state workloads, and offer a significant discount compared to on-demand. Cost reductions are up to 75% compared to on-demand. Recently AWS introduced the ability to convert RIs from one instance family to another instance family; this allows customers to take advantage of new instance families/types over the course of the commitment of the RI. Convertible RIs require a 3 year commitment but also offer a lower savings compared to "traditional" RIs.

The third purchasing option is spot instances. They are ideal for time- or instance-flexible workloads. Spot instances can offer up to 90% savings compared to on-demand instances. There's no commitment level, and AWS offers tool like Spot Fleet to help manage your spot instances.

Spot instances are, essentially, unused on-demand instances. There are two prices involved: the _market price_ and the _bid price_. The market price is what you'll pay. The bid price will determine how much/how often your spot instances will be reclaimed by AWS (when the instances are reclaimed by AWS your workloads will be interrupted). This model offers some pretty amazing savings, but requires a great deal more work (more management, more application awareness, more coordination, etc.).

Next, Burgin asks: which purchasing option is right for me?

The answer: _It depends._ (Of course!) Burgin says there are use cases that make sense for on-demand, reserved, and spot instances, and I totally agree.

Before he goes further in showing examples, Burgin takes a quick sidetour into tagging. Tagging, according to Burgin, is critical for understanding cost and being able to optimize cost when running EC2 instances.

Moving back to an example of how to optimize cost, Burgin talks about how to optimize cost for a stateless web application. You'd start with a "base layer" of RIs to handle the steady state of the web site. As load increases but has not yet reached critical load levels, you can leverage spot instances. At higher peak usage levels, on-demand instances handle the final load levels.

At the application tier, you would again have a base layer of RIs, but would supplement the RIs with on-demand instances. If you have details about periodic peaks, you can leverage spot instances (specifically, spot blocks) to help address those peaks.

Finally, at the database tier, you'd leverage RIs. There may be use cases for on-demand or spot instances for things like running reports or something like that, but generally RIs are the right fit for the database tier.

Burgin next mentions additional services like SQS, Lambda, etc., as opportunities to further eliminate servers (instances).

After a somewhat detailed discussion of on-premises purchasing provisioning patterns---somewhat focused on grid computing use cases---Burgin shares another example how combining RIs, on-demand, and spot instances to address the "conflicting needs" of IT versus the business. IT gets effective/efficient usage of resources, while the business gets access to computing resources when they need them.

Next, Burgin shares some common purchasing patterns across various industries:

* Web scale companies typically use quite a bit of on-demand but a ton of spot instances
* Enterprise SaaS companies generally use RIs, with only minimal supplementing that capacity with on-demand instances and spot instances
* An "onboarding enterprise" (an enterprise moving to the cloud) will use RIs in a "step function" where the amount of RIs will increase at certain periods; generally these RI increases will happen in response to generally increasing on-demand instance usage
* Gaming companies would typically have a strong base layer of RIs, and would use a heavy number of on-demand instances during a game launch, a promotion, or similar
* Scientific research companies have a small number of RIs, a slightly larger number of on-demand instances, but will have giant leaps of huge numbers of spot instances for a very short period of time. These short spikes represent some "crazy idea" involving computational research or similar.
* A generic "technology company" will have a blend of reserved instances, on-demand instances, and spot instances, each tailored to particular to particular use cases. (Burgin at this point mentions a Jenkins plug-in to help leverage spot instances.)

Burgin next shares how the typical usage patterns he shared across industries can also be generalized in some cases to represent different functions within a single large company (internal IT, new application development, test/dev, etc.).

At this point, Burgin recaps the key points from the presentation:

* Remember the pillars of optimization (right-sizing, increasing elasticity, measuring/monitoring/improving, and reviewing purchasing options)
* Use tags to understand your services (very important!)
* Have a "balanced meal" across the 3 purchasing options---use the right purchasing option for the right workload
* Architect your workloads with performance and cost in mind (move from monolithic to microservices architectures, etc.)

At this point, Burgin wraps up the session.
