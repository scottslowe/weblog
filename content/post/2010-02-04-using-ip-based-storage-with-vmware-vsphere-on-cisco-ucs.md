---
author: slowe
categories: Explanation
comments: true
date: 2010-02-04T09:27:28Z
slug: using-ip-based-storage-with-vmware-vsphere-on-cisco-ucs
tags:
- Cisco
- iSCSI
- NFS
- Storage
- UCS
- Vblock
- Virtualization
- VMware
- vSphere
title: Using IP-Based Storage with VMware vSphere on Cisco UCS
url: /2010/02/04/using-ip-based-storage-with-vmware-vsphere-on-cisco-ucs/
wordpress_id: 1815
---

I had a reader contact me with a couple of questions, one of which I felt warranted a blog post. Paraphrased, the question was this: How do I make IP-based storage work with VMware vSphere on Cisco UCS?

At first glance, you might look at this question and scoff. Remember though, that Cisco UCS does---at this time---have a few limitations that make this a bit more complicated than at first glance. Specifically:

* Recall that the UCS 6100XP fabric interconnects only have two kinds of ports: server ports and uplink ports.

* Server ports are _southbound_, meaning they can only connect to the I/O Modules running in the back of the blade chassis.

* Uplink ports are _northbound_, meaning they can only connect to an upstream switch. They **cannot** be used to connect directly to another end host or directly to storage.

With this in mind, then, how does one connect IP-based storage to a Cisco UCS? In these scenarios, you must have another set of Ethernet switches between the 6100XP fabric interconnects and the target storage array. Further, since the 6100XP fabric interconnects require 10GbE uplinks and do not---at this time---offer any 1GbE uplink functionality, you need to have the right switches between the 6100XP fabric interconnects and the target storage array.

Naturally, the Nexus 5000 fits the bill quite nicely. You can use a pair of Nexus 5000 switches between the UCS 6100XP interconnects and the storage array. Dual-connect the 6100XP interconnects to the Nexus 5000 switches for redundancy and active-active data connections, and dual-connect the target storage array to the Nexus 5000 switches for redundancy and (depending upon the array) active-active data connections. It would look something like this:

![IP storage with Cisco UCS](/public/img/ipstorage-with-ucs.jpg)

From the VMware side of the house, since you're using 10GbE end-to-end, it's very unlikely that you'll need to worry about bandwidth; that eliminates any concerns over multiple VMkernel ports on multiple subnets or using multiple NFS targets so as to be able to use link aggregation. (I'm not entirely sure you could use link aggregation with the 6100XP interconnects anyway. Anyone?) However, since you are talking Cisco UCS you'll have only two 10GbE connections (unless you're using the full width blade, which is unlikely). This means you'll need to pay careful attention to the VMware vSwitch (or dvSwitch, or Nexus 1000V) configuration. In general, the recommendation in this sort of configuration is to place Service Console, VMotion, and IP-based storage traffic on one 10GbE uplink, place virtual machine traffic on the second 10GbE uplink, and use whatever mechanisms are available to preferentially specify which uplink should be used in the course of normal operation. This provides redundancy in the uplinks but some level of separation of traffic.

One quick side note: although I'm talking IP-based storage here, block-based storage fans need to remember that Cisco UCS does not---at this time---support northbound FCoE. That means that although you have FCoE support southbound, and FCoE support in the Nexus 5000, and possibly FCoE support in your storage arrays, you still can't do end-to-end FCoE with Cisco UCS.

For those readers who are very familiar with Cisco UCS and Nexus, this will seem like a pretty simplistic post. However, we need to keep in mind that there are lots of readers out there who have not had the same level of exposure. Hopefully, this will help provide some guidance and food for thought.

(Of course, one _could_ just buy a Vblock and not have to worry about putting all the pieces together...hey, can't blame me for trying, right?)

Clarifications, questions, or suggestions are welcome in the comments below. Thanks!
