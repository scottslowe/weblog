---
author: slowe
categories: Tutorial
comments: true
date: 2007-08-31T09:23:13Z
slug: vlan-interfaces-with-openbsd-41
tags:
- BSD
- ESX
- Networking
- UNIX
- VLAN
- Interoperability
title: VLAN Interfaces with OpenBSD 4.1
url: /2007/08/31/vlan-interfaces-with-openbsd-41/
wordpress_id: 516
---

I've been doing some interoperability testing with [VMware ESX Server](http://www.vmware.com/products/vi/esx/) and VLANs (a separate article on that is in the works), and needed a guest OS that supported VLAN interfaces. From my previous (but limited) experience with [OpenBSD](http://www.openbsd.org/), I suspected that VLAN interfaces were indeed supported, and after setting up a quick VM running OpenBSD 4.1 I found that I was indeed correct. Not only are they supported, they are incredibly easy to setup and configure.

The command to configure a VLAN interface is simply a variation of the standard `ifconfig` command (note that I'm using a backslash to denote a line continuation, so that I can wrap lines here for readability):

	ifconfig <VLAN interface name> vlan <VLAN ID> \  
	vlandev <physical network device>

So, by example, the command I used to create a VLAN interface for VLAN ID 3 looked like this:

	ifconfig vlan3 vlan 3 vlandev pcn0

I did find that I couldn't name the VLAN interface ("vlan3", in this case) anything other than vlanX, where _X_ was a number. I don't know if this is an OpenBSD limitation, or just an error on my part. The latter is certainly a distinct possibility.

Once the VLAN interface, is created, then I just followed the standard OpenBSD way of provisioning an interface---create `/etc/hostname.ifname` (where _ifname_ is the name of the VLAN interface) for each VLAN interface and that should be that.

The ESX Server configuration to support these VLAN interfaces at the guest level was pretty easy, too. I just had to create a port group with a VLAN ID of 4095 and attach the OpenBSD guest to that port group. ESX Server automatically passed the VLAN tags up to the guest and everything worked as expected. (Again, I'll have a separate article on that published soon.)

Next, perhaps I'll try this with Linux or Solaris...
