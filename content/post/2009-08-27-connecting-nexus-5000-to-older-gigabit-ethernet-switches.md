---
author: slowe
categories: Tutorial
comments: true
date: 2009-08-27T15:01:35Z
slug: connecting-nexus-5000-to-older-gigabit-ethernet-switches
tags:
- Cisco
- HP
- Networking
- Nexus
- CLI
title: Connecting Nexus 5000 to Older Gigabit Ethernet Switches
url: /2009/08/27/connecting-nexus-5000-to-older-gigabit-ethernet-switches/
wordpress_id: 1567
---

One of the things that confused me when I first started working with the Nexus 5000 line was how I would connect this 10Gb Ethernet switch to older 1Gb Ethernet switches, like the older Cisco Catalyst or HP ProCurve switches that I also have in the lab. It turns out---many of you probably already know this---that the first 8 ports on a Nexus 5010 and the first 16 ports on Nexus 5020 can be configured to operate as Gigabit Ethernet ports. You can use these ports to connect to older Gigabit Ethernet switches.

It's really not too complicated. In this post, I'll describe the configuration I used to connect a Cisco Catalyst 3560G and an HP ProCurve 2924 to a Cisco Nexus 5010. In both cases, I used a 2-port port channel to link the switches together. The one drawback is that the Nexus doesn't participate in VTP, so all VLANs have to be manually defined on each switch independently. For my small lab environment, that's not a showstopper, but it does underscore the fact the Nexus 5000 series is primarily target as access switches.

Here's the configuration I used on the Nexus:

	interface Ethernet1/3  
	switchport mode trunk  
	speed 1000  
	switchport trunk native vlan 999  
	channel-group 3 mode on

This configuration was repeated on 2 ports for the Cisco Catalyst 3560G and on 2 more ports for the HP ProCurve 2924. Obviously, each of them used a different port channel (`channel-group 3 mode on` for the Catalyst and `channel-group 4 mode on` for the ProCurve). Remember that you have to use one of the first 8 (for a Nexus 5010) or the first 16 (for a Nexus 5020) ports because these are the only ports that support setting the speed down to Gigabit Ethernet.

On the Cisco Catalyst 3560G, the configuration is almost identical:

	interface GigabitEthernet1/10  
	switchport mode trunk  
	switchport trunk native vlan 999  
	channel-group 2 mode on

This configuration is repeated on two ports (same as the Nexus). Note that the channel-groups don't have to match _between_ the switches, only _within_ each switch. There's no need to specify the speed here on the Catalyst, as this is already a Gigabit Ethernet port. We only need to specify the speed on the Nexus because it won't negotiate down to Gigabit Ethernet.

On the HP ProCurve, the configuration is pretty understandable. First, the `trunk` command creates the port channel:

	trunk 23-24 Trk1 trunk

Then, the VLAN configuration specifies the same native (untagged) VLAN on the port channel:

	vlan 999  
	name "Trunk-Native"  
	untagged 12,14,20,A2,Trk1  
	no ip address  
	exit

Once the configuration is done, you'll need to insert RJ-45 SFPs (Cisco product number GLC-T, I believe) into the appropriate ports on the Nexus 5000 switch and then cable the switches together. If you didn't make any typos along the way, then you should be good to go!
