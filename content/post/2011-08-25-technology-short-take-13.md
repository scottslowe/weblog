---
author: slowe
categories: Information
comments: true
date: 2011-08-25T10:53:46Z
slug: technology-short-take-13
tags:
- Networking
- Storage
- Virtualization
- Personal
- VMware
- vSphere
- OTV
- UCS
- FibreChannel
- FCoE
- Security
- HyperV
title: 'Technology Short Take #13'
url: /2011/08/25/technology-short-take-13/
wordpress_id: 2375
---

Welcome to Technology Short Take #13. It's been a while since my last Technology Short Take, and much has happened ([vSphere 5 was announced][1], [HP discontinued the TouchPad and webOS](http://www.engadget.com/2011/08/18/hp-will-discontinue-operations-for-webos-devices/), and [I announced my move to Denver][2]). Here are a few data center technology-related links that stood out to me over the last few weeks. I hope you find something useful!

## Networking

* Here's a good post by Jeremy Waldrop on [Cisco Nexus 5000/7000 FEX (fabric extender) topologies](http://jeremywaldrop.wordpress.com/2011/06/30/cisco-nexus-50007000-fex-topologies/). I also saw this reference by Jeremy on [Nexus 7000 I/O modules](http://jeremywaldrop.wordpress.com/2011/06/30/cisco-nexus-7000-io-module-cheat-sheet/). Both of these are useful references for folks like me who don't use this sort of information every day. 

* This post by Ivan Pepelnjak on [data center fabric architectures](http://www.ioshints.info/Data_Center_Fabric_Architectures) is a good overview of the various approaches companies are taking to build so-called "data center fabrics". I have to say, I am regularly impressed by Ivan's work. He has become one of my top networking-focused resources. Great job, Ivan!

* Pete Welcher of Chesapeake Netcraftsmen posted [an article on Cisco Overlay Transport Virtualization (OTV)](http://www.netcraftsmen.net/resources/blogs/cisco-overlay-transport-virtualization-otv.html) that has some good information in it.

## Servers

* I'm not sure if I've previously linked to part 1 of this series, but I just recently took a look at Lane Roush's [part 2 on automating Cisco UCS server provisioning](http://www.laneroush.com/automated-cisco-ucs-server-provisioning-part-2-the-meat/). Cool stuff.

* Going to be connecting your Cisco C Series servers into UCS? [This document](http://www.cisco.com/en/US/docs/unified_computing/ucs/c/hw/C250M1/install/ucsm-integration.html) provides more information on how it's done.

* Here's a good collection of [I/O diagrams for various blade server solutions](http://bladesmadesimple.com/2011/07/blade-chassis-io-diagrams/) from different vendors. Very useful material, in my opinion.

## Storage

* Sometimes it seems like people don't fully understand the level of compatibility between FC and FCoE. [This post](http://virtualeverything.wordpress.com/2011/05/30/fcoes-impact-on-a-storage-administrator/) by Vijay Swami provides a good review of the impact of FCoE on the average storage administrator---in most cases, no impact at all.

* Erik Smith has a [good review of the FC/FCoE connectivity options](http://brasstacksblog.typepad.com/brass-tacks/2011/06/fcfcoe-connectivity-options-as-of-6272011.html) for EMC storage platforms in this post. It's worth taking a quick look if you are interested in more detail on what sort of FCoE connectivity options are supported.

* On the flip side, here's information from Cisco on [storage interoperability with UCS](http://www.cisco.com/en/US/docs/switches/datacenter/mds9000/interoperability/matrix/Matrix8.html#wp323852).

* This two-part series by Itzik Reich on Citrix XenDesktop 5 with EMC VNX and FAST Cache is a good read, especially if you are considering XenDesktop for your VDI environment. Part 1 is [here](http://itzikr.wordpress.com/2011/06/08/citrix-xendesktopp-5-on-emc-vnx-match-made-in-heaven-part1/), and part 2 is [here](http://itzikr.wordpress.com/2011/06/09/citrix-xendesktop-5-on-emc-vnx-match-made-in-heaven-part2/).

* Here's another look at the [impact of FAST Cache on VDI workloads](http://sudrsn.wordpress.com/2011/06/15/fast-cache-and-vdi/).

## Virtualization

* This post on [application consolidation](http://vpivot.com/2010/01/15/virtual-storage-design-application-consolidation/) by Scott Drummonds is an old post (from January 2010), but it's still a good one. In this post, Scott shares data from tests assessing the impact of consolidating sequential access workloads with random access workloads on the same datastore. The results of the tests underscore the importance of knowing the I/O profile of your workloads.

* Here's [a workaround](http://blog.vmpros.nl/2011/05/31/vmware-the-mac-address-entered-is-not-in-the-valid-range/) for using static MAC addresses that fall outside the normal range that vSphere allows.

* Cormac Hogan has a great series of blog posts on new storage features in vSphere 5. [Part 1](http://blogs.vmware.com/vsphere/2011/07/new-vsphere-50-storage-features-part-1-vmfs-5.html) covers VMFS-5, [part 2](http://blogs.vmware.com/vsphere/2011/07/new-vsphere-50-storage-features-part-2-storage-vmotion.html) discusses Storage vMotion, [part 3](http://blogs.vmware.com/vsphere/2011/07/new-enhanced-vsphere-50-storage-features-part-3-vaai.html) covers VAAI, and so forth. This is definitely worth reading. Of course, there is [this vSphere 5 book][3] slated to come out in early October that will discuss all these features, too

* I have a whole collection of posts by William Lam; he's been on a roll: a summary of the [updates to esxcli](http://www.virtuallyghetto.com/2011/07/major-enhancements-in-esxcli-for.html), information on [enabling support for nested 64-bit and Hyper-V VMs](http://www.virtuallyghetto.com/2011/07/how-to-enable-support-for-nested-64bit.html), and information on [enabling nested Fault Tolerance](http://www.virtuallyghetto.com/2011/07/how-to-enable-nested-vft-virtual-fault.html).

## Security

* I came across this article on [how to protect Hyper-V hosts against ARP spoofing](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/08/08/protecting-against-arp-spoofing-in-hyper-v.aspx). Unless I'm mistaken---and that's certainly very possible---I don't think that the vSwitch/distributed vSwitch security policies around MAC address changes and forged transmits protect against ARP spoofing. Anyone have any additional information on how a VMware vSphere shop would protect against ARP spoofing? Is it even necessary?

* Harley Stagner has [a pretty good write-up of the Nexus 1000V Virtual Security Gateway (VSG)](http://www.theblinkylight.com/virtualization-blog/end-to-end-virtual-security-with-the-cisco-nexus-vsg/). The VSG---and the Nexus 1000V, for that matter---are products in which I'm very interested, but just haven't had the time to really spend with them. Perhaps in the future!

It's time to wrap up now, but thanks for reading. Feel free to share any other interesting or useful links you've found, or any thoughts on the links I included here, in the comments.

[1]: {{< relref "2011-07-12-vmwares-launch-event.md" >}}
[2]: {{< relref "2011-07-19-time-for-a-fresh-start.md" >}}
[3]: {{< relref "2011-07-13-announcing-mastering-vmware-vsphere-5.md" >}}
