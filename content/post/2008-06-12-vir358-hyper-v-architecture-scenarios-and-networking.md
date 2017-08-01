---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-12T09:21:56Z
slug: vir358-hyper-v-architecture-scenarios-and-networking
tags:
- HyperV
- Microsoft
- Networking
- TechEd2008
- Virtualization
- Windows
title: 'VIR358: Hyper-V Architecture, Scenarios, and Networking'
url: /2008/06/12/vir358-hyper-v-architecture-scenarios-and-networking/
wordpress_id: 738
---

Day 3 of Tech-Ed 2008 is upon me, and the first session of the day is another session with Jeff Woolsey. This session, VIR358, is titled "Windows Server 2008 Hyper-V Architecture, Scenarios, and Networking." I suspect there will be some duplicate content from [Jeff's Day 1 session][1], and I'll try to weed that out wherever possible. I'm particularly interested in the networking discussions, as I was unable to gather any real information on Hyper-V networking from the Day 1 session or from [my private discussion with Jeff][2].

As the session begins, Jeff reminds everyone of the MAP 3.1 beta. I described MAP in more detail in a [session yesterday][3]. This new version adds some additional functionality and features, primarily around Windows Server 2008 Hyper-V. Jeff went into some additional detail about MAP, but I won't worry about covering that again here.

Jeff's agenda lists a virtualization comparison. I'm guessing that will mean a comparison of Hyper-V with other virtualization solutions. Will that comparison be against other vendors' products?

According to IDC, virtualization penetration is estimated to be only 17% in 2010, up from 5% in 2005.

(The session is _very_ crowded, perhaps the most crowded session I've attended thus far.)

With regards to system requirements, Hyper-V requires hardware assists (Intel VT or AMD-V) and hardware-enabled data execution prevention (DEP; in the form of AMD NX or Intel XD). Without these features, Hyper-V will not operate. Hyper-V is 64-bit also, meaning that you must use x64 processors.

Jeff describes the hypervisor itself as running in "Ring -1", which he explains as less than Ring 0 due to the hardware assists provided by Intel VT or AMD V. This allows child partitions (guest VMs) to run at native Ring 0.

The architecture slide that Jeff takes some time to walk through contains much of the same information as VIR367 on Day 1. Going back to I/O again, Jeff revisits the concept of emulation (used in Virtual Server) vs. synthetic devices. Emulation provides great backward compatibility, but performance was awful. Hyper-V uses "driver enlightenment," or synthetic devices, which leverage VMBus. VMBus is a point-to-point high-speed connection between a child partition and the parent partition. Note that synthetic devices are only available to "enlightened" guest operating systems. You can consider Hyper-V's synthetic devices and their corresponding drivers to be the equivalent of VMware Tools, VI Tools, etc. Some vendors also call these paravirtualized drivers. Virtualization Service Providers (VSPs) and Virtualization Service Clients (VSCs) are part of this synthetic device architecture and VMBus.

The partnerships between Microsoft and Linux vendors (like Novell) allows for enlightened drivers to be available for Linux distributions as well, preventing them from having to use emulation and suffering the performance penalty that results.

Hyper-V features checklist includes support for up to 64GB of RAM per VM, up to 4 logical CPUs per VM, integrated cluster support (this provides both HA and Quick Migration functionality), support for BitLocker (earlier sessions seemed to question Hyper-V support for BitLocker), live VM backups through integration with Volume Shadow Service (VSS), pass-through disk access for VMs, VLAN and load balancing support, and snapshots. For the most part, this puts Hyper-V on par with most other virtualization solutions, with the glaring exception of live migration. Live migration is supported by VMware, XenServer, and Virtual Iron, among others. Microsoft does have an advantage with the VSS support for live VM backups.

Jeff references a white paper due to be published soon that details how to use BitLocker with Hyper-V.

Jeff did cover one slide on Hyper-V security. I won't reproduce that stuff again; refer back to my coverage of Jeff's discussion on Day 1 in VIR367.

Next, Jeff reviews the results of the TAP, RDP, and MSIT deployments. Based on thousands of VMs running on Hyper-V running in production across a variety of industries, Jeff says there have been zero performance blockers, zero deployment blockers, zero application compatibility bugs, and zero scalability blockers. According to Jeff, "the little red phone" that TAP or RDP customers can call if there's a problem hasn't rung even once. He also revisits the use of Hyper-V for the TechNet and MSDN web sites.

Mike Sterling then takes over to provide a demo of Hyper-V. Following the demo of Hyper-V, Mike also provides a brief demo of System Center Virtual Machine Manager (SCVMM) 2008. I've covered that product extensively in [other][4] [sessions][5], so I won't cover that material again here.

Once Mike concludes his demo, Jeff starts into a discussion of networking. Microsoft recommends _at least_ two network adapters; obviously, more would be better. If you are going to use iSCSI, use another dedicated NIC for storage. That brings it up to three NICs at a minimum. I recommend an absolute minimum of three adapters with other virtualization solutions, so this is nothing surprising or unusual. In terms of connecting the NICs, connect one NIC to a management network (this is where the parent partition will communicate) and separate NICs connected to storage and production networks. Only VMs should be exposed to production networks.

We now move into some networking examples. In example 1, we have 4 adapters. One adapter will be assigned to the parent partition for management, and the remaining three NICs will be used for VM networking. Storage in this case will not be iSCSI; it will be Fibre Channel or direct attached. In the Hyper-V configuration, selecting these three NICs for use with VM traffic creates _three_ separate virtual switches. What about NIC bonding for virtual switches?

In example 2, we have 4 adapters again, but this time 1 NIC will be used for iSCSI traffic. This leaves only two NICs for VM traffic. This example shows multiple VMs sharing a single virtual switch, but I still don't see anything with multiple NICs assigned to a single virtual switch.

When looking at the properties for NICs assigned to the parent partition, all the typical components will be bound. Conversely, for NICs assigned to virtual switches, only the Virtual Switch Protocol will be bound to the NIC.

When looking at a VM, emulated NICs will be listed as "Legacy Network Adapter," whereas the new synthetic adapters will be listed simply as "Network Adapter."

If you'd like to run Hyper-V on a laptop (perhaps for demos or testing), Hyper-V does not provide any support for wireless networks. It also doesn't support sleep or hibernation, and multiple spindles (multiple physical hard disks) are highly recommended. You also need a laptop that uses the Santa Rosa chipset or a later chipset. These newer chipsets will allow you to use 4GB of RAM or more in the laptop.

Jeff went through a few more slides, describing his personal laptop configuration (dual-boot Windows Server 2008 and Windows Vista with dual hard drives), a cheap test/dev system, and the overall procedure for creating new VMs. I believe I described the process for creating new VMs earlier, but if you've used a virtualization solution before there's nothing new here. Jeff speaks highly of the rapid deployment capabilities that are possible now with Hyper-V, SCVMM, and VM libraries; I would dare to say this kind of functionality is pretty standard with most every virtualization solution out there. That's not a knock against Hyper-V, just a "level set" that this isn't something that doesn't exist with other platforms. This just brings Hyper-V on the same level with other products.

The next few slides were all material that's already been covered else, like SCVMM, SCOM integration with SCVMM, other System Center components, etc. I won't bore you with all the details again. If there is one thing that I'm tired of hearing here at Tech-Ed this year, it's the story about bringing all of System Center together with Hyper-V. Every single session says it.

The virtualization comparison first compares Hyper-V with Virtual Server 2005 R2, and then moves on to compare Hyper-V with ESX 3.5. I don't necessarily agree with the way in which Jeff makes the comparisons with ESX; for example, he lists Hyper-V as having "unified physical and virtual management" but ESX as having only "virtual management." It's not Hyper-V that provides this functionality; that's System Center Operations Manager. That kind of comparison is, in my opinion, playing loose and fast with the boundaries of the products and related products. I may just have to perform and publish my own comparison...

That wrapped up the session. They gave away three copies of Windows Server 2008, SQL Server, Visual Studio, but I didn't win. Bummer.

[1]: {{< relref "2008-06-10-vir367-hyper-v-security-and-best-practices.md" >}}
[2]: {{< relref "2008-06-10-a-discussion-with-jeff-woolsey.md" >}}
[3]: {{< relref "2008-06-11-vir361-introducing-map-for-windows-server-2008-hyper-v.md" >}}
[4]: {{< relref "2008-06-11-vir253-microsoft-system-center-vmm-2008-part-1-of-2.md" >}}
[5]: {{< relref "2008-06-11-vir360-microsoft-system-center-vmm-2008-part-2-of-2.md" >}}
