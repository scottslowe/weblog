---
author: adelp
categories: Education
comments: true
date: 2009-02-16T16:00:05Z
slug: blades-and-virtualization-arent-mutually-exclusive-part-four-hp-traditional-expansion-options
tags:
- bladevrack
- Hardware
- HP
- IBM
- Virtualization
- GuestPost
title: 'Blades and Virtualization Aren''t Mutually Exclusive: Part Four, HP Traditional
  Expansion Options'
url: /2009/02/16/blades-and-virtualization-arent-mutually-exclusive-part-four-hp-traditional-expansion-options/
wordpress_id: 1188
---

_By Aaron Delp_

Welcome to Part 4 in our series on blades and virtualization. Please note that I will be covering the HP virtualized I/O offerings in a near future post. I plan to cover Virtual Connect, Flex-10, and the BL495 blade at that time.

As always, lets start by reviewing the hardware specification we will be using to compare the rack servers against the blade servers. Here are the portions of the spec that concern us:

* A minimum of 32 GB memory  As many have commented and my own studies have shown, memory is usually the first limiting factor. The more memory you can install in the system, the better.  
* 2 on-board NICs + some combination of expansion cards (FC, iSCSI, NIC, 10Gb)

As we did for the IBM side, let's start with memory. The maximum amount of memory available on the DL360 is currently 64GB when using 8GB DIMMS in each of the eight slots. The BL460c has the same memory configuration (8 slots), but has a few caveats depending on the processor power consumption. According to the HP Product Bulletin Tool, if the processor is 80W or below, then the max amount of memory is 64GB (8x8GB). If you would like to use processors above 80W, then the maximum amount of memory is 48GB in a 6x8GB configuration. Which processors take above 80W you ask? As of this writing, there are only 2 processors in this category, the 3.0GHz Quad Core (Intel X5450 chip, **not** the E5450 chip!) and the 3.16GHz Quad Core (Intel X5460).

As with the IBM solution, I will explore four possible transport technologies for virtualization: 10Gb, iSCSI, FC, and NFS. I will also fill any remaining expansion slots with 1Gb Ethernet ports. Just like the IBM Chassis, we have a maximum of eight expansion ports.

![HP Chassis rear view](/public/img/hp-chassis-rear.jpg)

HP expansion is a little more straightforward than IBM. Bays 1 & 2 connect to the blade on-board NICs and must be populated with an Ethernet compatible switch. An HP BL460c blade has two expansion slots, labeled Mezzanine 1 & 2. Bays 3 & 4 on the chassis connect to the adapter in Mezzanine Slot 1 on the BL460c Blade. Bays 5-8 connect to Mezzanine bay 2 on the blade. If a dual port card is placed in Mezzanine 2, only Bays 5 & 6 will be active. A four port card is required (Ethernet is the only 4 port card currently) to access all four switch bays.

The only exception to the above port mappings is 10Gb. Like the IBM 10Gb switch, the HP 10Gb switch is a double wide switch. For dual 10Gb in the HP chassis, one switch must be placed in Bays 5/6, the other in Bays 7/8.

Here are the possible configurations given the port mappings above:

**BL460c blade with 10Gb & 4 NICs:**

| Expansion option        | Connects to/Requires                      |
|-------------------------|-------------------------------------------|
| Dual on-board NICs      | Bays 1 and 2 (Ethernet switches)          |
| Mezz 1 - Dual-port NIC  | Bays 3 and 4 (Ethernet switches)          |
| Mezz 2 - Dual-port 10Gb | Bays 5/6 (10Gb switch), 7/8 (10Gb switch) |

**BL460c blade with 10Gb, 2 NICs, and 2 FC:**

| Expansion option           | Connects to/Requires                      |
|----------------------------|-------------------------------------------|
| Dual on-board NICs         | Bays 1 and 2 (Ethernet switches)          |
| Mezz 1 - Dual-port FC card | Bays 3 and 4 (FC switches)                |
| Mezz 2 - Dual-port 10Gb    | Bays 5/6 (10Gb switch), 7/8 (10Gb switch) |

**BL460c blade with FC & 6 NICs:**

| Expansion option           | Connects to/Requires                 |
|----------------------------|--------------------------------------|
| Dual on-board NICs         | Bays 1 and 2 (Ethernet switches)     |
| Mezz 1 - Dual-port FC card | Bays 3 and 4 (FC switches)           |
| Mezz 2 - Quad-port NIC     | Bays 5 through 8 (Ethernet switches) |

**BL460c blade with iSCSI & 6 NICs:**

| Expansion option              | Connects to/Requires                 |
|-------------------------------|--------------------------------------|
| Dual on-board NICs            | Bays 1 and 2 (Ethernet switches)     |
| Mezz 1 - Dual-port iSCSI card | Bays 3 and 4 (Ethernet switches)     |
| Mezz 2 - Quad-port NIC        | Bays 5 through 8 (Ethernet switches) |

**BL460c blade 8 NICs:**

| Expansion option       | Connects to/Requires                 |
|------------------------|--------------------------------------|
| Dual on-board NICs     | Bays 1 and 2 (Ethernet switches)     |
| Mezz 1 - Dual-port NIC | Bays 3 and 4 (Ethernet switches)     |
| Mezz 2 - Quad-port NIC | Bays 5 through 8 (Ethernet switches) |

As you can see from the tables above, HP has a nice round portfolio without any holes. In the next section we will be covering the IBM virtualized I/O solutions.
