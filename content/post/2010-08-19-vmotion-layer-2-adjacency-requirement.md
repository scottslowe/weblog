---
author: slowe
categories: Explanation
comments: true
date: 2010-08-19T10:42:46Z
slug: vmotion-layer-2-adjacency-requirement
tags:
- Networking
- Virtualization
- VMotion
- VMware
- vSphere
title: vMotion Layer 2 Adjacency Requirement
url: /2010/08/19/vmotion-layer-2-adjacency-requirement/
wordpress_id: 2025
---

The topic of vMotion, it's practicality, and Layer 2 adjacency for vMotion has been a topic I've visited a few times over the last several months. The trend got kicked off with a post on [vMotion reality][1], in which I attempted to debunk an article claiming vMotion was only a myth. The series continued with a discussion of the [practicality of vMotion][2], where I again discussed the various Layer 2 requirements for vMotion.

In the vMotion practicality article, reader Paul Pindell, an employee of F5 Networks, discusses the networking requirements for vMotion. To quote from his comment:

>Notice that there is no requirement for the vMotion VMkernel interfaces of the ESX(i) hosts to have what was termed Layer 2 adjacency. The vMotion VMkernel interfaces are not required to be on the same subnet, VLAN, nor on the same L2 broadcast domain. The vMotion traffic on VMkernel interfaces is routable. IP-based storage traffic on VMkernel interfaces is also routable. Thus there are no L2 adjacency requirements for vMotion to succeed.

I was intrigued by this statement, so I contacted Duncan Epping (of [Yellow Bricks](http://www.yellow-bricks.com/) fame) and discussed the matter with him. Duncan has also [posted on this topic](http://www.yellow-bricks.com/2010/08/19/layer-2-adjacency-for-vmotion-vmkernel/) on his site as well; both his post and my post are the result of our discussion and collaboration around this matter.

So is Layer 2 adjacency for vMotion a requirement, or not? In the end, the answer is that Layer 2 adjacency for VMkernel interfaces configured for vMotion **is not required**; vMotion over a Layer 3 interface will work. The caveat is that routed vMotion, as it has sometimes been called, hasn't been through the extensive VMware QA process and therefore **is not yet supported**. (Please don't mistake my use of the word "yet" as any sort of commitment by VMware to actually support it.)

In summary, then: vMotion across two different subnets will, in fact, work, but it's not yet supported by VMware. As additional information becomes available---as Duncan indicated, the VMware KB article is going to be updated to avoid misunderstanding---I'll update this post accordingly.

[1]: {{< relref "2010-06-13-the-vmotion-reality.md" >}}
[2]: {{< relref "2010-07-12-vmotion-practicality.md" >}}
