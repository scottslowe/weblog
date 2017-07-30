---
author: slowe
categories: Liveblog
comments: true
date: 2006-11-08T10:40:13Z
slug: vmworld-2006-day-2-keynote
tags:
- Virtualization
- VMware
- VMworld2006
title: VMworld 2006 Day 2 Keynote
url: /2006/11/08/vmworld-2006-day-2-keynote/
wordpress_id: 357
---

The majority of the keynote was handled by Mendel Rosenblum, cofounder of VMware and an operating system researcher who also teaches OS classes at Stanford University. Mendel spoke at length about the functions of the virtualization layer and how those functions might be extended and/or enhanced.

While nothing was officially announced, Mendel did demonstrate the idea of using the virtualization layer to capture or record a stream of execution by a virtual machine (VM). This stream of execution could then be replayed against another VM, which he demonstrated using a prerelease version of VMware Workstation 6.0. This has immediate implications for OS forensics, but I also see tremendous implications in BC/DR (business continuity/disaster recovery). Think of the idea of a VM running on a virtual infrastructure in one datacenter, with a stream of execution on that VM being shipped across to a hot standby VM in another datacenter in an entirely different city. It's like using SAN replication between geographically separate datacenters, but includes real-time changes to memory state and CPU activity---not just disk changes. That's very exciting stuff. The possibilities of what could be done with that kind of information are almost endless.

After Mendel concluded his speech, a group of academic researchers took the stage to discuss the future of virtualization. Mendel was included here again, as were researchers from Columbia University and the University of Wisconsin, among others. It was an interesting discussion with a variety of other viewpoints regarding virtualization and where it may head in the future.

One thing keeps popping into my head, though, and it was something mentioned during my BoF discussion at breakfast. Twice during the VMworld 2006 conference the idea of taking a hosted desktop (hosted in a VDI-type scenario) offline to be used on a local system has been mentioned. Is this the next direction for VMware ACE? To me, making a connection between VDI and VMware ACE such that ACE images could be hosted on an VI3 farm and then "checked out" to be run on a local workstation when the user is away is a powerful and compelling innovation that I think would _really_ change the picture of VDI and VMware ACE today. ACE is a great idea, but companies just haven't gotten ahold of it. Could a connection between VDI and VMware ACE drive greater adoption? Is this also related to the push on virtual appliances, further reinforced by Mendel Rosenblum's mention of virtual appliances to handle desktop images today during the panel discussion? Put your two cents' worth into the comments and tell me if you think I'm on to something or if I'm just crazy.
