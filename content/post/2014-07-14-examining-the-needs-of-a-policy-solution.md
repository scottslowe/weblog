---
author: slowe
categories: Information
comments: true
date: 2014-07-14T09:00:00Z
slug: examining-the-needs-of-a-policy-solution
tags:
- Cloud
- Interoperability
- OpenStack
- OSS
- Policy
title: Examining the Needs of a Policy Solution
url: /2014/07/14/examining-the-needs-of-a-policy-solution/
wordpress_id: 3468
---

In April of this year, we started a series of articles at [Network Heresy](http://networkheresy.com) on the topic of policy in the data center. The first of these articles, which I mentioned [in this post][1], focused on the problem of policy in the data center. This was a great introduction to the need for policy and the challenges with the current ways of addressing policy in the data center.

A short while ago, we published the second of our series on policy, titled ["On Policy in the Data Center: The solution space"](http://networkheresy.com/2014/06/19/on-policy-in-the-data-center-the-solution-space/). This post describes the key features/functionality that a policy system must have to address the challenges identified in part 1 of the series. In a nutshell (I _highly recommend_ you go read the full article), these key areas include:

* The sources from which policy is derived

* The language(s) used to express policy

* The way policy systems interact with data center services

* The actions a policy system can take

I really liked this statement from the article (this is in reference to how a policy system interacts with other services in the data center):

>A policy system by itself is useless; to have value, the policy system must interact and integrate with other data center or cloud services.

The relationship between a policy system and the ecosystem of data center services with which it interacts is so critical. Having a policy system is great, but if the policy system can't be integrated with other data center or cloud services, then it's not very useful, is it?

Go have a look at [the second post in the series on policy in the data center](http://networkheresy.com/2014/06/19/on-policy-in-the-data-center-the-solution-space/) and feel free to join in the conversation. You can leave comments here or at the Network Heresy site.

[1]: {{< relref "2014-04-22-kicking-the-policy-discussion-into-high-gear.md" >}}
