---
author: slowe
categories: Liveblog
comments: true
date: 2011-07-13T22:28:06Z
slug: brkdct-2121-virtual-device-context-design-and-implementation-considerations
tags:
- Cisco
- CiscoLive2011
- Networking
- Nexus
title: 'BRKDCT-2121: Virtual Device Context Design and Implementation Considerations'
url: /2011/07/13/brkdct-2121-virtual-device-context-design-and-implementation-considerations/
wordpress_id: 2340
---

I joined this session late (a last minute change from the UCS VM-FEX session). This session is on VDC design considerations with the Nexus 7000, presented by Ron Fuller, CCIE #5851 ([@ccie5851](http://twitter.com/ccie5851) on Twitter).

A VDC (virtual device context) is a "superstructure" that encapsulates (or encompasses) both VLANs and VRFs. Each VDC maintains its own VLAN table and its own VRF instances. There are some resources that are global (not dedicated to a particular VDC); these would include things like a boot image configuration. Other resources are dedicated to a VDC. This is stuff like VLANs, Layer 2 and Layer 3 ports, and IP address space. There are some shared resources, like the out-of-band (OOB) Ethernet management port.

VDCs are a licensed feature within NX-OS; they are part of the Advanced license. Up to 4 VDCs are supported. (Is the VDC limit a software limit, a hardware limit, or a license limit?) Note that 120 day evaluation licenses are available in NX-OS so that you can test features like VDCs, OTV, etc.

There was a question about the "Enhanced L2" license; this refers to FabricPath (see [my FabricPath session blog][1] for more information).

Note that VDCs are considered PCI compliant and are FIPS 140-2 certified.

Even if you don't buy the Advanced license, you still have a VDC: the default VDC. The default VDC is where you create other VDCs, where you perform NX-OS upgrades, where Control Plane Policing (CoPP) occurs, and where resource allocation (interfaces and memory) occurs. Any non-default VDCs are fully functional (of course), and changes in the non-default VDC (which would be VDCs 2-4) only affect that VDC (with the exception of "cascading" impacts due to peer adjacencies, connections, etc.). Each VDC has its own discrete configuration file, discrete RBAC, discrete TACACS, discrete SNMP, etc.

As of NX-OS 5.1, there are now "module-type" modes. This parameter defines the behavior for each VDC. This allows you to specify an `m1` mode (can contain M1 modules), `m1-xl` (can only contain M1-XL modules), or `f1` mode (can contain F1 modules). You use these modes with the `limit-resource module-type` command.

As of NX-OS 5.2, you can define a storage VDC. The storage VDC is a "virtual MDS" within the Nexus 7000. The storage VDC consumes a domain ID, participates in zoning and VSANs, FC alias, IVR, fabric binding, etc. The storage VDC also provides FCoE ISLs (VE_ports) to other FCoE switches. Only a single storage VDC is allowed, it does not require the Advanced license, but does count toward the total VDC count.

Keep in mind that only F1 modules support FCoE; therefore, only F1 modules would be allowed in a storage VDC.

NX-OS has the ability to allocate resources on demand; this allows you to allocate resources to individual VDCs based on the VDC's specific requirements. You can also fine-tune the resource allocation.

Although you can assign ports on a per-VDC basis, some modules have groups of ports that share ASICs. In those instances, you have to allocate all the ports that share an ASIC. For example, the N7K-M132XP-12 has groups of 4 ports that share an ASIC; therefore, these ports have to be allocated in groups of 4. The N7K-F132XP-15 card, on the other hand, shares an ASIC for every 2 ports, so ports are allocated to VDCs in groups of 2. The N7K-M108X2-12L card can assign ports individually. All M1 48 port line cards can have their ports allocated individually, although there is a recommendation to align port allocation in groups of 12. You also cannot split a FEX between VDCs; all the ports on a FEX belong the VDC of the uplink port into the Nexus 7000.

How do you allocate ports to a VDC? In config mode, use the `vdc <Name>` command to enter a VDC, and then use the `allocate interface` command. You can also use the `show vdc membership` command to show VDC allocation.

Prior to NX-OS 5.2, an interface allocated to a VDC isn't visible from any other VDC, including the default VDC. NX-OS 5.2 enables an exception to this to enable FCoE initiators coming in on one VDC to see targets on a storage VDC by sharing interfaces between an Ethernet VDC and a storage VDC. The Nexus 7000 automatically splits traffic based on Ethertype (0x8914 and 0x8906, FIP and FCoE, are automatically redirected to the storage VDC). Shared interfaces must be on N7K-F132XP-15 modules, and the ports must be configured as a 802.1Q trunk. Use the `allocate shared interface` command to configure shared interfaces.

There is no soft cross-connect over the backplane to connect VDCs; all communication has to go through ports on the front panel. There are no restrictions on module types (M1 to F1, etc.) If you are using vPC or vPC+ between VDCs, make sure the domain IDs are unique. All the interfaces in a vPC peer link must come from the same module type.

Creating a VDC is accomplished using the `vdc <New Name>` command. Add `type storage` to that command to create a storage VDC (and that will automatically limit the module types as stated earlier). The show `vdc <New Name> detail` will give you more details about the specified VDC.

To navigate between VDCs, use the `switchto vdc <Name>` command; to switch back to the default VDC, use the `switchback` command. Use of the `switchto` command can be regulated via RBAC (which does not stand for Rogue Badger Access Control, by the way). You can only use the `switchto` command to move from the default VDC to a non-default VDC; you can't jump between non-default VDCs.

Non-default VDCs can be suspended, resumed, reloaded, or restarted. Use the `reload` command to reload a VDC; use the `vdc <Name> suspend` command to suspend a VDC. Reloading the default VDC reloads all VDCs.

Some VDC leading practices:

* Reserve the default VDC for administrative functions.

* On VDC 1 assign accounts with minimum privileges necessary to perform operational tasks.

* Use a linecard per VDC for improved HA and VDC isolation.

* Customize VDC HA policy and resource configurations as necessary.

Ron now moves on to a discussion of consolidation with VDCs. Consolidation is one of those areas where you have to consider the trade-offs between various network configurations. VDCs do enable 4:1 consolidation while still maintaining hierarchy. Naturally, there are risks (a catastrophic hardware failure will take out all the VDCs). Other considerations:

* VDC to forwarding engine mapping

* A single chassis is still a single point of failure

* MAC table sizing is bound to the "lowest common denominator"

* There are a limited number of SPAN sessions

Not only can you use VDCs for consolidation, you can also use VDCs for segmentation. Perhaps you want to use a pair of Nexus 7000s for core, DMZ, and Internet edge traffic. Using VDCs can make this possible. (In my mind this is just another consolidation scenario).

NX-OS 5.2 introduces support for MPLS, which means that each VDC can operate as a separate MPLS router (label switching router, LSR). This offers the opportunity to consolidate multiple layers of PE router and P routers.

VDC also works well with OTV. OTV requires an OTV edge device, and there is no universal agreement upon where the OTV edge device should be placed.  OTV does require separation of the SVI routing and the OTV encapsulation; this is ideally accomplished using VDCs instead of multiple physical appliances (including "on-a-stick" variations). Ron walks through several different scenarios and ways in which VDCs could be incorporated into an OTV design.

Next Ron shifts gears to discuss VDCs with FCoE. The use of VDCs allows us to further leverage fabric/network convergence beyond just the access layer. While it won't necessarily reduce the number of links between switches, it will reduce the overall number of switches and still allow us to maintain SAN A/SAN B separation, role-based access, etc.

The next technology Ron looks at with VDCs is FabricPath. I wasn't able to capture all of these information. Ron then wrapped up the session.

[1]: {{< relref "2011-07-12-brkdct-2081-cisco-fabricpath-technology-and-design.md" >}}