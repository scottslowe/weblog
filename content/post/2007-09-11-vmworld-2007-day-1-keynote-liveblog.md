---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-11T11:52:39Z
slug: vmworld-2007-day-1-keynote-liveblog
tags:
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 Day 1 Keynote Liveblog
url: /2007/09/11/vmworld-2007-day-1-keynote-liveblog/
wordpress_id: 532
---

There's a great deal of buzz surrounding the VMworld 2007 Day 1 keynote address, in which VMware is expected to make a number of product announcements as well as shed some additional details on product announcements already made, such as [ESX Server 3i](http://www.vmware.com/company/news/releases/esx3i.html), [VMware Site Recovery Manager](http://www.vmware.com/company/news/releases/srm.html), and [Virtual Desktop Manager 2.0](http://www.vmware.com/company/news/releases/vdm.html). I mentioned all these things [in a post yesterday][1], after completing the first half of Partner Day. Today, I'll be trying to liveblog the keynote, so stay tuned for additional details and information as it is presented.

## 7:50 AM

The conference hall is still filling up. This year the hall for the keynote looks significantly larger than last year's venue. A few other bloggers are already here Eric Sloof of [ntpro.nl](http://www.ntpro.nl/blog), Jim Jones of [vmwarez.com](http://www.vmwarez.com/), Bob Plankers of [lonesysadmin.net](http://lonesysadmin.net), among others. (If I didn't list you, don't be offended!)

## 8:00 AM

We're still waiting for the keynote to start. The conference hall filled pretty quickly; right now, it looks like the only spots left are at the far sides of the room, away from any good view of the stage and the numerous projection screens. While waiting for the keynote to start, I've had a great conversation with Jim Jones (of vmwarez.com) about VMware's new [SMB focus](http://www.vmwarez.com/2007/09/smb-focus-at-vmworld-2007.html). We both agree that ESX Server 3i is going to make a difference in the SMB market, if only because of the simplicity.

I also just met Viktor, who's publishing a [photo stream of VMworld 2007](http://www.flickr.com/photos/viktorious/sets/72157601844423001/) via Flickr; go have a look!

About 8:08, the general session started with a heavy duty video presentation and some live music on the stage. The music was backlit by dramatic natural photos and overlaid with keywords that VMware is driving home---Production Proven, Industry Standard, Better Computing Environment, etc. Rather nicely done, in my opinion.

Karthik Rau next took the stage, serving as the Master of Ceremonies for the session. Karthik announced that representatives from both Intel and AMD would be participating in today's keynote address. He first introduced Diane Greene, President and CEO of VMware.

## Diane Greene, President and CEO, VMware

Diane spoke first about how virtualization is driving a "complete infrastructure refresh". She went on to state that the hypervisor is not the same as the operating system, and that virtual infrastructure is not the same as the hypervisor. I would imagine that these statements are intended to deflect competition from Microsoft and Xen, respectively.

Diane announced again ESX Server 3i, a version of ESX Server that strips away the Service Console, reduces the memory footprint and disk footprint, and allows the hypervisor to run in as little as 32MB. According to Diane, it also makes VMware's hypervisor the only hypervisor that does not have any OS dependencies. (I'd be interested to know how the embedded Xen hypervisor has OS dependencies. Any Xen experts care to comment?) Diane then introduced Mark Jarvis from [Dell](http://www.dell.com/) (Dell's Chief Marketing Officer) to introduce the first ESX Server 3i implementation, the Dell VESO. They went on to boot, on screen, a Dell server running ESX Server 3i.

Before Mark even finished talking about the hardware, ESX Server 3i had already finished booting. The Dell appliance was specifically designed around virtualization. It has more memory slots to accommodate more RAM, double the number of expansion slots, and features the new AMD quad-core "Barcelona" CPUs.

The demo proceeded to use a Remote Console (looked like the VI Client) to find a VM stored on an iSCSI SAN and power it on. They then went on to show off the CIM standard interface for the hardware vendors to use to interface with the hardware partner's underlying hardware. The new Dell VESO system is expected to be available at the end of November. Other vendors that are expected to provide support for ESX Server 3i include IBM (with their X4-based systems), HP, Fujitsu Siemens, and NEC. There were also a number of video from beta users of the ESX Server 3i product.

Diane discussed the announcement of OVF, an open standard created by VMware after work with Microsoft and Xen and which will be submitted to the DMTF. VMware believes that the standardization of the containers in which virtual machines are stored will enable a number of new solutions.

VMware Site Recovery Manager is apparently based on OVF and is enabled by OVF to provide workflow and automation. Diane used Bowdoin and Loyola Marymount as an example of using VMware for disaster recovery, although she stopped short of stating whether these universities were using VMware Site Recovery Manager.

Almost as an aside, Diane also mentioned Virtual Desktop Manager, VMware's new connection broker that is expected at the end of 2007.

The discussion next moved into virtual appliances, particuarly discussing the LiquidVM appliance from BEA, which strips away everything except those components necessary to run Java applications. She mentioned again the program from PG&E; to rebate customers who implement virtualization. Two more California-based utilities have already signed on, and 20 more utilities companies are in the process of launching similar programs. Diane then wrapped up her keynote and introduced Pat Gelsinger, Senior VP for Digital Enterprise Systems at Intel.

## Pat Gelsinger, Senior VP for Digital Enterprise Systems, Intel

Pat spent quite a bit of time discussing Intel's various areas of focus, how those areas map against key IT initiatives and the business needs that those IT initiatives are designed to address. Some of the specific technologies he mentioned include FlexMigration (the ability to spoof processor IDs to improve live migration compatibility), FlexPriority, EPT (Extended Page Tables), virtual processor IDs, Intel VT for Directed I/O, and Intel VT for Connectivity. All these are designed to enhance virtualization and improve virtualization's ability to help satisfy IT initiatives.

Intel VT-d (Intel VT for Directed I/O) provides DMA and interrupt remapping. This is supposed to be available on all Intel platforms in 2008.

VMDq (Virtual Machine Device queues) is another technology that provides multiple send/receive queues and the ability to pre-sort incoming packets. This will reduce CPU utilization and improve throughput on network and storage devices. Intel expects to see, based on prerelease tests, as much as a 100% increase in I/O performance.

Regarding energy efficiency, Intel and Google founded the Climate Savers industry effort to reduce power consumption, and VMware has now joined that effort. Intel is going to further provide functionality to the VMM (the hypervisor), not only to provide better per-node power management but also to enable datacenter-wide power management. This would enable scenarios in which we live migrate workloads and power down idle hosts as workload demands decrease.

The Intel Quad-Core Xeon 7300 was demoed (supposed to be included in the HP ProLiant BL680c), and some benchmarks shared about this platform and how it compares to prior Intel CPU platforms.

## Hector Ruiz, Chairman and CEO, AMD

Diane next introduced Hector Ruiz of AMD. He discussed the functionality being added to the Opteron family to ease VMotion compatibility issues across the entire Opteron family.

Moving on to power consumption, Hector discussed the Green Grid, founded by VMware and AMD, and discussed the results of a power consumption study. In his words, "it's time to own up to our responsibility to address global climate change." By 2010, 70% of every dollar spent on servers will be spent on providing power and cooling for those servers. AMD itself consolidated their Austin, TX, data center from 117 servers to just 9 servers running ESX Server, resulting in a 79% reduction in power consumption.

The AMD quad-core Opteron processors were officially launched yesterday, and are specifically optimized for virtualization. Leendert van Doorn, a Senior Fellow with AMD, took the stage along with Hector.

Leendert demonstrated how the new quad-core Opteron currently features nested page tables (dubbed by AMD as "Rapid Virtual Indexing, or RVI) increases performance and helps reduce the size of the VMM. That, in turn, allows for greater usage of VMMs in more scenarios, on more hardware. He envisioned the possibilities that could be enabled by having VMMs everywhere, in every computer. Leendert also envisioned pervasive VMMs as a key technology to enable AMD's "50 by 15" initiative, which seeks to bring Internet access to 50% of the world population by 2015.

Hector's keynote was heavily focused on both personal and corporate responsibility, true and open partnerships, and continued innovation. The keynote was short on product announcements, but I personally enjoyed it.

And that's it! Karthik Rau took the stage again to recognize other VMworld 2007 Platinum Sponsors and wrap up the general session for Day 1.

I'll post more information here as soon as I can. Today is my busiest day the entire week, so updates may be a bit on the light side.

[1]: {{< relref "2007-09-10-vmworld-2007-partner-day.md" >}}
