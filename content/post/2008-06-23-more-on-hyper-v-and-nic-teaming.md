---
author: slowe
categories: Rant
comments: true
date: 2008-06-23T14:00:23Z
slug: more-on-hyper-v-and-nic-teaming
tags:
- HyperV
- Microsoft
- Networking
- Virtualization
- Windows
title: More on Hyper-V and NIC Teaming
url: /2008/06/23/more-on-hyper-v-and-nic-teaming/
wordpress_id: 746
---

My [original article][1] on Hyper-V's issues with NIC teaming has gotten a fair amount of attention.

First Keith Ward over at Virtualization Review [blogged about this issue](http://virtualizationreview.com/blogs/weblog.aspx?blog=2282). In his initial post, Keith basically pointed out the issue and then asked the readers for feedback: is this really as big of an issue as it seemed? The readers who responded were split; one blasted Hyper-V and the other wasn't too concerned.

Keith followed that up with [another post](http://virtualizationreview.com/blogs/weblog.aspx?blog=2296) in which he provides a response from Microsoft regarding this issue:

>NIC Teaming is a capability provided by our hardware partners such Intel and Broadcom. Microsoft supports our partners who provide this capability. This is true whether the customer is running Windows, Exchange, SQL, Hyper-V, etc. We'll have a detailed KB article about this coming out soon.

Keith's second article was then also [picked up by DABCC](http://www.dabcc.com/article.aspx?id=7994).

While Microsoft is sticking to the "this is a device driver issue" mantra, I'm not so sure I agree. I can see their position to a point. In Keith's second post, analyst Chris Wolf brings up storage drivers. This is similar in that Microsoft relies upon the storage vendors to provide device-specific modules (DSMs) that provide the multipathing functionality. So, like with the NIC teaming, Microsoft is pushing the functionality back to the device drivers and vendors who write them.

But that's as far as this comparison can be taken. Microsoft officially supports storage multipathing; they don't officially support NIC teaming. (See [this KB article](http://support.microsoft.com/kb/888747) or [this KB article](http://support.microsoft.com/kb/254101/).) In addition, Microsoft provides an official framework in which the storage vendors can operate: the MPIO framework. There is no such framework for network redundancy. In fact, if such a framework existed then much of the dissatisfaction with Microsoft over this issue would be alleviated, in my opinion.

Instead, there is no framework to provide official NIC redundancy for any Microsoft product running on Windows Server, and Windows itself doesn't provide that functionality. Users are forced to adopt unsupported means to provide NIC redundancy. Why shouldn't they be upset?

By the way, since publishing the first article I've been contacted by one of the presenters of [the VIR358 session][2] during this which issue came to light, but he has not yet been able to provide any additional information. As soon as more information is available, I'll be sure to let everyone know here.

[1]: {{< relref "2008-06-12-significant-networking-problem-with-hyper-v.md" >}}
[2]: {{< relref "2008-06-12-vir358-hyper-v-architecture-scenarios-and-networking.md" >}}
