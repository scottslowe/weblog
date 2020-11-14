---
author: slowe
categories: Information
comments: true
date: 2011-11-28T09:00:00Z
slug: technology-short-take-17
tags:
- Citrix
- EMC
- FCoE
- Fusion
- Linux
- macOS
- Microsoft
- Networking
- Storage
- vCloud
- Virtualization
- VMware
- vSphere
- VXLAN
- Windows
- Xen
title: 'Technology Short Take #17'
url: /2011/11/28/technology-short-take-17/
wordpress_id: 2474
---

Welcome to Technology Short Take #17, another of my irregularly-scheduled collections of various data center technology-related links, thoughts, and comments. Here's hoping you find something useful!

## Networking

* I think it was J Metz of Cisco that posted this to Twitter, but [this](http://www.networkworld.com/community/blog/confused-10gbe-optics-modules) is a good reference to the various 10 Gigabit Ethernet modules.

* I've spoken quite a bit about stretched clusters and their potential benefits. For an opposing view---especially regarding the use of stretched clusters as a disaster avoidance solution---check out [this article](http://blog.ioshints.info/2011/09/long-distance-vmotion-for-disaster.html). It's a nice counterpoint, especially from the perspective of the network.

* Anyone know anything about [sFlow](http://blog.sflow.com/)?

* Here's [a good post on VXLAN](http://www.borgcube.com/blogs/2011/11/vxlan-primer-part-1/) that has some useful information. I'd just like to point out that VXLAN is really only intended to address Layer 2 communications "within" a vApp or a collection of VMs (perhaps a single organization's VMs), and doesn't do anything to address Layer 3 routing/accessibility for clients (or "consumers") attempting to connect to those systems. For that, you'll still need---at least today---technologies like OTV, LISP, and others.

* A quick thought that I'm still exploring: what's the impact of OpenFlow on technologies like VXLAN, NVGRE, and others? Does SDN eliminate the need for these technologies? I'd be curious to hear your thoughts.

## Servers/Operating Systems

* If you've adopted Mac OS X Lion 10.7, you might have noticed some problems connecting to older servers/NAS devices running AFP (AppleTalk Filing Protocol). [This Apple KB article](http://support.apple.com/kb/HT4700) describes a fix. Although I'm running Snow Leopard now, I was running Lion on a new MacBook Pro and I can attest that this fix _does_ work.

* This Microsoft KB article describes [how to extend the Windows Server 2008 evaluation period](http://support.microsoft.com/kb/948472). I've found this useful for Windows Server 2008 instances in the lab that I need for longer 60 days but that I don't necessarily want to activate (because they are transient).

## Storage

* Jason Boche blogged about [a way to remove stubborn hosts from Unisphere](http://www.boche.net/blog/index.php/2011/11/14/unable-to-remove-stubborn-hosts-from-unisphere-and-the-solution/). I've personally never seen this problem, but it's nice to know how to address it should it occur.

* Who would've thought that an HDD could serve as a cache for an SSD? Shouldn't it be the other way around? Normally, that would probably be the case, but [as described here](http://thessdguy.com/an-hdd-cache-for-an-ssd/) there are certain instances and ways in which using an HDD as a cache for an SSD can improve performance.

* Scott Drummonds wraps up his 3 part series on flash storage in part 3, which contains [information on sizing flash storage](http://vpivot.com/2011/11/17/the-flash-storage-revolution-part-iii/). If you haven't been reading this series, I'd recommend giving it a look.

* Scott also weighs in on the [flash as SSD vs. flash on PCIe discussion](http://vpivot.com/2011/11/22/flash-or-ssd-or-why-interfaces-matter/). I'd have to agree that interfaces are important, and the ability of the industry to successfully leverage flash on the PCIe bus is (today) fairly limited.

* Henri updated his VNXe blog series with [a new post on EFD and RR performance](http://henriwithani.wordpress.com/2011/11/21/vnxe-3300-performance-follow-up/). No real surprises here, although I do have one question for Henri: is that your car in the blog header?

## Virtualization

* Interested in setting up host-only networking on VMware Fusion 4? Here's [a quick guide](http://mergy.org/2011/09/host-only-networking-setup-with-vmware-fusion-4/).

* Kenneth Bell offers up [some quick guidelines](http://blogs.citrix.com/2011/02/17/mcs-or-pvs-what-should-i-be-using/) on when to deploy MCS versus PVS in a XenDesktop environment. MCS vs. PVS is a topic of some discussion on the vSpecialist mailing list as they have very different IOPs requirements and I/O profiles.

* Speaking of VDI, Andre Leibovici has two articles that I wanted to point out. First, Andre does a deep dive on [Video RAM in VMware View 5 with 3D](http://myvirtualcloud.net/?p=2238); this has tons of good information that is useful for a VDI architect. (The note about the extra .VSWP overhead, for example, is priceless.) Andre also has a good piece on [VDI and Microsoft Outlook](http://myvirtualcloud.net/?p=1664) that's worth reading, laying out the various options for Outlook-related storage. If you want to be good at VDI, Andre is definitely a great resource to follow.

* Running Linux in your VMware vSphere environment? If you haven't already, check out Bob Plankers' [Linux Virtual Machine Tuning Guide](http://lonesysadmin.net/linux-virtual-machine-tuning-guide/) for some useful tips on tuning Linux in a VM.

* Seen [this page](http://kb.vmware.com/selfservice/google/searchpage.jsp)?

* You've probably already heard about Nick Weaver's new "Uber" tool, a new VM alignment tool called UBERAlign. This tool is designed to address VM alignment, a problem with how guest file systems are formatted within a VMDK. For more information, see Nick's announcement [here](http://nickapedia.com/2011/11/03/straighten-up-with-a-new-uber-tool-presenting-uberalign/).

* Don't disable DRS when you're using vCloud Director. It's as simple as that. (If you want to know why, read [Chris Colotti's post](http://www.chriscolotti.us/vmware/vcloud/gotcha-disabling-vmware-drs-with-vcloud-director/).)

* Here's a couple of great diagrams by Hany Michael on [vCloud Director management pods](http://www.hypervizor.com/2011/11/double-diagram-vcloud-director-management-pod-in-the-public-private-clouds/) (both public cloud and private cloud management).

* People automatically assume that "virtualization" means consolidating multiple workloads onto a single physical server. However, virtualization is really just a layer of abstraction, and that layer of abstraction can be used in a variety of ways. I [spoke about this](http://www.slideshare.net/lowescott/201004egroupkeynote) in early 2010. [This article](http://bradhedlund.com/2011/03/16/inverse-virtualization-for-internet-scale-applications/) (written back in March of 2011) by Brad Hedlund picks up on that theme to show another way that virtualization---or, as he calls it, "inverse virtualization"--can be applied to today's data centers and today's applications.

* My discussion on [the end of the infrastructure engineer][1] generated some conversations, which is good. One of the responses was by Aaron Sweemer in which he discusses [the new (but not new) "data layer"](http://www.virtualinsanity.com/index.php/2011/10/11/the-layer-between-the-layers/) and expresses a need for infrastructure engineers to be aware of this data layer. I'd agree with a general need for all infrastructure engineers to be aware of the layers above them in the stack; I'm just not convinced that we all need to become application developers.

* Here's a great post by William Lam on [the missing piece to creating your own vSEL cloud](http://www.virtuallyghetto.com/2011/10/missing-piece-in-creating-your-own.html). I'll tell you, William blogs some of the coolest stuffI wish I could dig in as deep as he does in some of this stuff.

* Here's a nice look at the use of PowerCLI to help with [the automation of DRS rules](http://www.van-lieshout.com/2011/06/drs-rules/).

* One of my projects for the upcoming year is becoming more knowledgeable and conversant with the open source Xen hypervisor and Citrix XenServer. I think that [the XenServer Design Handbook](https://community.citrix.com/kits/#/kit/3125008) is going to be a useful resource for that project.

* Interested in more information on deploying Oracle databases on vSphere? Michael Webster, aka [@vcdxnz001](http://twitter.com/vcdxnz001) on Twitter, has a lengthy article with [lots of information regarding Oracle on vSphere](http://longwhiteclouds.com/2011/11/22/deploying-enterprise-oracle-databases-on-vsphere/).

* [This VMware KB article](http://kb.vmware.com/kb/2004519) describes how to enable centralized logging for vCloud Director cells. This is particularly important for HA environments, where VMware's recommended HA strategy involves the use of multiple vCD cells.

I guess I should wrap it up here, before this post gets any longer. Thanks for reading this far, and feel free to speak up in the comments!

[1]: {{< relref "2011-09-14-the-end-of-the-infrastructure-engineer.md" >}}
