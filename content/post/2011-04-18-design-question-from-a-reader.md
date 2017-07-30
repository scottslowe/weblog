---
author: slowe
categories: Musing
comments: true
date: 2011-04-18T11:59:56Z
slug: design-question-from-a-reader
tags:
- Microsoft
- Storage
- Virtualization
- VMware
- vSphere
- Windows
title: Design Question from a Reader
url: /2011/04/18/design-question-from-a-reader/
wordpress_id: 2273
---

I had a reader contact me and ask if he could ask the rest of the readers a vSphere design question. I thought that it might start an engaging and interesting discussion around vSphere design, so here's the reader's scenario and question(s):

>I am looking to design an ESXi environment to potentially deploy Microsoft SQL servers that require extreme high availability at a  scale of 50+ MSCS/WFC clusters. We'd like to do this in an ESXi 4.1 environment using Windows Server 2008 R2, MSCS/WFC, and SQL Server 2008 with Fibre Channel storage. I've done this in the past on a smaller scale (3-4 total clusters) and know most of the caveats such as proper heartbeat requirements, no HA/DRS support, physical RDM compatibility mode requirements for shared disks, eageredzerothick OS disks, no round-robin multipathing, etc.
>
>The issues I've run into in the past revolved around managing these virtual servers differently than other guests since they couldn't readily be moved between hosts. We also found that the reboot time on these hosts with MSCS/WFC using RDMs was extremely slow (in excess of 45 minutes to fully reboot, we could speed this up by pulling the fibre cables).
>
>Some of the design considerations I'm curious about would include:  
> 
>* Where do people put the VMFS/RDM file links?
> 
>* Do people put the guests in different clusters? Is this even possible?
> 
>* How do people separate active/passive nodes? Do people use host based affinity rules to accomplish this?
> 
>* Do reboot times on hosts with lots of RDMs get linearly slower as more MSCS/WFC RDMs are presented to a host?
> 
>* Do people really push back and try to get database mirroring instead of clustering? If so, what caveats around this have people encountered?
>
>I'm just curious how others are handling situations like this or if anyone is really doing it at scale.

Thoughts? What do you guys think about this reader's situation? I'd love for this to jump start a conversation here with recommendations, experiences, additional questions, etc. vSphere design is a topic that lots of readers are tackling, either for certification or just because that's their job, and the discussion around this scenario could end up exposing some useful resources and information.

So jump in with your thoughts in the comments below! I only ask that you provide full disclosure with regards to vendor affiliations, where applicable. Thanks, and I look forward to seeing some of the responses.
