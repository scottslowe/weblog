---
author: slowe
categories: Musing
comments: true
date: 2008-04-16T21:28:07Z
slug: the-dark-side-of-virtualization
tags:
- ESX
- Security
- Virtualization
- VMotion
- VMware
- VMwareHA
title: The Dark Side of Virtualization
url: /2008/04/16/the-dark-side-of-virtualization/
wordpress_id: 683
---

In [The Four Horsemen of the Virtualization Security Apocalypse](http://rationalsecurity.typepad.com/blog/2008/04/the-four-horsem.html), Chris Hoff shines a great big spotlight on the dark side of virtualization security (or virtsec, as its increasingly being known). To quote from Hoff's article:

>Short of the notions I've discussed previously regarding instantiating the vSwitches into hardware and loading physical servers with accelerators and offloaders for security functions, there aren't a lot of people talking about this impending set of challenges or the solutions in the short or long term.  
>
>This should be cause for alarm.  
>
>These issues are nasty. Combined with the organizational issues of who actually owns and manages "security" in the virtualized context, this stuff makes me want to curl up in a fetal position.

I agree with what Hoff has to say and I'm glad he's taking the time to boil down the issues so that non-security-minded IT pros can really understand the problems. However, Hoff, I have to take you to task for one thing in your article: the kitten thing was just too much. _Poor little kitten..._

I particularly agree with Hoff's #1 point ("Virtualized Security Screws the Capacity Planning Pooch"). The idea behind VMsafe and all these virtsec appliances is a great idea and all, but what about the overhead? At what point does having all this "extra" security so greatly bog down our virtualization engine that it's no longer worth it to virtualize? And how do we actually, realistically begin to address this issue? Do we move the security functions into the hypervisor itself? And while this _might_ address the performance concerns---although I don't think so---isn't this just instantiating Hoff's vUTM?

One of the interesting things that I hope to be able to do soon is try to measure the overhead of some of the virtsec appliances that are currently available on the market. Not to publish any results or hit any vendors over the head with the information, but just to have a better idea for myself and my customers about how this stuff actually behaves in the real world. If anyone has already done that sort of thing and is willing to share their information with me, I'd be mighty appreciative.

I am curious about something---how many organizations are using a single physical host with VMs across different security zones? See, this is something that I would _never_ recommend, and to me it seems like physically segregating your security zones into different virtualization environments solves a fair number of the concerns about the "dynamic data centers" created by VMotion, VMware DRS, and VMware HA. Or am I overlooking a critical aspect?
