---
author: slowe
categories: Explanation
comments: true
date: 2009-07-09T10:03:45Z
slug: using-multiple-vlans-with-hp-virtual-connect-flex-10
tags:
- Hardware
- HP
- Networking
- Virtualization
title: Using Multiple VLANs with HP Virtual Connect Flex-10
url: /2009/07/09/using-multiple-vlans-with-hp-virtual-connect-flex-10/
wordpress_id: 1457
---

In this article on using [VMware ESX Virtual Switch Tagging (VST) with HP Virtual Connect][1], I showed you how to use the Multiple VLANs setting to map multiple VLANs onto a network connection so that the VLAN tags would pass all the way up to the VMware ESX/ESXi host---a necessary prerequisite for making VST work.

However, there is a key caveat to this approach that applies when using HP Virtual Connect Flex-10 and HP blades that have Flex-10 LOM (LAN on Motherboard) interfaces. As you might already know, Flex-10 LOMs have the ability to "subdivide" themselves into four logical instances, each of them a valid PCIe function, which are called FlexNICs. These FlexNICs appear as real, actual, physical NICs to the operating system installed on the blades. This includes VMware ESX/ESXi. In the Virtual Connect Manager, though, you have the ability to fine-tune the amount of bandwidth allocated to each of these FlexNICs, up to the shared maximum of 10Gbps.

This is pretty cool, but there is one limitation of which you must be aware---a limitation that is particularly significant in VMware ESX/ESXi environments. When you use the Multiple Networks option to map multiple VLANs onto a FlexNIC, _you can't map the same VLAN onto two different FlexNICs from the same LOM._

The FlexNICs are noted as LOM 1:a, LOM 1:b, LOM 2:a, etc. Again, as noted earlier, up to four FlexNICs are presented to the operating system on the blade. When you start assigning network connections in a Server Profile in Virtual Connect Manager, these network connections will bounce back and forth between the LOMs (assuming there are no other network interface cards in the server blade):

First network connection > LOM 1:a  
Second network connection > LOM 2:a  
Third network connection > LOM 1:b  
Fourth network connection > LOM 2:b  

...  

Seventh network connection > LOM 1:d  
Eighth network connection > LOM 2:d

As far as I know, there is no way to change this behavior.

With that in mind, what this means is that you can't map the same VLANs to the first, third, fifth, and seventh network connections, or to the second, fourth, sixth, or eighth network connections. Why? Because each of these connections are logical FlexNICs on the same LOM, and _you can't map the same VLANs to more than one FlexNIC on the same LOM._

Perhaps an example would help. Consider the configuration shown in this figure, in which multiple VLANs are mapped to all eight connections in Virtual Connect Manager:

![Incorrect Flex10 VLAN setting](/public/img/hp-flex10-vlans-incorrect.png)

This screenshot shows how the VLANs are mapped for each of those eight network connections:

![Flex10 VLAN mapping](/public/img/hp-flex10-vlan-mapping.png)

As you can see, I have the same set of five VLANs mapped onto all eight network connections (all eight logical FlexNIC instances). But only the first two show OK---the rest show Critical. Why? Because these logical FlexNICs have the same VLANs mapped to them as were mapped to the first FlexNIC, and therefore Virtual Connect Manager has placed them into a Critical state (they'll be reported as "Down" to an operating system on the blade).

This behavior is the strange behavior I tweeted about a few days ago, where I couldn't figure out why Virtual Connect was behaving in the way that it was. Now I know why!

Contrast that first configuration with the configuration shown in this screenshot:

![Correct Flex10 VLAN configuration](/public/img/hp-flex10-vlans-correct.png)

In this case, you'll note that I do not have the same VLANs mapped to more than one FlexNIC on the same LOM. As a result, Virtual Connect Manager does not place any of the FlexNICs into a Critical state, and all eight show OK (and will be reported as Up to an operating system on the blade).

So what does this mean? In its simplest terms, it means you can't use VST on all the FlexNICs---some of the FlexNICs will have to carry "ordinary" traffic to VMware ESX/ESXi port groups that have no VLAN ID specified. In the image above, you can see that the first three pairs of FlexNICs each carry a specific type of traffic. The matching output of `esxcfg-vswitch --list` for this VMware ESX host shows that the port groups on each of the three matching vSwitches do not have any VLAN IDs specified. This is because, in this configuration, these three pairs of FlexNICs carry only a single type of traffic, and that single type of traffic has no VLAN tags attached. Therefore, the VMware ESX/ESXi port groups must not have a VLAN ID specified in order for traffic to flow.

But it also presents some other interesting design considerations. If your VMware ESX Service Console (or VMware ESXi Management interface) is on the same VLAN as some of your virtual machines, you'll run into an issue---you won't be able to map the VLAN to one set of FlexNICs for Service Console traffic and then map that same VLAN to another set of FlexNICs for other virtual machine traffic. In effect, it greatly reduces the extent to which you can use VST on VMware ESX/ESXi hosts.

Of course, the other way of handling it is to assign only two network connections, map multiple VLANs to those network connections, assign the full 10Gbps of throughput to those two FlexNICs (network connections), and use a single vSwitch design.

As far as I can tell, this is not documented by HP in the Virtual Connect (or Flex-10) documentation. So, you might want to bookmark this article, or post it to Delicious.com or similar. Finally, as always, I'd love to hear any feedback or clarifications in the comments. Thanks!

[1]: {{< relref "2009-07-06-using-vmware-esx-virtual-switch-tagging-with-hp-virtual-connect.md" >}}
