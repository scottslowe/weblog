---
author: slowe
categories: News
comments: true
date: 2007-06-15T23:31:07Z
slug: moving-esx-into-firmware
tags:
- ESX
- Hardware
- Virtualization
- VMware
title: Moving ESX Into Firmware
url: /2007/06/15/moving-esx-into-firmware/
wordpress_id: 473
---

The rumor that a slimmed down version of [ESX Server](http://www.vmware.com/products/vi/esx/), supposedly called "ESX Lite," is being developed for placement into a server's firmware, is circulating the Internet (see [here](http://www.cmswire.com/cms/virtualization/embedded-esx-lites-allegedly-acooking-at-vmware-001377.php) or [here](http://www.thincomputing.net/comment.php?comment.news.3386)). Most of these reports link back to [a story on SearchServerVirtualization](http://searchservervirtualization.techtarget.com/originalContent/0,289142,sid94_gci1260992,00.html) which quotes sources close to VMware stating:

>According to several sources close to VMware, ESX Lite is real and currently under development. The new lightweight hypervisor would be installed directly on the motherboard, simplifying the deployment of an ESX host and ensuring 100% hardware integration.

Virtualization.info adds that the rumor is [apparently confirmed](http://www.virtualization.info/2007/06/vmware-esx-server-lite-edition-coming.html).

This is a smart move. It's smart because it derails Microsoft's attempts to marginalize the hypervisor by bundling it with the operating system (via Windows Server Virtualization, aka "Viridian"). It's smart because it expands the hypervisor market in new directions that no one else has yet tapped, helping VMware retain mindshare about its technical leadership and innovation. It's smart because it's the hardware vendors that have the most to lose via virtualization, and by partnering with them you remove potential future opponents.

However, it's also a very risky move. What if "ESX Lite" doesn't (or can't) perform as well as "full" ESX Server? What if embedding the hypervisor into a server's firmware causes VMware to lose visibility? After all, customers wouldn't be buying solutions from VMware any longer, because these would be integrated with the hardware. It would be prudent for VMware to create a kind of "VMware Inside"-type program that maintains their visibility in the overall solution, even though it's all being purchased through [Dell](http://www.dell.com/), [HP](http://www.hp.com/), or [IBM](http://www.ibm.com/).

It will be very interesting to see how this plays out, and which of the hardware vendors are "on board" with the effort.

**UPDATE:** In a recent blog posting, Gordon Haff agrees with me regarding this approach by VMware as a [move to forestall](http://www.illuminata.com/perspectives/?p=330) the "built in" virtualization functionality that exists or will soon exist in most operating systems.
