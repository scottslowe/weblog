---
author: slowe
categories: Explanation
comments: true
date: 2009-11-03T09:57:37Z
slug: fcoe-and-vlan-trunking-on-nexus-5000
tags:
- Cisco
- FCoE
- Networking
- Nexus
- Storage
- VLAN
title: FCoE and VLAN Trunking on Nexus 5000
url: /2009/11/03/fcoe-and-vlan-trunking-on-nexus-5000/
wordpress_id: 1722
---

In my earlier post on [how to configure FCoE on a Nexus 5000][1], one of the readers suggested in the comments that it was necessary to have the interfaces in VLAN trunk mode via the `switchport mode trunk` command. I didn't pay that much attention to it because the interfaces were indeed in VLAN trunk mode.

Fast forward to yesterday, when I was troubleshooting a problem between a Gen2 QLogic CNA and the Nexus 5010 in my lab (I [tweeted](https://twitter.com/scott_lowe/statuses/5371829429) about it). Although the Ethernet side of the CNA works just fine, the CNA refuses to bring up an FCoE connection. In the process of troubleshooting, Brad Hedlund (check his [outstanding web site](http://www.internetworkexpert.org/)) suggested to me in a Twitter direct message that I should double-check the VLAN trunking status of the interface. That part I'd already heard from the reader who commented on the first post, but the next part was new to me (emphasis mine):

>Gen2 requires 'switchport mode trunk' on the 5K. _**Gen1 doesn't.**_ Also make sure FCoE VLANs are allowed on the trunk.

Ah, now there's something I hadn't heard! That prompted me to do a bit of testing this morning (yes, I know I'm supposed to be studying for the VCDX Design Exam this afternoon). In my testing, I confirmed that a Gen1 CNA (I'm using Gen1 Emulex CNAs) **does not require VLAN trunking** to be enabled on the Ethernet interface.

There does appear to be a "gotcha" though: if the Ethernet interface is in access mode, it's access VLAN **must be the same** as the FCoE VLAN; otherwise, the `vfc` interface will report down.

In summary:

* If you are using a Gen2 CNA, you must put the Ethernet interface in VLAN trunk mode.

* If you are using a Gen1 CNA, the Ethernet interface may be in either access mode or trunk mode.

* If the interface is in trunk mode, be sure that you have allowed the FCoE VLAN via the `switchport trunk allowed vlan` command.

* If the interface is in access mode, be sure that you have placed the interface in the FCoE VLAN via the `switchport access vlan` command.

If there are any other subtleties or nuances I've missed, please post them in the comments below so that future readers will benefit. Thank you!

[1]: {{< relref "2009-10-25-setting-up-fcoe-on-a-nexus-5000.md" >}}
