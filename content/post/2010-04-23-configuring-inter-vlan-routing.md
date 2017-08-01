---
author: slowe
categories: Tutorial
comments: true
date: 2010-04-23T15:37:24Z
slug: configuring-inter-vlan-routing
tags:
- Cisco
- CLI
- Networking
- VLAN
title: Configuring Inter-VLAN Routing
url: /2010/04/23/configuring-inter-vlan-routing/
wordpress_id: 1902
---

Yesterday I completed the configuration of inter-VLAN routing (aka "router on a stick", or RoaS) as part of my ongoing CCNA preparation. A couple people mentioned that they would find the configuration useful, so I'm posting what I have. This is by no means a comprehensive treatise on the subject; for that, you should look elsewhere. Google can find you lots of sites with more in-depth and detailed information on the reasons behind the necessary configuration.

There are two primary components in a RoaS configuration:

1. The configuration of the VLANs and VLAN trunking port on the switch

2. The configuration of subinterfaces on the router

I describe how to configure each component below.

## VLANs and VLAN Trunking

How to create VLANs varies between various switch types. On some switches, you'll use the `vlan database` command in privileged EXEC mode. On other switches, you will use the `vlan <VLAN ID>` command while in global configuration mode. Regardless of which method is necessary for your particular Cisco switch, you will want to ensure that the switch has all the necessary VLANs defined.

After the VLANs have been defined, then you will need to configure the switch port connected to the router as a VLAN trunk port. This is pretty [well covered elsewhere][1], but here is a quick review of the commands (these commands assume port 15 on module 0, a Fast Ethernet port):

	switch(config)# int fa0/15  
	switch(config-if)# switchport trunk encapsulation dot1q  
	switch(config-if)# switchport mode trunk  
	switch(config-if)# switchport trunk allowed vlan 1,71-75  
	switch(config-if)# exit  
	switch(config)# exit  

A couple notes about these commands:

* Some switches only accept 802.1Q VLAN encapsulation; therefore, the `switchport trunk encapsulation dot1q` command isn't supported because that's the only encapsulation supported. So, the support for this command will vary from switch to switch.

* You will want to specify the correct VLANs for your environment in the `switchport trunk allowed vlan` command.

At this point, the switch is configured correctly; now it's time to move to the router.

## Subinterfaces on the Router

For each VLAN that needs to be routed, you will need to create a subinterface on the router. Creating a subinterface is pretty easy, the commands look something like this:

	router(config)# int fa0/0.1  
	router(config-if)# encapsulation dot1q 1 native  
	router(config-if)# ip address 192.168.1.1 255.255.255.0  
	router(config-if)# exit  
	router(config)# exit  

As before, there are a few notes to consider about these commands:

* The number of the subinterface (the "1" in `fa0/0.1` above) is only locally significant and doesn't need to match the VLAN ID, but matching the VLAN ID makes it easier to associate the subinterface with its configured VLAN ID. Again, as stated earlier, you'll need a separate subinterface for each VLAN that you want to route.

* Only specify the `native` keyword on the `encapsulation dot1q` command if this is the native VLAN on the switch side as well. Otherwise, the trunk won't form as expected.

* The IP address specified here will be the IP address of the default gateway for that VLAN/subnet.

For the physical interface itself, the interface needs to be up (so **don't** issue a `shutdown` command), but the interface does not need to have any IP address associated with it.

With this configuration in place, you should be able to route between the VLANs; just specify the IP address of the subinterface on the router for that VLAN as the default gateway of the systems on that VLAN and you should be good to go.

If I've missed anything glaring please speak up in the comments and let me know.

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
