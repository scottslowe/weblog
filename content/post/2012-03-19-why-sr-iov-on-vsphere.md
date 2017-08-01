---
author: slowe
categories: Musing
comments: true
date: 2012-03-19T17:05:20Z
slug: why-sr-iov-on-vsphere
tags:
- Hardware
- HyperV
- Microsoft
- Virtualization
- VMware
- vSphere
title: Why SR-IOV on vSphere?
url: /2012/03/19/why-sr-iov-on-vsphere/
wordpress_id: 2564
---

Yesterday I posted an article regarding [SR-IOV support in the next release of Hyper-V][1], and I commented in that article that I hoped VMware added SR-IOV support to vSphere. A couple of readers commented about why I felt SR-IOV support was important, what the use cases might be, and what the potential impacts could be to the vSphere networking environment. Those are all excellent questions, and I wanted to take the time to discuss them in a bit more detail than simply a response to a blog comment.

First, it's important to point out---and this was stated in John Howard's original series of posts to which I linked; in particular, [this post](http://blogs.technet.com/b/jhoward/archive/2012/03/13/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-2.aspx)--that SR-IOV is a PCI standard; therefore, it could potentially be used with any PCI device that supports SR-IOV. While we often discuss this in the networking context, it's equally applicable in other contexts, including the HBA/CNA space. Maybe it's just because in my job at EMC I see some interesting things that might never see the light of day (sorry, can't say any more!), but I could definitely see the use for the ability to have multiple virtual HBAs/CNAs in an ESXi host. Think about the ability to pass an HBA/CNA VF (virtual function) up to a guest operating system on a host, and what sorts of potential advantages that might give you:

* The ability to zone on a per-VM basis

* Per-VM (more accurate, per-initiator) visibility into storage traffic and storage trends

Of course, this sort of model is not without drawbacks: in its current incarnation, assigning PCI devices to VMs breaks vMotion. But is that limitation a byproduct of the current way it's being done, and would SR-IOV help alleviate that potential concern or issue? It sounds like Microsoft has found a way to leverage SR-IOV for NIC assignment without sacrificing live migration support (see John's [latest SR-IOV post](http://blogs.technet.com/b/jhoward/archive/2012/03/19/everything-you-wanted-to-know-about-sr-iov-in-hyper-v-part-6.aspx)). I suspect that bringing SR-IOV awareness into the hypervisor---and potentially into the guest OS via each vendor's paravirtualized device drivers, aka VMware Tools in a vSphere context---might go a long way to helping address the live migration concerns with direct device assignment. Of course, I'm not a developer or a programmer, so feel free to (courteously!) correct me in the comments.

Are there use cases beyond providing virtual HBAs/CNAs? Here are a couple questions to get you thinking:

* Could you potentially leverage a single PCI fax board among multiple VMs (clearly you'd have to manage fax board capacity) to virtualize your fax servers?

* Would the presentation of virtual GPUs to a guest OS eliminate the need for a paravirtualized video driver, and would the lack of a paravirtualized video driver streamline the virtualization layer even more? The same goes for virtual NICs.

I'm not saying that all these things are possible---again, I'm not a developer so I could be way off base---but it seems to me that SR-IOV at least _enables_ us to consider these sorts of options.

Regarding networking, this is where I see a lot of potential for SR-IOV. While VMware's networking code is highly optimized, the movement of Ethernet switching into hardware on a NIC that supports SR-IOV has _got_ to free up some CPU cycles and virtualization overhead. It also seems to me that putting that Ethernet switching on an SR-IOV NIC and then adding 802.1Qbg (EVB/VEPA) support would be a sweet combination. Mix in a hypervisor-to-NIC control plane for dynamically provisioning SR-IOV VFs and you've got a solution where provisioning a VM on a host dynamically creates an SR-IOV VF, attaches it to the VM, and uses EVB to provision a new VLAN on-demand onto that NIC. Is that a "pie in the sky" dream scenario? I'm not so sure that it's that far off.

What do you think? Please share your thoughts in the comments below. Where applicable, please provide disclosure. For example, I work for EMC, but I speak for myself.

[1]: {{< relref "2012-03-18-sr-iov-support-in-the-next-version-of-hyper-v.md" >}}
