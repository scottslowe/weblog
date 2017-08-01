---
author: slowe
categories: Information
comments: true
date: 2008-05-31T18:33:31Z
slug: virtualization-short-take-9
tags:
- Citrix
- ESX
- HyperV
- VDI
- Virtualization
- VMware
- VMwareHA
- Xen
title: 'Virtualization Short Take #9'
url: /2008/05/31/virtualization-short-take-9/
wordpress_id: 720
---

Here are some virtualization links I found interesting over the last few days:

* Duncan [points out a VMTN thread regarding](http://www.yellow-bricks.com/2008/05/20/vc-25-ha-constraints/) VMware HA behaviors in "heterogeneous" clusters, i.e., clusters that include 1/2 vCPU VMs as well as 4 vCPU VMs. The recommendation is to move these 4 vCPU VMs into their own cluster to help address this issue. This is similar to the discussions I had here about [VMware HA failover capacity calculations][1], and it goes to further reinforce the fact that planning is needed to fully take advantage of VMware HA's functionality. It's not quite "fire and forget" just yet, folks.

* Via a number of different sites, I learned that VMware has released version 2.1 of VDM. More information is available in the [Release Notes](http://www.vmware.com/support/vdm20/doc/releasenotes_vdm20.html). Of key interest to me is the defined process for bulk importing individual desktops, which will make it easier for organizations that already have a number of desktop images to bring those VMs into VDM.

* On the [VMware performance blog](http://blogs.vmware.com/performance/), they're discussing [achieving 100K IOPS with a single ESX server](http://blogs.vmware.com/performance/2008/05/100000-io-opera.html). While some of the readers are taking VMware to task for what they call an "unrealistic" test, I do have to agree with commenter Chad who points out that this exercise wasn't intended to create a "best practices" configuration. The point was simply to see just how high the IOPS could go---nothing more, nothing less, just a test to see how high they could take the number. Yes, I think we'd all agree that using a cluster without 1:1 VM-to-VMFS mappings would be a realistic test, and personally I'd love to see the results of a test like that as well. Even so, it's still handy to see that the I/O subsystem of ESX is more than capable of handling even the most demanding workloads.

* It becomes more obvious every day that I really need to take some time to learn PowerShell. With Microsoft embedding PowerShell in all their products and VMware embracing it via the VI Toolkit, it's becoming ubiquitous. Now VMware is even [showing off a series of videos](http://blogs.vmware.com/vipowershell/2008/05/ever-wonder-wha.html) about the VI Toolkit and its functionality. Ugh..I need more hours in a day to keep up with all this stuff.

* Paul Shannon of VM-Aware [points out](http://www.vm-aware.com/2008/05/21/vmware-ms-support-from-oems/) this [VMware page describing support for Microsoft products](http://www.vmware.com/support/policies/ms_support_statement.html), both from Microsoft as well as from various OEMs. Useful information to have, especially when you need to reassure a concerned customer about their support options. Personally, I think it's just poor business (or poor ethics, take your pick) for Microsoft to be giving customers a hard time about virtualization support while developing their own virtualization product. Come on, we all know that the day Hyper-V goes RTM, Microsoft will start offering full product support for virtualized instances---well, virtualized instances running on Hyper-V, anyway. Am I wrong?

* [Via Ruben at Brian Madden's site](http://www.brianmadden.com/blog/RubenSpruijt/Virtual-Desktop-Infrastructure-VDI-Connection-broker-comparison) (and thanks to an e-mail from Patrick Rouse himself), I learned about [this VDI broker comparison](http://blogs.inside.quest.com/provision/2008/05/24/virtual-desktop-infrastructure-vdi-connection-broker-comparison/) created by Patrick Rouse of Quest/Provision Networks. Right now, it only compares VDM, XenDesktop, and Provision Networks Virtual Access Suite (VAS), but they are open to including additional brokers if enough requests come in.

* Brian Madden delves into an [extended discussion](http://www.brianmadden.com/blog/BrianMadden/Citrixs-ICA-problem-while-not-as-bad-as-VMwares-RDP-problem-is-still-a-problem-for-widespread-VDI-adoption) of the key problem with VDI solutions: the display protocol. He posits that Citrix is in better shape than VMware because of the ICA protocol, but both suffer from the same problem in that "neither ICA nor RDP can remote all applications." It's a good read.

* This may be a bit dated now, but here's some information on an [unattended installation](http://blogs.technet.com/virtualization/archive/2008/05/07/unattended-installation-of-windows-server-2008-with-hyper-v-rc0.aspx) of Windows Server 2008 with Hyper-V.

* InformationWeek recently published an article [describing Hyper-V's "advanced virtualization features."](http://www.infoweek.ca/index.php?page=shop.product_details&category_id=116&flypage=shop.flypage&product_id=2144&option=com_virtuemart&vmcchk=1) The two things that are really touted by the article are I/O optimization via driver enlightenments, and support for failover clustering at the host level. Driver enlightenments, unless I am mistaken, are equivalent to Xen's paravirtualized drivers, VMware's VMware Tools, and Virtual Iron's VI Tools; they all accomplish the same thing. I'm not sure how having the same feature as all the other competitors makes it "advanced". It sounds like a standard feature if you ask me. Host clustering support is nice, but not that different from VMware HA; I believe Citrix is due to introduce a similar feature for XenServer soon as well. (It's my understanding that Marathon Technologies plans to build their "Continuous Availability"-like product to extend this new XenServer HA functionality.) Not that I'm knocking Hyper-V or these features that are slated to be included in Hyper-V; you just can't call them "advanced" if pretty much every other virtualization solution on the market also has the same features.

Well, that's it for now. If you have links that you'd like to share with me or other readers, feel free to add them in the comments below or put them in my del.icio.us inbox. Thanks for reading!

[1]: {{< relref "2007-12-04-calculating-vmware-ha-failover-capacity.md" >}}
