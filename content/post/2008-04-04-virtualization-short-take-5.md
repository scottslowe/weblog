---
author: slowe
categories: Information
comments: true
date: 2008-04-04T19:40:01Z
slug: virtualization-short-take-5
tags:
- ESX
- Microsoft
- NFS
- Security
- Virtualization
- VMware
- VMwareHA
title: 'Virtualization Short Take #5'
url: /2008/04/04/virtualization-short-take-5/
wordpress_id: 679
---

Here's some thoughts on a variety of links that passed by me over the last couple of weeks. (Yes, I've been a bit lax in getting another Short Take published. Sorry.)

* Colleague Colin McNamara has written a good article about some of the [challenges in integrating](http://www.colinmcnamara.com/2008/03/15/challenges-integrating-vmware-into-cisco-networks) VMware into a Cisco network. He highlights something I've been saying for a while: a VMware implementation is more than just server virtualization; it affects servers, storage, networking, and security, and a good implementation requires addressing all of these areas as well as [addressing things like staff organization and change management](http://searchservervirtualization.techtarget.com/tip/0,289483,sid94_gci1303371,00.html).

* Christofer Hoff started a good conversation about the [performance implications of virtual security initiatives](http://rationalsecurity.typepad.com/blog/2008/03/performance-imp.html). It's something many people are probably overlooking. After all, have you stopped to consider the additional processing power that running security products either inside the VMs, or at the hypervisor level, or both, will take from your CPU pool? I have a feeling that those high server consolidation ratios may not be so applicable when you factor in the security overhead.

* Per [Duncan](http://www.yellow-bricks.com/2008/04/02/support-for-microsoft-cluster-server-mscs-in-35-update/) and [Thomas](http://blogs.sun.com/thomasweyell_en/entry/support_for_microsoft_cluster_in), ESX Server 3.5 Update 1 will provide support for Microsoft Cluster Server (MSCS). Duncan also [broke the news](http://www.yellow-bricks.com/2008/03/31/new-isosbuilds-coming-up-for-3i-35-virtualcenter/) about the incorrect links for the update ESX ISOs.

* Massimo has initiated a discussion, [picked up](http://blogs.vmware.com/vmtn/2008/03/the-philosophy.html) by the VMTN Blog, about the [current state of high availability](http://it20.info/blogs/main/archive/2008/03/26/102.aspx). I'm not a clustering expert, although I've setup my share of Microsoft clusters for SQL Server and Exchange Server. In my simplistic view, MSCS and VMware HA don't really solve the same problem; MSCS is stateful (or mostly so), and VMware HA is stateless. Would you rather have a reasonably stateful failover for your Exchange Server, or would you rather have it rebooted? Stateful failover is not something that can be easily achieved in the virtual world right now, unless you bring MSCS into the virtual world; that, in turn, creates its own set of challenges. Continuous Availability, as [demonstrated at VMworld 2007][1], will bring stateful failover to the virtual infrastructure.

* In the comments for the VMTN post about clustering vs. HA, reader "Matt" questions the use of NFS for VMware. In [his linked article](http://universitytechnology.blogspot.com/2008/03/vmware-over-nfs-vs-fiberchannel-fc.html), he asks for a good white paper on why NFS instead of Fibre Channel. Well, I can't provide a good white paper, but I can provide a couple useful articles, like [this one][2] or [this one][3], to get started.

* David Marshall at VMblog has published parts [one](http://vmblog.com/archive/2008/03/28/best-practices-for-securing-virtual-networks-part-one-of-three.aspx), [two](http://vmblog.com/archive/2008/03/27/best-practices-for-securing-virtual-networks-part-two-of-three.aspx), and [three](http://vmblog.com/archive/2008/03/28/best-practices-for-securing-virtual-networks-part-three-of-three.aspx) of a three-part series on best practices for securing virtual networks. I haven't had the opportunity to finish reading all three articles yet, but it looks like it's avoided becoming an advertisement for Reflex Security.

Well, that wraps it up this time. Thanks for reading!

[1]: {{< relref "2007-09-13-vmworld-2007-day-3-keynote-liveblog.md" >}}
[2]: {{< relref "2007-09-21-nfs-for-vmware-storage.md" >}}
[3]: {{< relref "2008-01-14-proving-vmware-over-nfs.md" >}}
