---
author: adelp
categories: Education
comments: true
date: 2009-02-07T15:20:42Z
slug: blades-and-virtualization-arent-mutually-exclusive-part-two-ibm-power-sizing
tags:
- bladevrack
- Hardware
- HP
- IBM
- Virtualization
- GuestPost
title: 'Blades and Virtualization Aren''t Mutually Exclusive: Part Two, IBM Power
  Sizing'
url: /2009/02/07/blades-and-virtualization-arent-mutually-exclusive-part-two-ibm-power-sizing/
wordpress_id: 1162
---

_By Aaron Delp_

This is the second part of a series of articles on blades and virtualization. [The first article is found here.][1] Today, I will present IBM rack servers versus blade servers and explore the power consumption for each.

In case you need it again, here is the hardware platform I will be using as the standard:

* 2 x E5450 3.0 Ghz quad-core processors  
* 32 GB memory in an 8 x 4GB configuration  
* 2 on-board NICs  
* Additional quad NIC card for a total of 6 NICs  
* Dual FC card (assuming FC SAN storage for VMs but it could easily be NFS or iSCSI as well)  
* 2 x 146GB, 10k SAS drives (except the IBM HS21XM blades which only support one HD)

Here is the link to the [IBM System x and Blade Power Configuration Tool](http://www-03.ibm.com/systems/bladecenter/resources/powerconfig/index.html) I used for the numbers below. Using the hardware specifications above, I will be comparing the IBM System x 3550 server against the IBM HS21XM blade server.

Let me start by giving you a little history of the IBM Power Tool and note some limitations that may skew these results slightly. In case you haven't used it before, the IBM tool is a little rough around the edges. Not all of the products and configurations are included in the tool. The Power Tool was started as a side project by some members of the IBM Brand Team a few years ago and was picked up by IBM as a product. Although I knew the original authors before they moved on to other jobs, I am not sure who keeps it up to date today.

The System x 3550 calculations are pretty accurate but the IBM Blade Center calculations are missing a few pieces. The tool will only allow for up four switches in the chassis. Even though you can configure two MSIM's to enable the additional switch bays, you can't choose the additional switches in the tool. Also, the IBM Blades section of the tool won't allow for the CFFh expansion card that would attach to the switches. So, these calculations are missing four switches per chassis and one expansion card per blade. Not a big deal but I wanted to point it out. Also, please remember that all calculations assume 100% utilization of the server(s).

With all of that out of the way, what does a fully loaded IBM chassis look like compared to rack servers? An IBM BladeCenter H Chassis holds fourteen servers so we will begin there:

| 14 Servers (Full Chassis)     | 3550  | HS21XM | Savings |
|-------------------------------|-------|--------|---------|
| Total system power input (W)  | 7028  | 4869   | 30.72%  |
| Total system input current (A)| 33.6  | 23.41  | 30.33%  |
| Total system BTU/hr           | 23954 | 16614  | 30.64%  |
| Total system VA rating        | 7168  | 5098   | 28.87%  |

As you can see, from a power perspective, the IBM blades are pretty impressive! We are looking at approximately 30% savings in all areas versus IBM rack servers. Let me be very clear on one point though: I am not stating that IBM blades are more power efficient than HP blades (I know that comparison is right around the corner for many of you). For many reasons that I won't go into here, I don't believe you can make a jump to that conclusion based on the numbers. I am stating that IBM blades are more efficient than IBM rack servers, nothing more.

How about a chassis that is half full with seven servers?

| 7 Servers (Full Chassis)      | 3550  | HS21XM | Savings |
|-------------------------------|-------|--------|---------|
| Total system power input (W)  | 3514  | 2732   | 22.25%  |
| Total system input current (A)| 16.8  | 13.14  | 21.79%  |
| Total system BTU/hr           | 11977 | 9324   | 22.15%  |
| Total system VA rating        | 3584  | 2873   | 19.83%  |

We are seeing 20% or greater with a half full chassis. Again, the numbers are very nice.

Lastly, what is the breakeven point? For the IBM blades the breakeven point is four servers.

| 4 Servers (Full Chassis)      | 3550  | HS21XM | Savings |
|-------------------------------|-------|--------|---------|
| Total system power input (W)  | 2008  | 1805   | 10.10%  |
| Total system input current (A)| 9.6   | 8.68   | 9.58%   |
| Total system BTU/hr           | 6844  | 6159   | 10.00%  |
| Total system VA rating        | 2048  | 1907   | 6.88%   |

The most interesting bit of information I find with these numbers is the difference the fourth blade makes. With three blades, you have slightly negative percentages (the rack servers are better). But, add the fourth blade and you are suddenly saving somewhere near ten percent. I find that odd.

My conclusion this time is a cut and paste of last time (almost literally!). As with any consolidation project, there is a breakeven point with blade servers where the initial investment begins to pay off. Also, like virtualization, the more blades (or virtual machines) you add to the environment, the more savings you will see.

The next article will focus on the expansion abilities of both the IBM and HP blade servers. I know that expandability is a big concern for many readers, so stay tuned for Part 3!

[1]: {{< relref "2009-02-04-blades-and-virtualization-arent-mutually-exclusive-part-one-hp-power-sizing.md" >}}
