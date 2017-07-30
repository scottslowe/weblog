---
author: slowe
categories: Tutorial
comments: true
date: 2007-06-22T13:45:40Z
slug: link-state-tracking-in-blade-deployments
tags:
- Cisco
- Hardware
- HP
- IBM
- IOS
- Networking
title: Link State Tracking in Blade Deployments
url: /2007/06/22/link-state-tracking-in-blade-deployments/
wordpress_id: 476
---

It's common in blade deployments to use multiple Ethernet switches in the blade chassis to provide network redundancy (I'll refer to these as "in-chassis switches" moving forward). For example, in both the [IBM BladeCenter H](http://www-03.ibm.com/systems/bladecenter/bladeh/) and the [HP BladeSystem c-Class](http://h18004.www1.hp.com/products/blades/components/c-class-components.html), we can provision multiple in-chassis switches so that half of the NICs on the blades connect to one in-chassis switch and the other half connect to the other switch. Within the OS, we load NIC teaming software to provide automatic failover if one of the links goes down. In this scenario, if one of the in-chassis switches fails then traffic will automatically fail over to the other switch.

In cases like this, everything works as advertised. But what about when the in-chassis switch stays up, but the uplink from that switch to the outside world goes down (perhaps the upstream switch went down or the link was unplugged)? In that case, the link from the in-chassis switch to the blade's NIC is still up, and therefore the NIC teaming software in the OS does not know that a problem has occurred and will not move the traffic to the other link. In situations like this, we need to implement _link state tracking_.

&lt;aside&gt;Astute readers will recognize that link state tracking is actually applicable in any server deployment---not just a blade server deployment---where the servers connect to a distribution switch and not the core. I'm just going to focus on blade server deployments here, but the configuration would be much the same, if not exactly the same, in non-blade server deployments.&lt;/aside&gt;

Link state tracking is pretty easy to configure; you define one or more upstream ports and one or more downstream ports. The _upstream_ port(s) are the ports that uplink to the rest of the network; in a blade server deployment, this would be the ports (or port groups) that connect to the network backbone. The _downstream_ port(s) are the ports that connect back to the servers.

Here's an example. We have a Cisco in-chassis switch that has a GigabitEtherChannel port group defined as an uplink out to the outside world:

	interface Port-Channel1  
	description Uplink to network backbone  
	switchport trunk encapsulation dot1q  
	switchport trunk native vlan 2  
	switchport trunk allowed vlan 2-4094  
	switchport mode trunk  
	link state group 1 upstream

Note the `link state group 1 upstream` command, which marks this port channel as an upstream port. If all the links in this port channel go down (thus making the port channel itself go down), then the switch will notify downstream ports in the same group to mark themselves as down also.

The member ports of this port channel would **not** have the `link state` command present:

	interface GigabitEthernet0/18  
	description Port group member for uplink to network  
	switchport trunk encapsulation dot1q  
	switchport trunk native vlan 2  
	switchport trunk allowed vlan 2-4094  
	switchport mode trunk  
	channel-group 1 mode on

So for the ports on the same in-chassis switch that are connecting to the servers in the chassis, we have this configuration:

	interface GigabitEthernet0/10  
	description Web server NIC  
	switchport access vlan 2  
	switchport mode access  
	link state group 1 downstream  
	spanning-tree portfast

Note the `link state group 1 downstream` command, which marks this port as a downstream port from the Port-Channel1 interface. If Port-Channel1 goes down (because all the member links in Port-Channel1 also went down), then GigabitEthernet0/10 will also go down. Because GigabitEthernet0/10 went down, the NIC teaming software running in the OS on the blade will fail the traffic over to a different NIC, presumably a NIC that connects to the redundant in-chassis switch.

You'll also need the global `link state track 1` global command to enable link state tracking (thanks for the clarification, Matt!).

Because of the nature of blade deployments, this sort of configuration is particularly applicable in blade deployments, but also applies in other situations as well (as mentioned earlier). I hope this is useful!

**UPDATE:** I've changed from using "chassis switch" to "in-chassis switch" to help avoid confusion with products like the Cisco Catalyst 6500 series, which are commonly referred to as chassis switches. Thanks, James!
