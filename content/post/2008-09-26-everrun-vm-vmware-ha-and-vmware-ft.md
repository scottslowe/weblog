---
author: slowe
categories: Explanation
comments: true
date: 2008-09-26T11:43:23Z
slug: everrun-vm-vmware-ha-and-vmware-ft
tags:
- Citrix
- ESX
- Virtualization
- VMware
- VMwareFT
- VMwareHA
- Xen
title: everRun VM, VMware HA, and VMware FT
url: /2008/09/26/everrun-vm-vmware-ha-and-vmware-ft/
wordpress_id: 954
---

Having previously discussed Marathon Technologies' everRun VM product [in conjunction with XenServer HA][1] as part of XenServer 5, I think that I have some useful information to bring to the recent discussion that has come to light about everRun VM vs. VMware HA vs. VMware FT.

Apparently, this discussion started with a blog entry by Marathon titled [VMware FT - The Top Four Reasons it's Kinda Sorta Fault Tolerance](http://marathontechnologies.net/2008/09/16/vmware-ft-%E2%80%93-the-top-four-reasons-it%E2%80%99s-kinda-sorta-fault-tolerance/). Mike DiPetrillo of VMware responded from his personal blog, first [tackling Marathon's blog post](http://mikedatl.typepad.com/mikedvirtualization/2008/09/marathon-and-vm.html) and then again [tackling comparisons posted on Marathon's web site](http://mikedatl.typepad.com/mikedvirtualization/2008/09/marathon-everru.html). Various others also weighed in, such as [Duncan at Yellow Bricks](http://www.yellow-bricks.com/2008/09/24/marathon-haft-vs-vmware-haft/) and TechTarget's [Server Virtualization Blog](http://servervirtualization.blogs.techtarget.com/2008/09/24/vmware-defends-its-upcoming-fault-tolerance-feature/).

I've spoken with the folks from Marathon a couple of different times about everRun and its functionality, so let me attempt to compare these three products---everRun VM, VMware HA, and VMware FT---with an eye toward understanding the differences between them.

* Marathon everRun VM provides two levels of protection: Level 1 and Level 2. Level 1 is basic failover, and is included in XenServer 5 as XenServer HA. In this regard, it is essentially the same as VMware HA in that it will restart VMs in the event of host failure. Both products calculate available capacity for failover but do not reserve those resources in advance; hence, neither of them can provide guaranteed failover. VMware HA seems to have an upper hand here because admission control can actually prevent users from powering on VMs if there are not enough resources to provide failover for that VM. From all information I have been able to obtain, everRun VM Level 1/XenServer HA lacks that ability, and it's possible therefore that users could power on more VMs than the resource pool could sustain in the event of hardware failure. Both products should be considered "best effort" as a result. Users wanting to make comparisons between Marathon everRun VM and VMware HA should constrain their comparison to everRun Level 1. Otherwise, the comparison is not a like-to-like comparison.

* Marathon everRun VM goes on to add Level 2 protection for component-level failure. It's true that this level of protection exceeds anything that can be provided via VMware HA today. With component-level protection, I/O to or from a failed storage device or a failed network device is transparently redirected to another host, where an identical VM environment has been established. Please note that the two VM environments are not both executing at the same time, but that resources on the secondary host are reserved and cannot be used by any other VMs. These resources include not only RAM, but also storage and networking. If there is a host failure, the VM is restarted on the secondary host. Because resources were pre-allocated, everRun VM is able to provide guaranteed restart on the secondary host. The functionality provided by everRun VM when configured for Level 2 protection exceeds any functionality that VMware HA has today.

* On the flip side, however, it's also fair to note that VMware has not needed to provide component-level fault tolerance because they've supported storage multipathing and NIC teaming for quite some time. It's my understanding that those features have only recently made it into the XenServer product line.

* VMware Fault Tolerance (FT) and everRun VM Level 3 are comparable. Both establish an identical VM on another host and keep that VM "mirrored" with the original VM. If there is a host failure, the "mirrored" VM will automatically take over right where the primary was when it failed. It appears that everRun VM might have an edge here because it again supports component-level failover, but given that neither product is available yet it's still a bit too early to be making calls on which product is "better".

* As for the "complexity" of one product versus the other, both have their own complexities. Marathon everRun VM requires a dedicated network link, called the "Availability Link", in order to provide the component-level protection. I would assume the Availability Link will be needed for everRun VM Level 3 as well. That corresponds directly to VMware FT's logging NIC. VMware HA does not require any special NICs or unique configurations; it's unclear if the same is true for everRun VM Level 1/XenServer HA protection. I'll have to call Marathon out on their knocks against setting up NIC teaming and storage multipathing; those tasks may be complicated in XenServer environments but are drop-dead simple in VMware ESX environments. The same goes for enabling VMware HA and VMware DRS.

As you can see, each product has its own set of strengths and weaknesses.

As a final note, as [SearchServerVirtualization.com stated](http://servervirtualization.blogs.techtarget.com/2008/09/24/vmware-defends-its-upcoming-fault-tolerance-feature/), comparisons between these two product sets are a bit irrelevent anyway: VMware's functionality works only with VMware ESX environments, and Marathon's functionality works only with XenServer. It's not like users have to choose between them in the same virtualization environment.

I welcome everyone's input and thoughts on this matter. Please contribute in the comments to this article.

[1]: {{< relref "2008-09-15-marathon-and-xenserver-ha.md" >}}
