---
author: slowe
categories: Explanation
comments: true
date: 2010-07-12T09:00:00Z
slug: vmotion-practicality
tags:
- Cisco
- Networking
- Virtualization
- VLAN
- VMotion
- VMware
title: vMotion Practicality
url: /2010/07/12/vmotion-practicality/
wordpress_id: 1995
---

About a month ago I posted an article titled [The vMotion Reality][1]. In that article, I challenged the assertion that vMotion was a myth, not widely implemented in data centers today due to networking complexity and networking limitations.

In a recent comment to that article, a reader again mentions networking limitations---in this instance, the realistic limits of a large Layer 2 broadcast domain---as a gating factor for vMotion and limiting its "practical use" in today's data centers. Based on the original article's assertions and the arguments found in this comment, I'm beginning to believe that a lot of people have a basic misunderstanding of the Layer 2 adjacency requirements for vMotion and what that _really_ means. In this post, I'm going to discuss vMotion's Layer 2 requirements and how they interact (and don't interact) with other VMware networking functionality. My hope is that this article will provide a greater understanding of this topic.

First, I'd like to use a diagram to help explain what we're discussing here. In the diagram below, there is a single ESX/ESXi host with a management interface, a vMotion interface, and a couple of network interfaces dedicated to virtual machine (VM) traffic.

![VLAN Behaviors with VMware ESX/ESXi](/public/img/vmotion-l2-adjacency.jpg)

As you can see from this highly-simplified diagram, there are three basic types of network interfaces that ESX/ESXi uses: management interfaces, VMkernel interfaces, and VM networking interfaces. Each of them is configured separately and, in many configurations, quite differently.

For example, the management interface (in VMware ESX this is the Service Console interface, in VMware ESXi this is a separate instance of a VMkernel interface) is typically configured to connect to an access port with access to a single VLAN. In Cisco switch configurations, the interface configuration would look something like this:

	interface GigabitEthernet 1/1  
	switchport mode access  
	switchport access vlan 27  
	spanning-tree portfast

There might be other commands present, but these are the basic recommended settings. This makes the management interfaces on an ESX/ESXi host act, look, feel, and behave just exactly like the network interfaces on pretty much every other system in the data center. Although VMware HA/DRS cluster design considerations might lead you toward certain Layer 2 boundaries, there's nothing stopping you from putting management interfaces from different VMware ESX/ESXi hosts in different Layer 3 VLANs and still conducting vMotion operations between them.

So, if it's not the management interfaces that are gating the practicality of vMotion in today's data centers, it must be the VM networking interfaces, right? Not exactly. Although the voices speaking up against vMotion in this online discussion often cite Layer 3 VLAN concerns as the primary problem with vMotion---stating, rightfully so, that the IP address of a migrated virtual machine cannot and should not change---these individuals are overlooking the recommended configuration for VM networking interfaces in a VMware ESX/ESXi environment.

Physical network interfaces in a VMware ESX/ESXi host that will be used for VM networking traffic are most commonly configured to act as 802.1Q VLAN trunks. For example, the Cisco switch configuration for a port connected to a network interface being used for VM networking traffic would look something like this:

	interface GigabitEthernet1/10  
	switchport trunk encapsulation dot1q  
	switchport mode trunk  
	switchport trunk allowed vlan 1-499  
	switchport trunk native vlan 499  
	spanning-tree portfast trunk

As before, some commands might be missing or some additional commands might be present, but this gives you the basic configuration. In this configuration, the VM networking interfaces are actually capable of supporting multiple VLANs simultaneously. (More information on the configuration of VLANs with VMware ESX/ESXi can be found in [this aging but still very applicable article][2].)

In practical use what this means is that any given VMware ESX/ESXi host could have VMs running on it that exist on a completely different and separate VLAN than the management interface. In fact, any given VMware ESX/ESXi host might have multiple VMs running across multiple separate and distinct VLANs. And as long as the ESX/ESXi hosts are properly configured to support the same VLANs, you can easily vMotion VMs between ESX/ESXi hosts when those VMs are in different VLANs. So, the net effect is that ESX/ESXi can easily support multiple VLANs at the same time for VM networking traffic, and this means that vMotion's practical use isn't gated by some inherent limitation of the VM networking interfaces themselves.

Where in the world, then, does this Layer 2 adjacency thing keep coming from? If it's not management interfaces (it isn't), and it's not VM networking interfaces (it isn't), then what is it? It's the VMkernel interface that is configured to support vMotion. In order to vMotion a VM from one ESX/ESXi host to another, each host's vMotion-enabled VMkernel interface has to be in the same IP subnet (i.e., in the same Layer 2 VLAN or broadcast domain). Going back to Cisco switch configurations, a vMotion-enabled VMkernel port will be configured very much like a management interface:

	interface GigabitEthernet 1/5  
	switchport mode access  
	switchport access vlan 37  
	spanning-tree portfast

This means that the vMotion-enabled VMkernel port is just an access port (no 802.1Q trunking) in a single VLAN.

Is this really a limitation on the practical use of vMotion? Hardly. In [my initial rebuttal][1] of the claims against vMotion, I pointed out that because it is _only the VMkernel interface_ that must share Layer 2 adjacency, all this really means is that a vMotion domain is limited to the number of vMotion-enabled VMkernel interfaces you can put into a single Layer 2 VLAN/broadcast domain. VMs are unaffected, as you saw earlier, as long as the VM networking interfaces are correctly configured to support the appropriate VLAN tags, and the management interfaces do not, generally speaking, have any significant impact.

Factoring in the consolidation ratio, as I did in my earlier post, and you'll see that it's possible to support very large numbers of VMs spread across as many different VLANs as you want with a small number of ESX/ESXi hosts. Consider 200 ESX/ESXi hosts---whose vMotion-enabled VMkernel interfaces would have to share a broadcast domain---with a consolidation ratio of 12:1. That's 2,400 VMs that can be supported across as many different VLANs simultaneously as we care to configure. Do you want 400 of those VMs in one VLAN while 300 are in a different VLAN? No problem, you only need to configure the physical switches and the virtual switches appropriately.

Let's summarize the key points here:

1. Only the vMotion-enabled VMkernel interface needs to have Layer 2 adjacency within a data center.

2. Neither management interfaces nor VM networking interfaces require Layer 2 adjacency.

3. VM networking interfaces are easily capable of supporting multiple VLANs at the same time.

4. When the VM networking interfaces on multiple ESX/ESXi hosts are identically configured, VMs on different VLANs can be migrated with no network disruption even though the ESX/ESXi hosts themselves might all be in the same VLAN.

5. Consolidation ratios multiply the number of VMs that can be supported with Layer 2-adjacent vMotion interfaces.

Based on this information, I hope it's clearer that vMotion is, in fact, quite practical for today's data centers. Yes, there are design considerations that come into play, especially when it comes to long-distance vMotion (which requires stretched VLANs between multiple sites). However, this is still an emerging use case; the far broader use case is within a single data center. Within a single data center, all the key points I provided to you apply, and there is no practical restriction to leveraging vMotion.

If you still disagree (or even if you agree!), feel free to speak up in the comments. Courteous and professional comments (with full disclosure, where applicable) are always welcome.

[1]: {{< relref "2010-06-13-the-vmotion-reality.md" >}}
[2]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
