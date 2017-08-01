---
author: slowe
categories: Rant
comments: true
date: 2010-06-13T19:18:18Z
slug: the-vmotion-reality
tags:
- Networking
- Virtualization
- VLAN
- VMotion
- VMware
title: The vMotion Reality
url: /2010/06/13/the-vmotion-reality/
wordpress_id: 1974
---

The quite popular GigaOm site recently published an article titled ["The VMotion Myth"](http://gigaom.com/2010/06/13/the-vmotion-myth/), in which the author, Alex Benik, debunks the myth of vMotion and the live migration of virtual machines (VMs).

In his article, Benik states that the ability to dynamically move workloads around inside a single data center or between two data centers is, in his words, "far from an operational reality today". While I'll grant you that inter-data center vMotion isn't the norm, vMotion within a data center is very much an operational reality of today. I believe that Benik's article is based on some incorrect information and incomplete viewpoints, and I'd like to clear things up a bit.

To quote from Benik's article:

>Currently, moving a VM from a one physical machine to another has two important constraints. First, both machines must share the same storage back end, typically a Fibre Channel/iSCSI SAN or network-attached storage. Second, the physical machines must reside in the same VLAN or subnet. This means that inside a single data center, one can only move a VM across a relatively small number of physical machines. Not exactly what the marketing guys would have you believe.

Benik is correct in that shared storage is required in order to use vMotion. The use of shared storage in VMware environments is quite common for this very reason. Note also that shared storage is required in order to leverage other VMware-specific functionality such as VMware High Availability (HA), VMware Distributed Resource Scheduler (DRS), and VMware Fault Tolerance (FT).

On the second point---regarding VLAN requirements---Benik is not entirely correct. You see, by their very nature VMware ESX/ESXi hosts tend to have multiple network interfaces. The majority of these network interfaces, in particular those that carry the traffic to and from VMs, are what are called _trunk ports_. Trunk ports are special network interfaces that carry multiple VLANs at the same time. Because these network interfaces carry multiple VLANs simultaneously, VMware ESX/ESXi hosts can support VMs on multiple VLANs simultaneously. (For a more exhaustive discussion of VLANs with VMware ESX/ESXi, see this [collection of networking articles][1] I've written.)

But that's only part of the picture. It is true that there is one specific type of interface on a VMware ESX/ESXi host that must reside in the same VLAN on all the hosts between which live migration is required. This port is a VMkernel interface that has been enabled for vMotion. Without sharing connectivity between VMkernel interfaces on the same VLAN, vMotion cannot take place. I suppose it is upon this that Benik bases his statement.

However, the requirement that a group of physical systems share a common VLAN on a single interface is hardly a limitation. First, let's assume that you are using an 8:1 consolidation ratio; this is an **extremely conservative** ratio considering that most servers have 8 cores (thus resulting in a 1:1 VM-to-core ratio). Still, assuming an 8:1 consolidation ratio and a maximum of 253 VMware ESX/ESXi hosts on a single Class C IP subnet, that's enough physical systems to drive over 2,000 VMs (2,024 VMs, to be precise). And remember that these 2,024 VMs can be distributed across any number of VLANs themselves because the network interfaces that carry their traffic are capable of supporting multiple VLANs simultaneously. This means that the networking group can continue to use VLANs for breaking up broadcast domains and segmenting traffic, just like they do in the physical world.

Bump the consolidation ratio up to 16:1 (still only 2:1 VM-to-core ratio on an 8 core server) and the number of VMs that can be supported with a single VLAN for vMotion is over 4,000. How is this a limitation again? Somehow I think that we'd need to pay more attention to CPU, RAM, and storage usage before being concerned about the fact that vMotion requires Layer 2 connectivity between hosts. And this isn't even taking into consideration that organizations might want multiple vMotion domains!

Clearly, the "limitation" that a single interface on each physical host share a common VLAN isn't a limitation at all. Yes, it is a design consideration to keep in mind. But I would hardly consider it a limitation and I definitely don't think that it's preventing customers from using vMotion in their data centers today. No, Mr. Benik, there's no vMotion myth---only vMotion reality.

[1]: {{< relref "2008-12-19-vmware-esx-networking-articles.md" >}}
