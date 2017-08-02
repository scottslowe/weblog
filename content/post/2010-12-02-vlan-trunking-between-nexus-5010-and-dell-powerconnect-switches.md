---
author: slowe
categories: Explanation
comments: true
date: 2010-12-02T09:00:00Z
slug: vlan-trunking-between-nexus-5010-and-dell-powerconnect-switches
tags:
- Cisco
- Networking
- VLAN
title: VLAN Trunking Between Nexus 5010 and Dell PowerConnect Switches
url: /2010/12/02/vlan-trunking-between-nexus-5010-and-dell-powerconnect-switches/
wordpress_id: 2169
---

If you don't work in the networking space on a regular basis, it's easy to overlook interoperability issues between equipment from different vendors. After all, a VLAN trunk is a VLAN trunk is a VLAN trunk, right? Alas, the answer is not always quite so simple.

While standards such as 802.1Q promise easy interoperability, the devil is usually in the details. I ran into just this sort of problem today in the lab. Specifically, I had a need to trunk VLANs between a pair of Cisco Nexus 5010 switches and a pair of Dell PowerConnect 6248 switches.

The configuration on the Cisco Nexus side was pretty straightforward (note that this was one of the first eight ports on the switch and was throttled down to 1Gbps):

	interface ethernet 1/1  
	switchport mode trunk  
	speed 1000

I tried replicating this same setup on the PowerConnect switches using Dell's `switchport mode trunk` command. Unfortunately, it didn't work. I kept digging around, but regardless of the configuration the `show interfaces switchport ethernet` command would always show that VLAN 1 was marked as tagged. This clearly wouldn't work; since VLAN 1 was defined as the native VLAN on the Cisco Nexus switch, it would be untagged on the Cisco side. I needed the VLAN to be untagged on the Dell side as well.

Quite by accident, I stumbled upon a slightly different command on the Dell: the `switchport mode general` command. The help text in the interface indicated that this was the correct configuration for 802.1Q operation.

I modified the Dell PowerConnect to use this configuration:

	interface ethernet 1/g47  
	switchport mode general  
	switchport general allowed vlan add 1 untagged  
	switchport general allowed vlan add 900 tagged

With this configuration, the `show interfaces switchport ethernet` command now reported that VLAN 1 was untagged, as shown in the screenshot below.

![Dell VLAN configuration](/public/img/dell-vlan-cfg.png)

A quick connectivity test showed that traffic was now flowing properly between the Dell PowerConnect 6248 switches and the Cisco Nexus 5010 switches. Problem resolved! Key takeaway: use `switchport mode general` for interoperability with other vendors' switches.

If you have any experience with Dell PowerConnect switches and have additional information to share, please post it in the comments below.
