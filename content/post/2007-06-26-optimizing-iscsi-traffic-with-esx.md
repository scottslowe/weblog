---
author: slowe
categories: Musing
comments: true
date: 2007-06-26T23:26:19Z
slug: optimizing-iscsi-traffic-with-esx
tags:
- ESX
- iSCSI
- Networking
- Virtualization
- VMware
- Storage
title: Optimizing iSCSI Traffic with ESX
url: /2007/06/26/optimizing-iscsi-traffic-with-esx/
wordpress_id: 479
---

A response in [this VMTN forums thread](http://www.vmware.com/community/message.jspa?messageID=604130) by Paul Lalonde got me to thinking about iSCSI traffic, network designs, and the software initiator provided with [ESX Server](http://www.vmware.com/products/vi/esx/). The statement was this (in response to questions about how ESX uses network links to communicate with an iSCSI storage array):

>In a single server environment, 802.3ad would only offer failover. A single ESX box would only ever use one network path for iSCSI traffic.

In my lab, I've setup a [Network Appliance](http://www.netapp.com/) storage system with a virtual interface (a "VIF" in NetApp parlance), which is essentially 802.3ad link aggregation (in fact, newer versions of Data ONTAP can use LACP to build link aggregates). On the ESX side, I've created Gigabit EtherChannels and configured the vSwitches to use IP hash load balancing, with the thought that this would help improve network utilization. But after reading that statement (and following up on some other related threads; see [these del.icio.us bookmarks](http://del.icio.us/slowe/VMware+iSCSI)), I started wondering if there was a better way to architect the network for iSCSI traffic from ESX Server.

I have some ideas, and have already started working on implementing and testing those ideas in the lab. As soon as I have more information, I'll share it here. In the meantime, any iSCSI gurus out there care to share their network designs for optimizing ESX-iSCSI traffic?
