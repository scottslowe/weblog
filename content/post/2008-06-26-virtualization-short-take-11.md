---
author: slowe
categories: Information
comments: true
date: 2008-06-26T11:17:29Z
slug: virtualization-short-take-11
tags:
- CLI
- ESX
- ESXi
- HyperV
- Storage
- Virtualization
- VMware
- VMwareHA
- Xen
title: 'Virtualization Short Take #11'
url: /2008/06/26/virtualization-short-take-11/
wordpress_id: 750
---

Here in Virtualization Short Take #11, I offer to you a collection of virtualization-related news and tidbits and my thoughts on them.

* I seem to be on a bit of kick reading Ryan Arneson's stuff these days. This time is actually an _older_ post of Ryan's on [using the COMSTAR stuff from Sun with ESX](http://blogs.sun.com/rarneson/entry/an_early_look_comstar_and). It's an interesting read. I'm quite fascinated by the myriad things that Sun is doing with storage, and I hope that some of these actually get backed with good execution. I've guess I've heard the saying "Sun is where storage goes to die" from too many Sun veterans.

* I was notified of this post by Chris Barclay of Virtual Iron regarding a [comparison of Virtual Iron Virtualization Manager and Citrix XenCenter](http://blog.virtualiron.com/Virtual-Infrastructure/2008/06/virtual_iron_virtualization_ma.html). This is an interesting comparison considering that both products are built on the same underlying hypervisor (Xen). In this case, Chris makes the argument that management is the piece that sets one virtualization solution apart from other solutions, and that in this particular case Virtual Iron's management capabilities far exceeds those provided by XenCenter. I don't have any direct experience with either of these products, so I can't attest as to the accuracy of his claims. While I don't necessarily agree that the hypervisor is being commoditized, I do agree that management is increasingly becoming the factor that distinguishes solutions. In this regard Microsoft has an early lead, in my opinion, with cross-platform VM management inside Virtual Machine Manager 2008. Will other vendors follow suit?

* Last week the new VMware Networking blog [posted a notice](http://blogs.vmware.com/networking/2008/06/deploying-vi-wi.html) about a new whitepaper jointly authored by VMware and Cisco. Duncan over at Yellow Bricks [also picked this up](http://www.yellow-bricks.com/2008/06/17/cisco-and-vmware-best-practice/), but from a different [source](http://www.xanalisys.com/Virtualization/VMware-Infrastructure-3-in-a-Cisco-Network-Environment.html); the whitepaper, however, appears to be the same from both sources. I haven't had the opportunity to fully review it yet, but I do plan to do so and will highlight any notable recommendations here.

* Chad Sakac, the "VMware Guru" for EMC, published an entry on [stretched ESX clusters](http://virtualgeek.typepad.com/virtual_geek/2008/06/the-case-for-an.html). This article was picked up by a number of other bloggers ([here](http://vmetc.com/2008/06/15/can-you-vmotion-between-different-physical-data-centers/) or [here](http://www.yellow-bricks.com/2008/06/20/update-your-bookmarks/), for example), so I won't rehash it all here again. The timing on the article was helpful; he wrote that and not more than two days later I had a customer asking about doing this very thing. Personally, I agree with Chad that it's generally a bad idea, and so it was handy to be able to point the customer to this article as further support. One other thing I did get out of Chad's post---how many of you picked up that up to 10 different isolation addresses can be configured? Is that in the documentation somewhere and I just missed it?

* Continuing on with Chad, it appears that an [old VMware HA article][1] of mine is useful in helping to understand how the VMware HA admittance algorithm works. [Chad's article](http://virtualgeek.typepad.com/virtual_geek/2008/06/so-how-exactly.html) provides excellent details on the key concepts to understand.

* Most readers have probably seen the article describing how to access the ESXi command line. [This article](http://cid-50811f3490d13c0a.spaces.live.com/blog/cns!50811F3490D13C0A!163.entry) also shows you how to enable SSH access to that CLI. I found this information so handy that I added it to my del.icio.us bookmarks. As ESXi gains broader adoption, this kind of stuff will be very useful.

* With the release of Hyper-V, [comparisons of Hyper-V vs. ESX](http://weblog.infoworld.com/enterprisewindows/archives/2008/06/hyperv_gets_att.html) will become much, much more common. Here's [another one](http://searchvmware.techtarget.com/news/article/0,289142,sid179_gci1314298,00.html#) for review as well. I'll echo the comments in [this article](http://channelvirtualization.wordpress.com/2008/06/19/microsoft-teched-2008-hyper-v-versus-esx/) regarding the comparisons: it's not about the brand, or the technology, it's about the _solution._

* I'll have to partially disagree with the sentiment behind this article regarding [the use of virtualization as a DR tool](http://searchdatabackup.techtarget.com/tip/0,289483,sid187_gci1315216,00.html?track=NL-1060&ad=643818&asrc=EM_NLT_3768622&uid=4075918#). The article intends to present 5 things that should be considered when using virtualization for DR, but does not, IMHO, accurately present some of the challenges around virtualization for DR. How are the VMs being replicated over to the DR site? Replication technologies need to be properly coordinated with the virtualization software so that the data being replicated is consistent and useable. If this is synchronous replication it's not as much of an issue, but it's definitely an issue with asynchronous replication. What about registering VMs on the DR site? How does one handle VirtualCenter in this kind of scenario? Is testing failover really that easy? My experience indicates that while virtualization can certainly assist in creating a good DR plan, it's only one part of an overall DR solution, and it can create its own unique challenges. Again, the timing of this is interesting; I just came across the article after finishing up a presentation about the use of virtualization in disaster recovery solutions.

* Anyone working in the VDI environment has almost certainly had more than their fair share of discussions about remote display protocols. This article on x86virtualization.com provides a decent [overview of VNC, RDP, ICA, and Net2Display](http://x86virtualization.com/x86-virtualization/vnc-vs-rdp-vs-ica-vs-net2display.html). Seems like I recall seeing something somewhere about VMware assisting in the development of Net2Display; anyone know anything more about that?

I guess that about does it for this round. Thanks for reading, and feel free to share your thoughts in the comments.

[1]: {{< relref "2008-01-07-vmware-ha-clarification.md" >}}
