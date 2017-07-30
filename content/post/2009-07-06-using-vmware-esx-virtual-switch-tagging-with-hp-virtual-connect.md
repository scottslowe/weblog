---
author: slowe
categories: Tutorial
comments: true
date: 2009-07-06T17:16:33Z
slug: using-vmware-esx-virtual-switch-tagging-with-hp-virtual-connect
tags:
- Hardware
- HP
- Networking
- Virtualization
- VLAN
- VMware
title: Using VMware ESX Virtual Switch Tagging with HP Virtual Connect
url: /2009/07/06/using-vmware-esx-virtual-switch-tagging-with-hp-virtual-connect/
wordpress_id: 1446
---

In January 2008, SearchVMware.com published an article of mine titled [VMware ESX Server Networking with HP Virtual Connect](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1295274,00.html). In that article, I stated that one drawback of HP Virtual Connect was that it forced you to use External Switch Tagging (EST):

>When used in conjunction with ESX Server, shared uplink sets force the use of EST because VLAN tags are stripped away by the Virtual Connect switch. Therefore, the ESX Server can't use the VLAN tags, and must resort to a different vSwitch---each with one or more pNICs as uplinks---for each VLAN/associated network. This solution may be useful in some situations, but typically wouldn't scale well for environments with many different VLANs.

It turns out that a firmware revision to the HP Virtual Connect software addresses this problem. As I've recently had the opportunity to work with an HP Virtual Connect Flex-10 module (you can expect to see some articles on Flex-10 in the near future), I wanted to revisit the idea of using HP Virtual Connect with VMware ESX/ESXi. In this post, I'll describe how you go about configuring HP Virtual Connect so that you can use Virtual Switch Tagging (VST) and Shared Uplink Sets together. Each of the steps is described in one of the sections below.

## Configure VC for VLAN Mapping

Before you even create the Shared Uplink Set, you must first configure the Virtual Connect module to use VLAN mapping instead of VLAN tunneling.

To access the option for configuring VLAN mapping, you can use the menu bar across the top of the right side of the HP Virtual Connect Manager. Simply click Configure > Ethernet Settings, then click on the Advanced Settings tab. There you will see the option to either tunnel VLAN tags or map VLAN tags. Choose to map VLAN tags. Optionally, you can click the check box to force server connections to use the same VLAN mappings; this will eliminate a step later but limits the overall flexibility of the solution. It's up to you if you want to use this option.

## Create the Shared Uplink Set

Now you're ready to create the Shared Uplink Set. Using the menu bar across the top of the right side of the HP Virtual Connect Manager interface, simply choose Define > Shared Uplink Set. Specify a name for the uplink set, then choose the external uplink ports. While you here, you can also go ahead and create the associated networks you'll need later.

## Create the Associated Networks

Either while you're creating the Shared Uplink Set or after the Shared Uplink Set has been created, you can add the Associated Networks. While you are editing the Shared Uplink Set, simply click the Add Network button under the Associated Networks area and define one or more VLANs. The following parameters are available when you define an Associated Network:

* **Network Name and VLAN ID:** These are pretty self-explanatory. The VLAN ID here needs to match the external VLAN ID on the rest of the network.

* **Native:** If this VLAN is marked as the native VLAN on the rest of your network, check this box. This network would then receive all of the untagged traffic on the uplink set.

* **Smart Link:** If you would like this network to be marked as down if the uplinks go down, check this box.

* **Private network:** With this box checked, the network will act like a private VLAN---nodes on this network cannot communicate with each other.

* **Advanced:** This area allows you to set a custom speed for either the preferred speed or the maximum speed.

After you've defined the Associated Networks, then you're ready to create the Server Profile---and that's where the real magic in making VST is found.

## Assign Networks in the Server Profile

Once again, you'll use the menu bar across the right side of the HP Virtual Connect Manager to create a Server Profile. Simply select Define > Server Profile. In the Server Profile, you'll add a network for each NIC present in the server blade (for a blade with Flex-10 NICs and a Flex-10 module, you will have eight NICs). When prompted for what network to associate to that NIC, choose Multiple Networks. Then click the small Edit button just to the right of the Network Name drop-down to show the Server VLAN to vNet Mappings screen.

The fact that you selected "Multiple Networks" when you added the connection to the Server Profile means that Virtual Connect will pass the VLAN tags up to the blade. The fact that you configured the Virtual Connect module to use VLAN mapping now means that you can create an association between the VLAN tags that a server uses and an corresponding Associated Network.

If you want to keep the server's VLAN tags and the Associated Networks' VLAN IDs matched up, just follow these steps:

1. Check the box labeled Force Same VLAN Mappings As Shared Uplink Sets.

2. Choose the Shared Uplink Set from the drop-down list.

3. Place a check mark next to each Associated Network/vNet/VLAN. This tell the Virtual Connect module to include that VLAN.

4. Place a check mark under Untagged for whichever Associated Network is should be handled as the untagged (native) VLAN.

If, on the other hand, you want to specify different server VLAN tags than Associated Network/vNet VLAN IDs, leave the check box for Force Same VLAN Mappings As Shared Uplink Sets _unchecked_, and specify the server VLAN ID that should be used for each Associated Network/vNet. For example, if the server was using VLAN ID 10 to refer to the Production network, but the Production network was using VLAN 1000 on the Associated Network and on the rest of the network, then choose the Associated Network that represents the Production network and specify a server VLAN ID of 10. This allows you to create a mapping between the VLAN tags the server uses and the VLAN tags the rest of the network uses.

After you've defined the Server Profile and created the vNet mappings, attach the Server Profile to a blade running VMware ESX/ESXi and you're good to go! Within VMware ESX/ESXi, you would configure the vSwitches, distributed vSwitches, and port groups as you would normally.

## How I Tested

My testing was performed on a HP Virtual Connect Flex-10 module running firmware revision 2.10. The Flex-10 module was uplinked via a single 10Gbps connection to an HP ProCurve switch. The blades were running VMware ESX 4.0.0.
