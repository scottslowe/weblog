---
author: slowe
categories: Musing
comments: true
date: 2013-06-10T09:00:00Z
slug: the-symbiosis-between-automation-and-engineering
tags:
- Automation
- Puppet
title: The Symbiosis Between Automation and Engineering
url: /2013/06/10/the-symbiosis-between-automation-and-engineering/
wordpress_id: 3211
---

A little while ago Steve Beaver wrote a post titled ["Is Automation Killing The Engineering?"](http://www.virtualizationpractice.com/is-automation-killing-the-engineering-21687/) In the post, Steve ponders whether the increased use of automation in today's data centers is killing engineering knowledge. The argument, as I understand it, says that because tasks are becoming increasingly automated, data center professionals are increasingly less knowledgeable about how things actually work. But is this a valid argument? Is automation killing engineering?

I think there are at least two aspects to this idea that are worth exploring:

1. On one hand, increased attention to and use of automation is quite likely enabling some IT professionals to do things they weren't able to do manually. For example, I might not know very much about GlusterFS, but using a Puppet module for GlusterFS I could get it installed and configured without actually having to learn how it is done. (I think this is the trend that Steve is picking up on in his post.)

2. On the other hand, an increased focus on automation and configuration management is enabling IT professionals to more quickly accomplish things that otherwise would have taken more time and effort. Using the same example from before, while I might know exactly how to configure GlusterFS and get it up and running manually, using a Puppet module to do so makes the process quicker, more scalable, and the results more consistent.

It's up to us, as an industry and a profession, to ensure that we find---and maintain---the right balance between depth of engineering knowledge and extent of automation in our data centers. While the meme says "Automate all the things!", we have to ask ourselves, what does it make sense to automate? Further, we have to ensure that we are holding ourselves accountable to know _how_ the automation works. See, the pendulum can swing too far in either direction. We can carefully engineer and hand-craft our solutions ([snowflake servers](http://www.martinfowler.com/bliki/SnowflakeServer.html), anyone?), but that's not the most scalable approach. We can also blindly use automation tools without understanding how things work---in which case the solution might be up and running, but is it running well? Is it optimized? Is it actually meeting the requirements it needs to meet? We don't know, because we've allowed the pendulum to swing too far in the opposite direction.

I believe there is a careful symbiosis between engineering and automation that we must maintain. Yes, we _should_ use automation---it's a force multiplier that brings scalability and consistency. However, in order to ensure these consistent configurations are the _correct_ configurations, we need to have the right depth of engineering knowledge. We must ensure that we understand both _how_ the automation tools work as well as _what_ the tools are doing.

In other words, we have to live by this phrase I posted to Twitter some time ago (the individual tweet is long gone in the "stream of consciousness" that is Twitter, so I can't include a link):

>"You can't automate something if you don't understand it."

So, my advice to you: Start with automating what you know. Codify your existing engineering knowledge. Then, use the time freed up by those efforts to expand your knowledge, allowing you to automate new things once you understand them. This, I think, allows us to strike the correct balance between automation and engineering.

Disagree with what I've said here? That's OK---share your views, perspectives, or thoughts in the comments below. All courteous comments are welcome!
