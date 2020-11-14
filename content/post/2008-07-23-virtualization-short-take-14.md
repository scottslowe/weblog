---
author: slowe
categories: Information
comments: true
date: 2008-07-23T15:01:40Z
slug: virtualization-short-take-14
tags:
- Citrix
- Fusion
- HyperV
- Security
- Storage
- Virtualization
- VMFS
- VMware
- VMwareHA
- Xen
title: 'Virtualization Short Take #14'
url: /2008/07/23/virtualization-short-take-14/
wordpress_id: 768
---

Welcome to another installation of Virtualization Short Takes!

* For you Quicksilver lovers out there that also run VMware Fusion, here's a handy trick to allow you to [launch Windows apps to run under Fusion via Quicksilver](http://shayne.powerlot.net/2008/07/23/vmware-fusion-apps/).

* Duncan of Yellow Bricks points out [this VMware Communities Forums thread](http://communities.vmware.com/thread/156778?tstart=0) discussing how to determine which host has a lock on a LUN. This thread also makes brief mention of the new VMFS version, version 3.31, that was released with ESX 3.5, which does a better job of handling SCSI reservations than previous versions. Good find, Duncan!

* Speaking of the new VMFS version, a summary of the information shared in the VMware Communities Forums threads can be found [here](http://blog.standorsett.net/?p=11).

* While we are on a bit of a storage kick, VMware has launched a new [VMware Storage blog](http://blogs.vmware.com/storage/), and one of the [early posts deals with VMFS](http://blogs.vmware.com/storage/2008/07/vmwares-proprie.html). The post primarily attacks the notion of VMFS as a "proprietary" file system (which it is) by describing the advantages that VMFS provides. I'm hoping that the new storage blog will get more technical than marketing in the future, but the information is useful nevertheless.

* [This link](http://community.citrix.com/blogs/citrite/ruiguoy/2007/07/18/Use+VMWARE+workstation+team+feature+to+conduct+ICA+performance+testing) falls more into the "ironic" category than anything else. Do you suppose he got into trouble with Citrix for blogging about how to use a competitor's product to test ICA performance?

* John Howard gives us an [in-depth look at Hyper-V's handling of virtual NICs](http://blogs.technet.com/jhoward/archive/2008/07/22/hyper-v-why-is-networking-reset-in-my-vm-when-i-copy-a-vhd.aspx) in this article. This is particularly important for users who are interested in cloning VMs hosted on Hyper-V; I would assume that SCVMM 2008 will handle this correctly.

* [This news](http://vmblog.com/archive/2008/07/09/leostream-named-a-crn-emerging-tech-vendor.aspx) emerged several weeks ago via VMblog.com. It's good to see Leostream getting some recognition; their broker is actually quite good in many respects.

* Sven over at Virtualfuture.info recently blogged about [XenServer's HA functionality](http://virtualfuture.info/2008/07/marathon-xenserver-faulttoleranc/) and how Marathon's EverRun products play into that functionality. I actually had a conference call with the folks from Marathon several months ago about EverRun, but never got around to blogging about it. I do like the fact that you can control HA functionality on a per-VM basis, whereas VMware HA is applied to all VMs. (Well, I suppose you could disable HA for the VMs that you don't want restarted, but it's not quite the same.) I do agree with both Sven and PeterB's comments regarding "Continuous Availability"; the sooner that VMware gets this functionality out the door, the more of a leg up they'll have on the competition.

* As has been reported elsewhere as well, Reflex Security has released the Reflex Virtual Security Center (VSC). The full press release is [here](http://www.reflexsecurity.com/news/20080723_VSCLaunch.php). Based on what I've read thus far, it appears that the idea behind the VSC is to combine the information from multiple instances of their Virtual Security Appliance (VSA) so that users get the "full view" of what's occurring across the virtual infrastructure. In this regard, it is remarkably similar to Altor Networks' [Virtual Network Security Analyzer (VNSA)](http://altornetworks.com/products/vnsa/), which is also designed to provide visibility across the entire virtual infrastructure.

As always, feel free to share other interesting links and news in the comments below. Thank you!
