---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-04T14:37:46Z
slug: esx-server-nic-teaming-and-vlan-trunking
tags:
- Cisco
- ESX
- Interoperability
- IOS
- Networking
- Virtualization
- VLAN
- VMware
title: ESX Server, NIC Teaming, and VLAN Trunking
url: /2006/12/04/esx-server-nic-teaming-and-vlan-trunking/
wordpress_id: 377
---

Before we get into the details, allow me to give credit where credit is due. First, thanks to Dan Parsons of [IT Obsession](http://www.itobsession.com/) for an [article that jump-started the process](http://www.itobsession.com/2005/12/20/nic-teaming-8023ad-in-vmware-esx-server/) with notes on the Cisco IOS configuration. Next, credit goes to the VMTN Forums, especially [this thread](http://www.vmware.com/community/thread.jspa?messageID=445938&#445938), in which some extremely useful information was exchanged. I would be remiss if I did not adequately credit these sources for the information that helped make this testing successful.

There are actually two different pieces described in this article. The first is NIC teaming, in which we logically bind together multiple physical NICs for increased throughput and increased fault tolerance. The second is VLAN trunking, in which we configure the physical switch to pass VLAN traffic directly to [ESX Server](http://www.vmware.com/products/vi/esx/), which will then distribute the traffic according to the port groups and VLAN IDs configured on the server. I wrote about [ESX and VLAN trunking][1] a long time ago and ran into some issues then; here I'll describe how to work around the issues I ran into at that time.

So, let's have a look at these two pieces. We'll start with NIC teaming.

## Configuring NIC Teaming

There's a bit of confusion regarding NIC teaming in ESX Server and when switch support is required. You can most certainly create NIC teams (or "bonds") in ESX Server without any switch support whatsoever. Once those NIC teams have been created, you can configure load balancing and failover policies. However, those policies will affect _outbound traffic only._ In order to control inbound traffic, we have to get the physical switches involved. This article is written from the perspective of using Cisco Catalyst IOS-based physical switches. (In my testing I used a Catalyst 3560.)

To create a NIC team that will work for both inbound and outbound traffic, we'll create a port channel using the following commands:

	s3(config)#int port-channel1  
	s3(config-if)#description NIC team for ESX server  
	s3(config-if)#int gi0/23  
	s3(config-if)#channel-group 1 mode on  
	s3(config-if)#int gi0/24  
	s3(config-if)#channel-group 1 mode on

This creates port-channel1 (you'd need to change this name if you already have port-channel1 defined, perhaps for switch-to-switch trunk aggregation) and assigns GigabitEthernet0/23 and GigabitEthernet0/24 into team. Now, however, you need to ensure that the load balancing mechanism that is used by both the switch and ESX Server matches. To find out the switch's current load balancing mechanism, use this command in enable mode:

	show etherchannel load-balance

This will report the current load balancing algorithm in use by the switch. On my Catalyst 3560 running IOS 12.2(25), the default load balancing algorithm was set to "Source MAC Address". On my ESX Server 3.0.1 server, the default load balancing mechanism was set to "Route based on the originating virtual port ID". The result? The NIC team didn't work at all---I couldn't ping any of the VMs on the host, and the VMs couldn't reach the rest of the physical network. It wasn't until I matched up the switch/server load balancing algorithms that things started working.

To set the switch load-balancing algorithm, use one of the following commands in global configuration mode. To enable IP-based load balancing, use this command:

	port-channel load-balance src-dst-ip

To enable MAC-based load balancing, use this command:

	port-channel load-balance src-mac

There are other options available, but these are the two that seem to match most closely to the ESX Server options. I was unable to make this work at all without switching the configuration to "src-dst-ip" on the switch side and "Route based on ip hash" on the ESX Server side. From what I've been able to gather, the "src-dst-ip" option gives you better utilization across the members of the NIC team than some of the other options. (Anyone care to contribute a URL that provides some definitive information on that statement?)

Creating the NIC team on the ESX Server side is as simple as adding physical NICs to the vSwitch and setting the load balancing policy appropriately. At this point, the NIC team should be working.

## Configuring VLAN Trunking

In my testing, I set up the NIC team and the VLAN trunk at the same time. When I ran into connectivity issues as a result of the mismatched load balancing policies, I thought they were VLAN-related issues, so I spent a fair amount of time troubleshooting the VLAN side of things. It turns out, of course, that it wasn't the VLAN configuration at all. (In addition, one of the VMs that I was testing had some issues as well, and that contributed to my initial difficulties.)

To configure the VLAN trunking, use the following commands on the physical switch:

	s3(config)#int port-channel1  
	s3(config-if)#switchport trunk encapsulation dot1q  
	s3(config-if)#switchport trunk allowed vlan all  
	s3(config-if)#switchport mode trunk  
	s3(config-if)#switchport trunk native vlan 4094

This configures the NIC team (port-channel1, as created earlier) as a 802.1Q VLAN trunk. You then need to repeat this process for the member ports in the NIC team:

	s3(config)#int gi0/23  
	s3(config-if)#switchport trunk encapsulation dot1q  
	s3(config-if)#switchport trunk allowed vlan all  
	s3(config-if)#switchport mode trunk  
	s3(config-if)#switchport trunk native vlan 4094  
	s3(config-if)#int gi0/24  
	s3(config-if)#switchport trunk encapsulation dot1q  
	s3(config-if)#switchport trunk allowed vlan all  
	s3(config-if)#switchport mode trunk  
	s3(config-if)#switchport trunk native vlan 4094

If you haven't already created VLAN 4094, you'll need to do that as well:

	s3(config)#int vlan 4094  
	s3(config-if)#no ip address

The `switchport trunk native vlan 4094` command is what fixes the problem I had last time I worked with [ESX Server and VLAN trunks][1]; namely, that most switches don't tag traffic from the native VLAN across a VLAN trunk. By setting the native VLAN for the trunk to something other than VLAN 1 (the default native VLAN), we essentially force the switch to tag all traffic across the trunk. This allows ESX Server to handle VMs that are assigned to the native VLAN as well as other VLANs.

On the ESX Server side, we just need to edit the vSwitch and create a new port group. In the port group, specify the VLAN ID that matches the VLAN ID from the physical switch. After the new port group has been assigned, you can place your VMs on that new port group (VLAN) and---assuming you have a router somewhere to route between the VLANs---you should have full connectivity to your newly segregated virtual machines.

## Final Notes

I did encounter a couple of weird things during the setup of this configuration (I plan to leave the configuration in place for a while to uncover any other problems).

* First, during troubleshooting, I deleted a port group on one vSwitch and then re-created it on another vSwitch. However, the virtual machine didn't recognize the connection. There was no indication inside the VM that the connection wasn't live; it just didn't work. It wasn't until I edited the VM, set the virtual NIC to a different port group, and then set it back again that it started working as expected. Lesson learned: don't delete port groups.

* Second, after creating a port group on a vSwitch with no VLAN ID, one of the other port groups on the same vSwitch appeared to "lose" its VLAN ID, at least as far as VirtualCenter was concerned. In other words, the VLAN ID was listed as "*" in VirtualCenter, even though a VLAN ID was indeed configured for that port group. The `esxcfg-vswitch -l` command (that's a lowercase L) on the host still showed the assigned VLAN ID for that port group, however.

* It was also the `esxcfg-vswitch` command that helped me troubleshoot the problem with the deleted/recreated port group described above. Even after recreating the port group, `esxcfg-vswitch` still showed 0 used ports for that port group on that vswitch, which told me that the virtual machine's network connection was still somehow askew.

Hopefully this information will prove useful to those of you out there trying to set up NIC teaming and/or VLAN trunking in your environment. I would recommend taking this one step at a time, not all at once like I did; this will make it easier to troubleshoot problems as you progress through the configuration.

[1]: {{< relref "2006-04-17-vlans-and-port-groups.md" >}}
