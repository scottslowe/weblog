---
author: slowe
categories: Information
comments: true
date: 2008-08-28T10:11:56Z
slug: virtualization-short-take-17
tags:
- ESX
- ESXi
- Hardware
- HP
- HyperV
- SAN
- VDI
- Virtualization
- VMware
- VMworld2008
title: 'Virtualization Short Take #17'
url: /2008/08/28/virtualization-short-take-17/
wordpress_id: 844
---

News, views, events, and commentary from around the virtualization world that struck my fancy over the past few weeks---that's what you'll find in Virtualization Short Take #17!

* I stumbled across this series of podcasts on building a scalable virtual desktop deployment. It's a four part series: [Part 1](http://www.podtech.net/home/5306/designing-and-implementing-a-scalable-virtual-desktop-deployment-part-1), [Part 2](http://www.podtech.net/home/5309/designing-and-implementing-a-scalable-virtual-desktop-deployment-part-2), [Part 3](http://www.podtech.net/home/5316/designing-and-implementing-a-scalable-virtual-desktop-deployment-part-3), and [Part 4](http://www.podtech.net/home/5319/designing-and-implementing-a-scalable-virtual-desktop-deployment-part-4). I'm still struggling with finding a way to incorporate podcasts into my day; I'm drinking from the firehose as it is. For those that do listen to podcasts, perhaps this series will prove helpful.

* Apparently, the argument about VMware violating the GPL (I wrote about that [here][1] quite some time ago) has surfaced again. Gordon Haff promptly and rather cleanly [squashes that concern](http://news.cnet.com/8301-13556_3-10011395-61.html?hhTest=1&tag=blogFeed). Well said, Gordon.

* Tim Jacobs posts a great article on [matching LUNs between ESX and the VCB proxy server](http://timjacobs.blogspot.com/2008/08/matching-luns-between-esx-hosts-and-vcb.html). This is  just one of several good posts by Tim; here's another one on [VSS snapshots with ESX 3.5 Update 2](http://timjacobs.blogspot.com/2008/07/full-backups-of-virtual-machines-and.html). This is a web site to watch.

* Ben Armstrong reminds us [that Hyper-V requires NTFS](http://blogs.msdn.com/virtual_pc_guy/archive/2008/08/28/hyper-v-vm-on-usb-disk-fails-to-start.aspx) in order to work. Isn't it time for FAT32 to go away?

* Will VMware ESX/ESXi 3.5 Update 2 be the [first hypervisor validated under the SVVP](http://www.gabesvirtualworld.com/?p=79)? Gabe posts some information on his site to that effect, indicating that VMware expects to announce validation before VMworld. That would be a strong competitive advantage, indeed.

* VMware is also hitting hard with performance advantages and ties to hardware acceleration, as indicated by John Troyer's post regarding [VMDq, VMware Netqueue, and VMDirectPath](http://blogs.vmware.com/vmtn/2008/08/netqueue-vmdire.html) (seen [via VMblog.com](http://vmblog.com/archive/2008/08/22/vmdq-and-vmware-netqueue-with-your-virtual-10gbe-nics.aspx)). The coolest part about this stuff is that a lot of it is _already available._ We're not waiting for a future version of VMware Infrastructure to take advantage of this stuff; instead, we're waiting for the hardware. Now how's that for a change!

* Rich over at VM /ETC gives us a great breakdown on [licensing Windows VMs for a non-Microsoft virtualization solution](http://vmetc.com/2008/08/26/how-to-license-windows-vms-in-a-non-microsoft-virtual-environment/). He comes to the same conclusion some of my customers have already made: licensing Windows Server 2008 Datacenter Edition may make the most sense. Good job, Rich!

* Duncan alerts us to a [potential problem with HP Insight Manager agents on VMware ESX 3.5 Update 2](http://www.yellow-bricks.com/2008/08/27/why-i-dislike-agents-in-my-service-console/). I share Duncan's view; I try to avoid agents in the Service Console wherever possible. That's why I was glad to see VMware add the Health Status functionality in Update 2, as this gives you some hardware monitoring functionality without any agents in the Service Console. I don't know that I've quite arrived at the same place as Duncan; I like the Service Console. I guess that's because I'm accustomed to it. In any case, if you're running HP hardware with the IM agents, be on the lookout for this issue. Thanks for bringing this to everyone's attention, Duncan!

* Interested in keeping up with all the various goings-on at VMworld? David Davis turns us on to a [few additional sources of information](http://blogs.virtualizationadmin.com/davis/?p=56) (as if we needed anything more!).

Also, I'm excited to announce that my blog has been selected for inclusion in a new blog aggregation site, [VirtualizationFeed.com](http://www.virtualizationfeed.com/). Patrick over at Microsoft announced the new site yesterday via the Virtualization Team Blog with [this post](http://blogs.technet.com/virtualization/archive/2008/08/27/Virtualization-Feed-for-Your-RSS-Reader.aspx). I'm thrilled to be included and I hope to be able to continue to deliver quality information. Thanks for reading!

[1]: {{< relref "2007-08-19-the-linux-kernel-binary-modules-and-esx-server.md" >}}
