---
author: slowe
categories: Information
comments: true
date: 2015-03-12T08:35:00Z
tags:
- OpenStack
- Cloud
- OSS
- Policy
title: A Quick Update on OpenStack Congress
url: /2015/03/12/quick-update-congress/
---

OpenStack Congress, a project aimed at providing "policy as a service" for OpenStack clouds, is a project I've had the privilege of being involved in from very early days. I [first mentioned Congress][xref-1] almost a year ago, and since then the developers have been hard at work on the project. Recently, one of the lead developers posted a summary of some pretty impressive performance improvements that have been made with Congress.

I won't repeat all the sordid details here; for all the details, I encourage you to go read [the full post over at ruleyourcloud.com][link-1]. Just to give you a quick highlight of some of the performance gains they've been able to realize, consider these numbers:

* A 500x improvement in query performance
* A 20,000x increase in data import speed
* A 6x reduction in memory overhead

Given the nature of Congress---that it must, by its very definition, import data from multiple cloud services and perform queries across that data to determine policy violations---the performance improvements seen in query performance and data import speeds are quite significant.

For the detailed explanation of how the developers were able to see such incredible performance improvements, see [the full post][link-1]. If you're interested in learning more about Congress, [ruleyourcloud.com][link-4] also has a number of other Congress-related posts, or you can check out [the Congress project wiki][link-2]. You can also check out [the code on GitHub][link-3].

Keep up the great work, Congress team!


[link-1]: http://ruleyourcloud.com/2015/03/12/scaling-up-congress.html
[link-2]: https://wiki.openstack.org/wiki/Congress
[link-3]: https://github.com/stackforge/congress
[link-4]: http://ruleyourcloud.com/
[xref-1]: {{< relref "2014-04-22-kicking-the-policy-discussion-into-high-gear.md" >}}
