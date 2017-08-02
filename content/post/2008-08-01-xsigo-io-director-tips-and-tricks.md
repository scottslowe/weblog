---
author: slowe
categories: Information
comments: true
date: 2008-08-01T09:00:15Z
slug: xsigo-io-director-tips-and-tricks
tags:
- Hardware
- Networking
- SAN
- Virtualization
- VMware
- Xsigo
title: Xsigo I/O Director Tips and Tricks
url: /2008/08/01/xsigo-io-director-tips-and-tricks/
wordpress_id: 774
---

For the last few weeks, I've had the privilege of using a [Xsigo Systems VP780 I/O Director](http://www.xsigo.com/products/io_director.html) in my lab. After using the product for these past few weeks, I thought I'd share a few tips and tricks for integrating the product into your VMware Infrastructure 3 (VI3) environment.

## Jumbo Frames

If you want to use jumbo frames in your VI3 environment with the VP780, you'll need to set the MTU on the Ethernet ports _before_ creating any vNICs. If any vNICs are already created, you'll need to remove them, set the MTU, and then re-add them. Fortunately that's no big deal with the VP780, but it is something to keep in mind. The command to set the MTU looks something like this:

	set ethernet-port 13/6 -mtu=9000

Obviously, you'll need to specify the appropriate Ethernet port instead of 13/6, and if you want to use an MTU size of something other than 9000 bytes you'll need to specify the correct value there as well.

## VLAN Trunking Mode

Like with jumbo frames, if you anticipate wanting to pass VLAN tags through the VP780 up to the ESX server, you'll want to set the Ethernet port's VLAN trunking mode _before_ creating vNICs. Otherwise, as stated in the previous section, you'll need to remove the vNICs, set the trunk mode, and then re-add the vNICs. I consider this an essential step, as I almost always recommend the use of VLANs in a VI3 environment.

The command for setting the VLAN trunking mode is very similar to the MTU command shown earlier:

	set ethernet-port 13/6 -mode=trunk

It's perfectly OK to combine these two commands into a single step:

	set ethernet-port 13/6 -mode=trunk -mtu=9000

While you're in there setting properties for the Ethernet ports, you may also want to consider whether you want the VP780 to tag native VLANs. I'll refer you back to [this blog entry][1] and [this blog entry][2] for more information on VLANs and the native VLAN with ESX. Alternatively, you could just use [this link][3] to see all VLAN-related articles.

## Using esxtop with the VP780

This one had me stumped, but Xsigo support found the problem pretty quickly. Originally, I was unable to use `esxtop` to view network statistics. It turns out that, as documented by VMware, ESX has a maximum number of NICs that are supported in a host. Since the Xsigo drivers pre-create 32 vNICs, this was causing the host to exceed the maximum and `esxtop` would simply quit when trying to view network statistics. The fix is to unload the Xsigo module, then reload it and pre-create fewer NICs, like this:

	vmkload_mod -u vnic  
	vmkload_mod vnic vmk_preregister=26  
	esxcfg-boot -r

With this change in place, `esxtop` worked just fine. Obviously, you'll need to adjust the number of pre-created vNICs according to the number of other NICs in the system so that the system-wide total does not exceed 32.

By the way, if you'd like more information on I/O virtualization and why it might be beneficial, have a look at a couple of articles I've written about the topic:

[Benefiting from I/O Virtualization](http://searchservervirtualization.techtarget.com/tip/0,289483,sid94_gci1310580,00.html)  

[Maximizing I/O Virtualization](http://searchservervirtualization.techtarget.com/tip/0,289483,sid94_gci1322087,00.html) _(New!)_

It's quite likely I'll have more Xsigo-related tips in the future, so if you're interested, be sure to stay tuned. The easiest way to do that would be to subscribe to the site's RSS feed, which is easily accessible via the bright orange logo in the upper right corner of the site.

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[3]: /tags/vlan/
