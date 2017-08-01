---
author: slowe
categories: Explanation
comments: false
date: 2006-04-17T21:49:10Z
slug: vlans-and-port-groups
tags:
- Cisco
- ESX
- Networking
- Virtualization
- VLAN
- VMware
title: VLANs and Port Groups
url: /2006/04/17/vlans-and-port-groups/
wordpress_id: 226
---

One topic that I had been very interested in exploring was the idea of VLAN tagging within [ESX Server](http://www.vmware.com/products/esx/). ESX Server offers the ability to establish [802.1Q](http://en.wikipedia.org/wiki/802.1Q) trunks with compatible switches so that 802.1Q tagged frames can be routed to appropriately configured "port groups" within ESX Server. This would allow a single physical host server to run virtual servers in multiple VLANs without requiring a separate physical connection for each VLAN.

The idea of having an ESX Server create an 802.1Q trunk (even using multiple physical connections for redundancy) becomes really important when you move into the larger-scale server consolidation projects. Organizations just won't have the switch density (or the NIC density on the ESX Server, for that matter) to have multiple physical connections to each VLAN. The ability to "extend" these VLANs into ESX Server's virtual networking capabilities is very important.

Today, I set out to test this interoperability. A host server running ESX Server 2.5.3 (the latest version, just released a few days ago) was connected to a Cisco Catalyst 3524XL running IOS 12.0(5). The following commands were used to configure the port to which the ESX Server was connected:

    Switch(config-if): switchport mode trunk
    Switch(config-if): switchport trunk encapsulation dot1q

A quick review of the output from `sh int fa0/18 switchport` (where "fa0/18", for example, is the interface to which the ESX Server is connected) showed that the port was indeed configured and operating as an 802.1Q trunk.

Next, a new VLAN (VLAN ID 10) was created on the switch. No ports were placed into this VLAN because no physical ports were required; the ports would all be coming from ESX Server across the trunk.

Third, port groups were configured in the MUI for ESX Server. Two port groups were configured, one to match the native VLAN on the switch (VLAN 1, typically) and one to match VLAN 10.

Once all of the configuration was done, testing began. Upon moving the first VM to the default port group (this was the port group created to match the native VLAN), I lost connectivity to that VM from other systems connected to other ports also in the native VLAN. Thinking that perhaps all VMs needed to configured for a port group (some VMs were still connected to virtual switches and not port groups), I added the second running VM to the default port group as well. Connectivity was established between the two VMs within the same port group, but I still had no connectivity to other systems on the same VLAN.

After a fair amount of troubleshooting and searching, I came across a reference on the VMware web site regarding the use of native VLANs. VMware recommends against the use of native VLANs because switches tend to strip off the 802.1Q tags for frames on the native VLAN. To make VLAN tagging in ESX Server work in this situation, the switch must be reconfigured to tag all frames, including frames in the native VLAN.

The command in Cisco IOS to do this is `vlan dot1q tag native`. Unfortunately, this command is not supported on the version of IOS that is running on the Catalyst 3524XL I used in the test lab, so there was no way to make VLAN tagging work when using the native VLAN. This is a key "gotcha"---make sure that the switches support tagging native VLAN traffic if you plan to use native VLANs with ESX Server and port groups, otherwise it isn't going to work.

As soon as I have the opportunity to upgrade the IOS image on the Catalyst 3524XL switch in the test lab, I'll try testing port group/VLAN interoperability again and post results here.

**UPDATE:** I've posted some updated information and more comprehensive configuration notes in a posting titled "[ESX Server, NIC Teaming, and VLAN Trunking][1]".

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
