---
author: slowe
categories: Tutorial
comments: true
date: 2008-09-05T11:22:17Z
slug: vmware-esx-nic-teaming-and-vlan-trunking-with-hp-procurve
tags:
- ESX
- Hardware
- HP
- Networking
- Virtualization
- VMware
title: VMware ESX, NIC Teaming, and VLAN Trunking with HP ProCurve
url: /2008/09/05/vmware-esx-nic-teaming-and-vlan-trunking-with-hp-procurve/
wordpress_id: 862
---

In an earlier article about [VMware ESX, NIC teaming, and VLAN trunking][1], I described what the configuration should look like if one were using these features with Cisco switch hardware. It's been a quite popular post, one I will probably need to update soon.

In this article, I'd like to discuss how to do the same thing, but using HP ProCurve switch hardware. The article is broken into three sections: using VLANs, using link aggregation (NIC teaming), and using both together.

## Using VLAN Trunking

To my Cisco-oriented mind, VLANs with ProCurve switches are handled quite differently. Port-based VLANs, in which individual ports are assigned to one or more VLANs, allow a switch port to participate in that VLAN as either an _untagged_ fashion or in a _tagged_ fashion.

The difference here is really simpler than it may seem: the untagged VLAN can be considered the "native VLAN" from the Cisco world, meaning that the VLAN tags are not added to packets traversing that port. Putting a port in a VLAN in untagged mode is essentially equivalent to making that port an access port in the Cisco IOS world. Only one VLAN can be marked as untagged, which makes sense if you think about it.

Any port groups that should receive traffic from the untagged VLAN need to have VLAN ID 0 (no VLAN ID, in other words) assigned.

A tagged VLAN, on the other hand, adds the 802.1q VLAN tags to traffic moving through the port, like a VLAN trunk. If a user wants to use VST (virtual switch tagging) to host multiple VLANs on a single VMware ESX host, then the ProCurve ports need to have those VLANs marked as tagged. This will ensure that the VLAN tags are added to the packets and that VMware ESX can direct the traffic to the correct port group based on those VLAN tags.

In summary:

* Assign VLAN ID 0 to all port groups that need to receive traffic from the untagged VLAN (remember that a port can only be marked as untagged for a single VLAN). This correlates to the discussion about [VMware ESX and the native VLAN][2], in which I reminded users that port groups intended to receive traffic for the native VLAN should not have a VLAN ID specified.

* Be sure that ports are marked as tagged for all other VLANs that VMware ESX should see. This will enable the use of VST and multiple port groups, each configured with an appropriate VLAN ID. (By the way, if users are unclear on VST vs. EST vs. VGT, see [this article](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1283036,00.html).)

* VLANs that VMware ESX should not see at all should be marked as "No" in the VLAN configuration of the ProCurve switch for those ports.

## Using Link Aggregation

There's not a whole lot to this part. In the ProCurve configuration, users will mark the ports that should participate in link aggregation as part of a trunk (say, Trk1) and then set the trunk type. Here's the only real gotcha: the trunk must be configured as type "Trunk" and not type "LACP".

In this context, LACP refers to dynamic LACP, which allows the switch and the server to dynamically negotiate the number of links in the bundle. VMware ESX doesn't support dynamic LACP, only static LACP. To do static LACP, users will need to set the trunk type to Trunk.

Then, as has been discussed elsewhere in great depth, configure the VMware ESX vSwitch's load balancing policy to "Route based on ip hash". Once that's done, everything should work as expected. This blog entry gives the [CLI command to set the vSwitch load balancing policy][3], which would be necessary if configuring vSwitch0. For all other vSwitches, the changes can be made via VirtualCenter.

That's really all there is to making link aggregation work between an HP ProCurve switch and VMware ESX.

## Using VLANs and Link Aggregation Together

This section exists only to point out that when a trunk is created, the VLAN configuration for the members of that trunk disappears, and the trunk must be configured directly for VLAN support. In fact, users will note that the member ports don't even appear in the list of ports to be configured for VLANs; only the trunks themselves appear.

Key point to remember: apply your VLAN configurations _after_ your trunking configuration, or else you'll just have to do it all over again.

With this information, users should now be pretty well prepared to configure HP ProCurve switches in a VMware ESX environment. Feel free to post any questions, clarifications, or corrections in the comments below, and thanks for reading!

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[3]: {{< relref "2008-09-05-setting-vmware-esx-vswitch-load-balancing-policy-via-cli.md" >}}
