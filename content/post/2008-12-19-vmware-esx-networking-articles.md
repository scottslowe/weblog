---
author: slowe
categories: Information
comments: true
date: 2008-12-19T15:59:59Z
slug: vmware-esx-networking-articles
tags:
- Cisco
- CLI
- ESX
- HP
- Networking
- Virtualization
- VLAN
- VMware
title: VMware ESX Networking Articles
url: /2008/12/19/vmware-esx-networking-articles/
wordpress_id: 1101
---

Over the past few years---almost four years now that I've been writing here on this site---I've written quite a few articles on VMware ESX networking. Here's a collection of links to some of the more notable, and hopefully more useful, articles in that group.

We'll start with a classic article, but one that is still the number 1 post on this site. Written in December of 2006, this article on [VMware ESX, NIC teaming, and VLAN trunking][1] still generates lots of traffic even today. In that post, I describe how to use NIC teaming---more precisely, link aggregation or EtherChannel---along with VLAN trunking with VMware ESX. I guess this is a scenario that still causes problems with admins even today.

That first article was written with Cisco Catalyst switches in mind, but earlier this year I also published a version with information on [VMware ESX, NIC teaming, and VLAN trunking with HP ProCurve][2] switches as well.

One key piece to more fully understanding how VMware ESX interacts with VLAN tags is understanding what happens with untagged traffic, and this is something I described in this article on [VMware ESX and the native VLAN][3]. The key thing to remember here is to be sure to configure a VMware ESX port group to capture the untagged traffic, if that's what you desire; otherwise, the traffic will just be ignored.

Similarly, a lot of VMware admins don't fully understand the implications of the various load balancing policies on the vSwitch. The NIC teaming article from December 2006 assumed link aggregation (EtherChannel in the Cisco world), but how did VMware ESX behave in configurations without link aggregation? I tackled that subject in the post on [NIC utilization in VMware ESX][4], and then provided [a follow up post][5] only a couple of months later. Both of these posts provide great information on how VMware ESX will place traffic onto the vSwitch uplinks.

Of course, if you find yourself needing to change the load balancing policy on a vSwitch, you might find this tip on [using the CLI to change the vSwitch load balancing policy][6] helpful, too. And this [CLI guide to modify a port group][7] may also prove useful.

In addition to an in-depth guide to NIC teaming, link aggregation, and VLAN trunking, I also provided a how-to on [configuring VMware ESX, IP-based storage, and jumbo frames][8]. While not officially supported by VMware, many users have reported success with this configuration and seen improved performance. (And before you ask: No, I haven't run any performance comparisons myself yet.)

With the release of VI4--or whatever it ends up being called---there'll be plenty of new networking fodder for me to write about: the Distributed vSwitch, the Cisco Nexus 1000V, and more. In the meantime, though, are there any other networking-oriented articles, guides, or how-to's that readers would be interested in seeing published? I'm open to suggestions.

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2008-09-05-vmware-esx-nic-teaming-and-vlan-trunking-with-hp-procurve.md" >}}
[3]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[4]: {{< relref "2008-07-16-understanding-nic-utilization-in-vmware-esx.md" >}}
[5]: {{< relref "2008-10-08-more-on-vmware-esx-nic-utilization.md" >}}
[6]: {{< relref "2008-09-05-setting-vmware-esx-vswitch-load-balancing-policy-via-cli.md" >}}
[7]: {{< relref "2008-12-16-using-vmware-vim-cmd-to-modify-a-portgroup.md" >}}
[8]: {{< relref "2008-04-22-esx-server-ip-storage-and-jumbo-frames.md" >}}
