---
author: slowe
categories: Musing
comments: true
date: 2009-08-04T23:02:17Z
slug: netronome-and-io-virtualization
tags:
- Hardware
- Networking
- Virtualization
title: Netronome and I/O Virtualization
url: /2009/08/04/netronome-and-io-virtualization/
wordpress_id: 1511
---

At what point does a product become a virtualization product? With all the hype and the buzz around virtualization---predominantly because of VMware's success---every company is hopping on the "virtualization bandwagon" and releasing a new "virtualization product". In most cases, these products are simply repackaged versions of their existing products, perhaps with a slight improvement here or there to justify the virtualization moniker.

Of course, virtualization is more than just machine virtualization such as that offered by VMware, Microsoft Hyper-V, and Citrix XenServer. Virtualization is about inserting layers of abstraction between different computing resources, and there are other valid forms of virtualization. Take application virtualization, for example. Products such as ThinApp and App-V insert a layer of abstraction between applications and the underlying operating system.

A couple of weeks ago I had the opportunity to speak with [Netronome](http://www.netronome.com/), a small company developing a 40-core processor called the Network Flow Processor (NFP). Based on technology acquired from Intel, Netronome touts the NFP as an I/O virtualization product. But is this another instance of repackaging an existing product with the virtualization moniker just to capitalize on a sales opportunity, or is the NFP truly an I/O virtualization product? Like a lot of other products, it's really hard to say.

Netronome's upcoming product, the NFP-3200, is a hardware product that they intend to OEM to leading server manufacturers. The idea is that the NFP will be embedded into the server design to allow networking I/O functions to be offloaded to the NFP. Much ado has been made about hypervisor bypass (aka VMDirectPath), but rather than removing the software switch, why not put the switch into hardware? That would be one such function offered by the NFP. The NFP could provide line-rate switching between physical ports, virtual NICs, or any combination of the two. This approach with the NFP-3200 is almost like the best of both worlds---no "hypervisor overhead" for software switching but all the benefits of a local switch in each host.

Of course, the Netronome folks are also talking about all kinds of other functionality made possible by the NFP-3200, like flow classification, firewalling, load balancing, etc., all running at line speed inside the NFP. Of course, the keys are a) getting one or more major server manufacturers on board with the idea; and b) getting one or more hypervisor vendors on board with the idea.

Netronome wasn't willing to share any information on the former (naturally), but with regard to the latter they did indicate that much of this functionality will be available on open source Xen almost immediately upon the release of the NFP-3200. That's nice, but it doesn't really accomplish a whole lot. They need to get VMware and Microsoft (more VMware than Microsoft right now) to jump on this idea.

Aside from the lack of widespread hypervisor support, my only other complaint with Netronome's approach is that it isn't standards-based. Rather than leveraging SR-IOV, the PCI SIG standard for PCIe-based device virtualization, Netronome is leveraging what they call Software Configurable Virtual Functions (SCVF). I understand the benefits they see out of using SCVF instead of relying upon SR-IOV, but I'd still prefer to see more innovation upon standards instead of innovating without standards. But what do I know? It might be that PCI SIG adopts SCVF as their future standard. Stranger things have happened...

Anyway, for more information visit the Netronome web site. If they can pull off support from one or more major server manufacturers and one or more major hypervisor vendors, the NFP-3200 could make the virtualization scene very interesting indeed.
