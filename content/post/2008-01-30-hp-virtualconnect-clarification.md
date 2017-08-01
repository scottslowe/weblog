---
author: slowe
categories: Explanation
comments: true
date: 2008-01-30T16:04:54Z
slug: hp-virtualconnect-clarification
tags:
- HP
- Networking
- Virtualization
- VLAN
- VMware
title: HP VirtualConnect Clarification
url: /2008/01/30/hp-virtualconnect-clarification/
wordpress_id: 619
---

SearchVMware.com recently published an article of mine about [using HP VirtualConnect with ESX Server](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1295274,00.html) and the impact it has on configuring ESX Server networking. Based on the comments to my [original post][1] about the article, it appears that I didn't adequately explain the interaction between VirtualConnect's Standard Ethernet Networks and VLAN tagging. I'd like to thank those readers that asked about this issue and take a moment to clarify.

Standard Ethernet Networks are defined in the HP VirtualConnect Manager and allow you to control the mapping between physical NIC ports on the blades (the "downstream ports") and the connections from the VirtualConnect switches to the external network infrastructure (the "upstream ports"). For example, I could define a Standard Ethernet Network called "Production" and specify that Production would uplink either via Slot 2 Port 7 or Slot 2 Port 8.

That part is pretty straightforward. Here's how it plays into VLAN tagging with ESX Server. Most organizations prefer to use VST (Virtual Switch Tagging; described in more detail in [this article](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1283036,00.html)). To use VST, the ports on the network infrastructure must be configured as 802.1Q VLAN trunks so that the external physical switches will pass the VLAN tags to the vSwitches inside ESX Server, where ESX Server can then handle the traffic appropriately.

To use VST with VirtualConnect and Standard Ethernet Networks, there is _no difference._ The VirtualConnect upstream ports, as defined in the Standard Ethernet Network configuration, must be plugged into physical switch ports that are configured as 802.1Q VLAN trunks. When you plug the upstream port into an 802.1Q VLAN trunk, the downstream port---the port going to the ESX Server---automatically becomes an 802.1Q VLAN trunk as well. The VLAN tags will pass all the way to the ESX Server's vSwitches, where ESX Server can deal with the traffic appropriately.

Likewise, if the VirtualConnect's upstream ports are plugged into a static access port---a port which is not configured as an 802.1Q VLAN trunk but instead carries traffic only for a single VLAN---then the downstream ports also become static access ports and you are, effectively, using External Switch Tagging (EST).

I stated this in the original article in the third paragraph in the section titled "How Virtual Connect differs in ESX":

>In either way, these Ethernet networks will "pass through" the 802.1Q status of the physical switch port to which it is uplinked. If the physical switch port to which they are connected is configured as an 802.1Q VLAN trunk, then the downstream ports will act as 802.1Q VLAN trunks. Likewise, if the uplink is connected to a switch port that is configured as a static access port, then the downstream ports will act as static access ports.

Shared Uplink Sets, on the other hand, are different. They don't behave in the same way as Standard Ethernet Networks. With Shared Uplink Sets, you are forced to use EST because the VLAN tags are stripped away at the VirtualConnect level. The associated networks that are defined in VirtualConnect Manager define the different VLANs, and each downstream port is connected to an associated network. Unlike with Standard Ethernet Networks, no VLAN tags are passed up to the ESX Server with Shared Uplink Sets.

So, to summarize:

* Standard Ethernet Network uplink connected to external switch port configured as 802.1Q VLAN trunk allows ESX Server to see VLAN tags and supports both VST and Virtual Guest Tagging (VGT)

* Standard Ethernet Network uplink connected to external switch port configured as static access port only allows EST configuration

* Shared Uplink Set only allows EST configuration

I hope this clarifies the interaction between HP VirtualConnect and ESX Server networking. I welcome any further questions or clarifications in the comments below. Thanks!

[1]: {{< relref "2008-01-23-new-article-on-hp-virtualconnect.md" >}}
