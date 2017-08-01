---
author: slowe
categories: Education
comments: true
date: 2015-06-01T08:30:00Z
tags:
- Linux
- Networking
- CLI
- Cumulus
title: Some Cumulus Linux Networking Concepts
url: /2015/06/01/cumulus-networking-concepts/
---

As I've recently had the opportunity to start working with [Cumulus Linux][link-1] (running on a [Dell S6000-ON][link-2] switch), in this post I wanted to share a few concepts I've learned about networking with Cumulus Linux.

I'm not a networking guru, but I'm also not new to configuring network equipment---I've configured [GRE tunnels on a Cisco router][xref-1], [set up link-state tracking][xref-2], and [enabled jumbo frames on a Nexus 5000][xref-3] (to name a few examples). I've worked with Cisco gear, HP equipment, Dell PowerConnect switches, and Arista EOS-powered switches. However, as a full distribution of Linux, networking with Cumulus Linux is definitely different from your typical network switch. To help make the transition easier, I'll share here a few things I've learned so far.

It's important to understand that Cumulus Linux isn't just a "Linux-based network OS"---it's _actually_ a full Linux distribution (based on Debian). Lots of products are Linux-based these days, but often hide the full power of Linux behind some sort of custom command-line interface (CLI) or shell. Not so in this case! I think this fact is perhaps a bit easy to overlook, but it shapes everything that happens in Cumulus Linux:

* Every port in a Cumulus Linux-powered switch are treated as a Linux network interface. The `eth0` interface is the management port, as expected, but so are all the front-panel ports. (On my S6000-ON, the ports show up as `swp1` through `swp32`.) Viewing the status of a port, then, is done via `ip link list <interface>`, just like on any other system running Linux.
* Want to set an interface to "admin up" status? `ip link set <interface> up` is what you'll use (or `ip link set <interface> down` to administratively disable an interface).
* Need to configure an interface? As with other Debian Linux-derived products, just configure the interface in `/etc/network/interfaces`. Cumulus also supports the use of individual files in the `/etc/network/interfaces.d` directory to configure interfaces (a technique I personally prefer and also utilize on my Ubuntu-based servers).
* With a typical network switch, all the ports are automatically part of the same Layer 2 bridge: you plug something into two ports on a Cisco or Arista switch, and there's automatically Layer 2 connectivity. Not so with a Cumulus Linux-powered switch, and this is something that threw me off at first. Because every port is a Linux network interface, you have to explicitly set up Layer 2 (bridged) and Layer 3 (routed) connectivity between interfaces---just like any other system running Linux.
* To establish Layer 2 connectivity, you must create a Linux bridge and add ports (interfaces) to the bridge. If you want all the ports on the switch to be active in the same Layer 2 broadcast domain, then create a single bridge and add all the ports.
* To establish Layer 3 connectivity, you must provide IP configuration for the ports (interfaces) and enable/configure routing between them.
* VLANs can be supported by multiple bridges and VLAN subinterfaces, or (as of Cumulus Linux 2.5, I believe) a _VLAN-aware bridge_. The former method is a more "traditional Linux" configuration, but the latter configuration will likely make more sense to former networking professionals. 

I know this is high-level stuff, and a bit abstract. I plan to publish more detailed posts on many of these topics in the near future, but wanted to go ahead and get these initial thoughts and "lessons learned" posted. If there are specific things about Cumulus Linux you're interested in hearing about, feel free to [hit me up on Twitter][link-3]---I'll do my best to cover all reasonable requests. In the meantime, stay tuned for more Cumulus Linux-centric posts coming soon.


[link-1]: http://cumulusnetworks.com
[link-2]: http://www.dell.com/us/business/p/open-networking-switches/pd
[link-3]: https://twitter.com/scott_lowe

[xref-1]: {{< relref "2006-01-27-gre-tunnels-on-a-cisco-router.md" >}}
[xref-2]: {{< relref "2007-06-22-link-state-tracking-in-blade-deployments.md" >}}
[xref-3]: {{< relref "2009-11-09-enabling-jumbo-frames-on-a-nexus-5000.md" >}}
