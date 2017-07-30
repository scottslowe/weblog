---
author: adelp
categories: Education
comments: true
date: 2009-02-04T19:40:49Z
slug: blades-and-virtualization-arent-mutually-exclusive-part-one-hp-power-sizing
tags:
- bladevrack
- Hardware
- HP
- IBM
- Virtualization
- VMware
- GuestPost
title: 'Blades and Virtualization Aren''t Mutually Exclusive: Part One, HP Power Sizing'
url: /2009/02/04/blades-and-virtualization-arent-mutually-exclusive-part-one-hp-power-sizing/
wordpress_id: 1144
---

_By Aaron Delp_

I have seen a lot of misconceptions recently when it comes to blade servers and virtualization. Many seem to think that if you are using blades you can't use virtualization. Most recently, someone shared [this article](http://get-admin.com/blog/?p=392) with me. I couldn't disagree with this viewpoint more. I will explore the reasons why in the next few articles. For today's article, I will present a foundation for comparison and I will use this basis going forward.

Bar none, the most common misconception surrounding blades is power consumption. Many think that blade servers consume more power than their rack mount counterparts. If you have ever seen a fully loaded blade chassis, I can see why many come to this conclusion. They are loud, move a ton of air, and require larger power circuits that many people don't normally see in a data center ("Look at the size of that plug! This thing must be a power monster!").

But, if you are installing more than a few servers, this isn't true. First, let me present my basis for comparison and the tools I will be using. I have chosen a middle of the road VMware ESX configuration that is supported by both IBM and HP. Here is the configuration:
	
* 2 x E5450 3.0 Ghz quad-core processors  
* 32 GB memory in an 8 x 4GB configuration  
* 2 on-board NICs  
* Additional quad NIC card for a total of 6 NICs  
* Dual FC card (assuming FC SAN storage for VMs but it could easily be NFS or iSCSI as well)  
* 2 x 146GB, 10k SAS drives (except the IBM HS21XM blades which only support one HD)

Here are the links to both the IBM and HP Power Tools:

* [IBM System x and Blade Power Tool](http://www-03.ibm.com/systems/bladecenter/resources/powerconfig/index.html)

* [HP BladeSystem Power Tool](http://h71019.www7.hp.com/ActiveAnswers/cache/347628-0-0-0-121.html)

* [HP Proliant Server Power Tool](http://h30099.www3.hp.com/configurator/powercalcs.asp)

Using the above configuration and tools yielded an IBM System x 3550 and HP DL360 in the rack mount category and the IBM HS21XM and HP BL460c in the blade category. I will cover HP today and IBM in a subsequent article.

If you plug the above specs into the DL360 tool and multiply by sixteen to equal the number of servers in a chassis you will get the results below. I then ran the HP Blade System Sizer tool and filled the chassis with an equivalent spec. In order to properly populate the blades with the expansion cards, I was required to also add all of the switches. I added six VirtualConnect Ethernet and two VirtualConnect FC switches. I also assumed the max number of power supplies and fans in all calculations. Both tools assume 100% utilization of all resources.

| 16 Servers (Full Chassis)     | HP DL360  | HP BL460c  | Savings |
|-------------------------------|-----------|------------|---------|
| Total system power input (W)  | 7904      | 6021       | 23.82%  |
| Total system input current (A)| 40        | 29.54      | 26.15%  |
| Total system BTU/hr           | 26976     | 20530      | 23.89%  |
| Total system VA rating        | 8432      | 6143       | 27.14%  |

As you can see, even with the addition of eight switches in the blade chassis, you still save in excess of 20% in all areas. Pretty impressive no matter what you are running on the blades!

What if the chassis isn't entirely full? How about 8 blades?

| 8 Servers (Half Chassis)      | HP DL360  | HP BL460c  | Savings |
|-------------------------------|-----------|------------|---------|
| Total system power input (W)  | 3952      | 3396       | 14.06%  |
| Total system input current (A)| 20        | 16.66      | 16.70%  |
| Total system BTU/hr           | 13488     | 11579      | 14.15%  |
| Total system VA rating        | 4216      | 3465       | 17.81%  |

Still very nice results! Again, even with the switches in the blade chassis, you are saving in excess of 14% in all areas. The main story to tell here is this: the more populated the chassis, the more savings you will see.

What is the breakeven point from a power perspective with HP blades? The answer currently is five. Any less and the overhead of the chassis becomes a factor. Here are the numbers with five blades.

| 5 Servers (Half Chassis)      | HP DL360  | HP BL460c  | Savings |
|-------------------------------|-----------|------------|---------|
| Total system power input (W)  | 2470      | 2361       | 4.41%   |
| Total system input current (A)| 12.5      | 11.58      | 7.36%   |
| Total system BTU/hr           | 8430      | 8052       | 4.48%   |
| Total system VA rating        | 2635      | 2409       | 8.57%   |

In conclusion, as with any consolidation project, there is a breakeven point with blade servers where the initial investment begins to pay off. Also, like virtualization, the more blades (or virtual machines) you add to the environment, the more savings you will see.

Thank you for your time! I will be covering the IBM comparison in the near future, so check back regularly.
