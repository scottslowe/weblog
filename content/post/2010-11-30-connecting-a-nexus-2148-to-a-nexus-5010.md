---
author: slowe
categories: Tutorial
comments: true
date: 2010-11-30T23:35:24Z
slug: connecting-a-nexus-2148-to-a-nexus-5010
tags:
- Cisco
- Networking
- Nexus
- CLI
title: Connecting a Nexus 2148 to a Nexus 5010
url: /2010/11/30/connecting-a-nexus-2148-to-a-nexus-5010/
wordpress_id: 2167
---

The Cisco Nexus 2000 series fabric extender (or "fex") is an implementation of [network interface virtualization][1]. Because the Nexus 2000 fabric extender acts as a remote line card to the upstream IV-capable bridge (a Nexus 5000 series switch, typically), all the configuration takes place on the upstream bridge. In this post, I'll describe how to connect a  Nexus 2148T to a Nexus 5010. In reality, the process is incredibly simple and not really worthy of a blog post, but in the interest of completeness I'll document it here.

The key to making the fabric extender work is the `switchport mode fex-fabric` command, used on a specific interface to let the Nexus 5000 know that a fabric extender is connected to that port:

	nexus(config)# interface eth2/1  
	nexus(config-if)# switchport mode fex-fabric

You can use the `switchport mode fex-fabric` command on a specific interface, as shown above, or on a port-channel:

	nexus(config)# interface port-channel 2  
	nexus(config-if)# switchport mode fex-fabric  
	nexus(config-if)# interface eth2/1  
	nexus(config-if)# switchport mode fex-fabric  
	nexus(config-if)# channel-group 2 mode on  
	nexus(config-if)# interface eth2/2  
	nexus(config-if)# switchport mode fex-fabric  
	nexus(config-if)# channel-group 2 mode on  
	nexus(config-if)# interface port-channel 2  
	nexus(config-if)# fex associate 100

In this example, I created a port-channel with two interfaces and associated the fabric extender to the port-channel, providing link-level redundancy for the connection between the Nexus 2148 and the upstream Nexus 5010.

To verify that the fabric extender is coming online properly, you can use a few different commands. The `show fex detail` command produces some useful output (only the first several lines are included here):

	FEX: 100 Description: FEX0100 state: Online  
	FEX version: 4.2(1)N1(1) [Switch version: 4.2(1)N1(1)]  
	FEX Interim version: 4.2(1)N1(1)  
	Switch Interim version: 4.2(1)N1(1)  
	Extender Model: N2K-C2148T-1GE,  Extender Serial: JAF1321ANJN  
	Part No: 73-12009-05  
	Card Id: 70, Mac Addr: 00:0d:ec:cf:03:42, Num Macs: 64  
	Module Sw Gen: 12594  [Switch Sw Gen: 21]  
	post level: complete  
	 pinning-mode: static    Max-links: 1  
	Fabric port for control traffic: Eth2/1  
	Fabric interface state:  
	Po2 - Interface Up. State: Active  
	Eth2/1 - Interface Up. State: Active

And that's really about it. Yes, there are more advanced configurations---I'm interested in exploring the use of virtual port-channels upstream of a Nexus 2000 series fabric extender to multiple Nexus 5000 switches---but this will get you started.

Feel free to post corrections, suggestions, or any other feedback in the comments!

[1]: {{< relref "2010-03-16-understanding-network-interface-virtualization.md" >}}
