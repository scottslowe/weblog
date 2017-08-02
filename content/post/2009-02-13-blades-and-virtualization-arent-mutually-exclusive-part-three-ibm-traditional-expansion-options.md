---
author: adelp
categories: Education
comments: true
date: 2009-02-13T16:00:03Z
slug: blades-and-virtualization-arent-mutually-exclusive-part-three-ibm-traditional-expansion-options
tags:
- bladevrack
- Hardware
- HP
- IBM
- Virtualization
- GuestPost
title: 'Blades and Virtualization Aren''t Mutually Exclusive: Part Three, IBM Traditional
  Expansion Options'
url: /2009/02/13/blades-and-virtualization-arent-mutually-exclusive-part-three-ibm-traditional-expansion-options/
wordpress_id: 1176
---

_By Aaron Delp_

_UPDATE: I have made some corrections to the 10GB and iSCSI sections based on feedback. Thank you!_

Welcome to part 3 of my series on blades and virtualization. I will be breaking the expansion articles into four parts: traditional IBM options, traditional HP options, virtualized IBM options, and virtualized HP options. By "virtualized options" I mean the IBM and HP blade I/O virtualization solutions (like IBM Blade Open Fabric Manager and HP VirtualConnect), not a server configuration that will run virtualization. Today, we will be covering IBM traditional options.

Let's take a second to revisit a portion of our hardware standard we have been using for the power series and make a few additional notes. Here is the spec again:

* A minimum of 32 GB memory  As many have commented and my own studies have shown; memory is usually the first limiting factor. The more memory you can install in the system, the better.  
* 2 on-board NICs + some combination of expansion cards (FC, iSCSI, NIC, 10Gb)  
* 2 x 146GB, 10k SAS drives (except the IBM HS21XM blades which only support one HD)

I will get into the expansion options in a second, but let me start with the memory since this is the area most people are concerned about. With the recent addition of 8GB DIMMS to the HS21XM product line, the HS21XM now supports 64GB of memory. The IBM 3550 supports a max of 32GB and even the IBM 3650 only supports 48GB. If memory is your concern, the IBM blade is actually the BEST fit for virtualization.

Another area of concern is the fact that the HS21XM only supports one hard disk. If you would like to boot the OS from a local disk, this can be an issue. IBM also offers the dual solid state drive in a RAID-1 configuration but both platters are mounted in the same enclosure so if you lose one, you need to replace them both. We know because it happened to one of our customers. In the end that doesn't make sense either. I don't see the actual ESX build as critical in a VirtualCenter environment so I usually don't worry about it excessively.

The expansion cards can get a little tricky depending on the configuration. For virtualization I see four possible options to fill out the blades to meet our needs. They are FC, 10Gb, iSCSI, and NFS. We will fill any remaining expansion with Ethernet 1GB ports to provide as much connectivity as possible.

![BladeCenter H rear view](/public/img/bladecenterh-rear.jpg)

Above is a picture of an IBM BladeCenter H Chassis from the rear, highlighting the expansion bays. Bays 1 and 2 connect to the onboard Ethernet so the switches must be Ethernet compatible. Bays 3 and 4 connect to the first expansion card on the IBM Blade, often called the CFFv form factor. The v in CFFv stands for vertical meaning this card will talk to the vertical expansion switches.

Bays 7-10 often cause much confusion. They are positioned horizontally in the chassis and connect to any CFFh (h for horizontal, get it?) card on a blade. A number of different switches can be inserted into these bays. The first one is the easiest one, the 10Gb switch. The first switch would go in bay 7. The second would go in bay 9. Each switch would connect to a port on the dual port 10Gb card.

UPDATE: You can also do a 4x10GB configuration by adding 2 more 10GB switches in Bays 8 and 10. You will then need the 4 port 10GB card instead of the 2 port 10Gb card. I have updated the charts below to reflect the MAXIMUM number of connections. You can of course go with less.

The second option for Bays 7-10 involves something called the MSIM (Multi-Switch Interconnect Module). This option will allow the FC, iSCSI, and NFS configurations. The MSIM is an adapter that divides the High Speed Bays into Bays 7 to 10 shown above. To take advantage of all four bays, two MSIMs divide both High Speed Bays and you will need a four port CFFh card to map to all four bays.

The above information yields the following switch and blade expansion card combinations:

**Blade with 10Gb & 4 NICs (no MSIMs -- 4 NICs and 4 10Gb per blade):**

| Expansion option    | Connects to/Requires             |
|---------------------|----------------------------------|
| Dual on-board NICs  | Bays 1 and 2 (Ethernet switches) |
| Dual-port CFFv NIC  | Bays 3 and 4 (Ethernet switches) |
| Dual-port 10Gb card | Bays 7 thru 10 (10Gb switches)   |

**Blade with 10Gb, FC & Eth (no MSIMs -- 2 NICs, 2 FC and 4 10Gb per blade):**

| Expansion option       | Connects to/Requires             |
|------------------------|----------------------------------|
| Dual on-board NICs     | Bays 1 and 2 (Ethernet switches) |
| Dual-port CFFv FC card | Bays 3 and 4 (FC switches)       |
| Dual-port 10Gb card    | Bays 7 thru 10 (10Gb switches)   |

**Blade with FC Option #1 (requires 2 MSIMs -- 2 FC and 6 NICs per blade):**

| Expansion option       | Connects to/Requires               |
|------------------------|------------------------------------|
| Dual on-board NICs     | Bays 1 and 2 (Ethernet switches)   |
| Dual-port CFFv FC card | Bays 3 and 4 (FC switches)         |
| Quad-port CFFh NIC     | Bays 7 thru 10 (Ethernet switches) |

**Blade with FC Option #2 (requires 2 MSIMs -- 2 FC and 6 NICs per blade):**

| Expansion option           | Connects to/Requires                     |
|----------------------------|------------------------------------------|
| Dual on-board NICs         | Bays 1 and 2 (Ethernet switches)         |
| Dual-port CFFv NIC         | Bays 3 and 4 (Ethernet switches)         |
| Dual NIC/dual FC CFFh card | Bays 7,9 (Ethernet) & 8,10 (FC switches) |

**Blade with NFS or software iSCSI (2 MSIMs -- 8 NICs per blade):**

| Expansion option   | Connects to/Requires               |
|--------------------|------------------------------------|
| Dual on-board NICs | Bays 1 and 2 (Ethernet switches)   |
| Dual-port CFFv NIC | Bays 3 and 4 (Ethernet switches)   |
| Quad-port CFFh NIC | Bays 7 thru 10 (Ethernet switches) |

You will notice that only 3 of the 4 options are given. Hardware-based iSCSI is missing. That is because IBM has a hole in their portfolio. The IBM iSCSI card is the older form factor that if used on an HS21XM blade it doesn't allow any other card. So, you end up with 2 NICs and 2 iSCSI. This isn't acceptable and isn't a valid option.

So what do you think? I believe that any of the above configurations will suit virtualization nicely given a proper design. I look forward to your thoughts as we continue on to the next article, HP traditional expansion.
