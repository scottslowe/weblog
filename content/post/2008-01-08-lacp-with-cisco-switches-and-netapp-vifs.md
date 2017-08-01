---
author: slowe
categories: Tutorial
comments: true
date: 2008-01-08T23:19:43Z
slug: lacp-with-cisco-switches-and-netapp-vifs
tags:
- Cisco
- Interoperability
- IOS
- NetApp
- Networking
- ONTAP
title: LACP with Cisco Switches and NetApp VIFs
url: /2008/01/08/lacp-with-cisco-switches-and-netapp-vifs/
wordpress_id: 602
---

In my previous article about using [NetApp multi-mode VIFs with Cisco switches][1], I mentioned that you could---at that time---only use 802.3ad static link aggregation:

>Be aware that Data ONTAP's multi-mode VIFs are only compatible with static 802.3ad link aggregation; you can't use PAgP (Cisco proprietary protocol). I would assume dynamic LACP is also incompatible. For this reason we used the `channel-group 1 mode on` statement instead of something like `channel-group 1 mode desirable`.

I recently got some feedback from a NetApp SE in my area; this SE informed me that Link Aggregation Control Protocol (LACP, part of the IEEE 802.3ad specification) is indeed supported with Data ONTAP version 7.2. This [KB article](https://now.netapp.com/Knowledgebase/solutionarea.asp?id=kb20148) on the NetApp NOW site (login required) indicates that ONTAP 7.2.1 is required in order to use a LACP VIF.

There are a couple important requirements to note; these are laid out in the referenced KB article:

1. Dynamic multimode VIFs should use IP address-based load balancing. This means that the Cisco switch or the channel group must also use IP address-based load balancing.

2. Dynamic multimode VIFs must be first-level VIFs. This makes sense; LACP is a Layer 2 protocol, so layering a LACP VIF on top of other VIFs just doesn't work.

To create the dynamic multimode VIF on the Data ONTAP side, the command is pretty simple:

	vif create lacp <vif name> -b ip <interface list>

On the Cisco side, the commands are very similar:

	s3(config)#int port-channel1  
	s3(config-if)#description LACP multimode VIF for netapp1  
	s3(config-if)#int gi0/23  
	s3(config-if)#channel-protocol lacp  
	s3(config-if)#channel-group 1 mode active

These commands would be repeated for all physical ports that should be included in the LACP bundle. Note the differences from the earlier commands in the previous article; here we use `channel-group 1 mode active` instead of `channel-group 1 mode on`. We also added the `channel-protocol lacp` command.

Together, these commands will establish a LACP-based link aggregate between a NetApp storage system running Data ONTAP version 7.2.1 or higher and a Cisco IOS-based switch.

Thanks to Jeff, our NetApp SE, for providing the updated information.

[1]: {{< relref "2007-06-13-cisco-link-aggregation-and-netapp-vifs.md" >}}
