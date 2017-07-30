---
author: slowe
categories: Musing
comments: true
date: 2007-04-05T21:07:31Z
slug: open-source-virtualization-thoughts
tags:
- ESX
- OSS
- Virtualization
- VMware
title: Open Source Virtualization Thoughts
url: /2007/04/05/open-source-virtualization-thoughts/
wordpress_id: 436
---

Edward Aractingi started it all back on March 20 when he blogged about [why VMware should open source ESX Server](http://edward.aractingi.net/blog/archives/2007/03/why_should_vmware.html). Tarry Singh then [weighed in on the matter](http://tarrysingh.blogspot.com/2007/03/should-vmware-open-source-esx-server.html) from his weblog. Both men make very good points on the matter.

It's true that there is a lot of virtualization work being done in the open source community. We have the [Xen hypervisor](http://www.cl.cam.ac.uk/research/srg/netos/xen/), now capable of hosting unmodified guest operating systems through the hardware-assisted virtualization support of the newest [Intel](http://www.intel.com/) and [AMD](http://www.amd.com/) CPUs; we have the inclusion of [KVM](http://kvm.qumranet.com/kvmwiki) in the Linux kernel and the [addition of VMI](http://www.eweek.com/article2/0,1895,2107818,00.asp) into the next stable kernel; and projects such as [OpenVZ](http://openvz.org/) thriving as well. That's a lot of activity going on around virtualization and virtualization-related technologies. And, while it's most definitely not open source, we also must consider the impact of "Viridian," Microsoft's hypervisor to be release shortly after Windows Server 2007 (aka "Longhorn").

The real question comes to this: will open source commoditize the hypervisor? If you agree that the introduction of open source hypervisors such as Xen will commoditize the hypervisor, then VMware's future needs to lie with other technologies, such as the management layer and value-added functionality such as live migration ([VMotion](http://www.vmware.com/products/vi/vc/vmotion.html)), dynamic load balancing ([VMware DRS](http://www.vmware.com/products/vi/vc/drs.html)), and high availability ([VMware HA](http://www.vmware.com/products/vi/vc/ha.html)). In that scenario, VMware would be better served to open source the [ESX Server](http://www.vmware.com/products/vi/esx/) code and allow the community to drive development of the hypervisor itself. I think that's a viable model, one that has been embraced by other organizations with varying degrees of success.

If, on the other hand, you _don't_ think that the hypervisor will become a commodity, then the idea of open sourcing ESX Server doesn't really hold a lot of value. Why release your competitive advantage? Instead, you continue to develop the hypervisor and add features and functionality to it to differentiate it from the competitors.

What do you think? Will the hypervisor become a commodity? I think it's a bit too early to tell. Open source aficionados point to the success of Linux and tell you that the OS is becoming a commodity, but look at the reality of the [sales numbers for Windows Vista](http://www.microsoft.com/presspass/features/2007/mar07/03-26VistaDebut.mspx). Perhaps the OS is becoming a commodity, but has anyone bothered to tell people buying [Windows Vista](http://www.microsoft.com/windows/products/windowsvista/default.mspx)? Linux has had years to make "the proprietary OS history", and is only now starting to really have an effect. Will open source virtualization efforts take the same time? If so, VMware has plenty of time to decide the course of action to take. In the meantime, I think that VMware has done a reasonably good job of blending open source code, proprietary technologies, and published standards into their products. If they can continue to find the right balance between these often contradictory positions, I think they'll continue to be successful.
