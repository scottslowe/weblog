---
author: slowe
categories: Musing
comments: false
date: 2006-05-01T18:46:07Z
slug: virtualization-layers
tags:
- Citrix
- ESX
- Networking
- Virtualization
- VMware
title: Virtualization Layers
url: /2006/05/01/virtualization-layers/
wordpress_id: 237
---

I'd had this article titled "Where Is Virtualization Heading And How This Might Effect The Design And Use Of Citrix And Terminal Servers" flagged in [NetNewsWire](http://ranchero.com/netnewswire/) for quite some time now, with the idea of going back and reading the full article. (I'll have to explain my use of RSS feeds sometime.) In any case, I finally took the time to read the full article, and here are my thoughts.

The article (found [here](http://geekswithblogs.net/wallabyfan/archive/2006/04/19/75624.aspx)) draws from an [earlier article by Brian Madden](http://www.brianmadden.com/content/content.asp?ID=566) (here are [my earlier comments][1] on this same article) that discusses the various ways in which applications can be delivered to end-users. In that article, the idea of using [VMware](http://www.vmware.com/) to provide virtualized desktop sessions (something VMware is now pursuing with its [Virtual Desktop Infrastructure Alliance](http://www.vmware.com/news/releases/vdi.html)).

In this particular article, the author (I don't even know the author's name!) wonders if [SystemGuard](http://www.softgrid.com/products/virtualization.asp) (the application virtualization technology used by [Softricity](http://www.softricity.com/) in their SoftGrid product) can somehow reduce the layers of virtualization in the scenario proposed by Brian Madden so as to improve performance and/or resiliency.

It's an interesting idea, and I believe that the desire to improve performance is what's driving the rise of paravirtualization products (such as [Xen](http://www.cl.cam.ac.uk/Research/SRG/netos/xen/) and [SWsoft Virtuozzo](http://www.swsoft.com/en/products/virtuozzo/)) that don't virtualize the entire OS and typically have lower overhead, as well as the introduction of virtualization support into the hardware (via Intel VT and AMD Pacifica).

So, instead of using VMware to provide virtual desktops, perhaps a paravirtualization product is more applicable? That _may_ (stress the "may," since VMware's flagship product [ESX Server](http://www.vmware.com/products/esx/) is an exception to the rule that full virtualization products typically have higher overhead and lower performance) be helpful for performance, but it doesn't address the increased resiliency the author is really seeking. The article states:

>What I'm imagining here is that instead of Logging on to a traditional Server, you are making a call from the thin client to a "undefined at present" middle layer component that is able to "auto start" an Application encased in a SystemGuard Shell that could then be run on say a Server 2003 x64 System by virtue of a VMware/VMotion layer.

This is followed by:

>If this middle layer component was able to deliver some reasonable level of Load Balancing then we could almost take a hit on the resilience, because the redundancy would now be based not on Servers, Sessions or Users but at the individual Application level.

Unfortunately, even [VMotion](http://www.vmware.com/products/vc/vmotion.html) today lacks the ability to recover from a failed host server, although if I recall correctly that kind of "clustered host" functionality may be coming in ESX Server 3.0 (now in beta). To achieve the kind of resiliency the author is seeking we need more than application virtualization (like that provided by SystemGuard) and more than OS virtualization (like that provided by Xen, VMware, or Virtuozzo) and we need a way of maintaining state across multiple systems. This would ensure that the failure of one server (because even the author's ideal scenario has the user logging in to some sort of server) does not impact the applications running on that server---their state is being maintained across a group of servers that can seamlessly take up the slack. Sounds like a cluster to me!

Once we find this way to maintain state across multiple servers, then we can go easily bring together technologies for delivering applications (like [Citrix Presentation Server](http://www.citrix.com/English/ps2/products/product.asp?contentID=186)) and technologies for managing applications (like SoftGrid and SystemGuard) to create the ideal managed desktop environment that many organizations are seeking.

[1]: {{< relref "2006-03-15-synergies.md" >}}