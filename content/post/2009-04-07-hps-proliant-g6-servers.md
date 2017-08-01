---
author: slowe
categories: Explanation
comments: true
date: 2009-04-07T10:22:17Z
slug: hps-proliant-g6-servers
tags:
- Hardware
- HP
- Virtualization
title: HP's ProLiant G6 Servers
url: /2009/04/07/hps-proliant-g6-servers/
wordpress_id: 1270
---

I spent the day today at Hewlett-Packard's Houston campus, talking about their Xeon 5500-based ProLiant servers. In HP parlance, these are the G6 (sixth generation) ProLiant servers, and HP has eleven different G6 server models. That's more Xeon 5500-based servers than both IBM and Dell, and represents a significant investment on HP's part to incorporate this new technology into their product lineup. HP's not alone in delivering servers based on the Xeon 5500 CPUs, but I think there may be a bit more here than meets the eye.

## Memory Bandwidth

There is little doubt that you haven't seen some of the performance comparisons of the Xeon 5500 with previous generations; these comparisons typically show significant performance advantages with the Xeon 5500. Many people have attributed that to QuickPath Interconnect (QPI), the new high-speed bus architecture Intel uses with the Xeon 5500 CPUs. Among other things, QPI provides higher memory bandwidth---but how much higher depends on the amount of memory installed. That's right: _the more memory you install in the server, the slower your memory speed will be._ This is a "dirty little secret" that many server vendors don't want to disclose.

Populate a memory bus with only a single DIMM, and it runs at 1333MHz. Put two DIMMs on the bus, and it will run at 1066MHz. Put three DIMMs on a bus (the maximum supported for each bus), and the speed drops to 800MHz. Ouch! So, the very systems that can benefit most from larger amounts of RAM (like virtualization hosts) will also be the systems that see the least benefit from increased memory bandwidth.

What HP has done here to differentiate themselves from some of the other server vendors is spent extra engineering time on the signal integrity of the memory bus so that they can actually preserve the bus speed in some instances. For example, when populating a memory bus with 2 DIMMs, HP ProLiant G6 servers can continue to run at 1333MHz instead of having to drop back to 1066MHz. In a server with 18 DIMM slots (like the DL380 G6, the BL490c G6, or the ML370 G6), this means the server can be loaded with up to 96GB of RAM and the memory will still run at 1333MHz. This helps to maximize both the capacity gains and the bandwidth gains of QPI and the Xeon 5500 CPU. To take advantage of this functionality, there is a BIOS setting that must be enabled.

## Power Efficiency

Power efficiency is another area where all the server vendors who are shipping servers with Xeon 5500 CPUs are claiming advances. I would imagine that most, if not all, of these claims of reduced power consumption are absolutely true. I would also guess that the server vendors are simply "piggy-backing" on the power consumption improvements that Intel made in the Xeon 5500 and not necessarily innovating on their own.

HP's ProLiant G6 servers have what they call a "sea of sensors": 32 different sensors on various areas of the system board that provide extensive information back to the system so that it can intelligently adjust fan speeds on the fly as the system operates. Fans are independently operated, so that a specific fan can be spun up to help reduce heat in specific areas of the system without having to spin up other fans. This can result in some pretty significant power reductions.

HP has also enabled systems with redundant power supplies to run primarily on one power supply instead of splitting the load equally between the power supplies. It's a well-known fact that a power supply running at a higher load works more efficiently, so this further helps improve the ProLiant G6's power savings.

&lt;aside&gt;By the way, I just have to point out that all the various G6 models now share a common power supply. Finally! Kudos to the HP team who pushed this through.&lt;/aside&gt;

I've always thought highly of the ProLiant server line, although some products were better liked than others (I didn't care for p-Class blades, for example). With this G6 release, I like HP's ProLiant servers even more. All the various features---the universal power supplies, the well-marked memory installation guidelines inside the chassis, the signal integrity work to help preserve bus speed---to me show an attention to detail that is exciting to see.

If you're interested in learning more about the G6 products, HP is hosting a ["Web Jam" event](http://www.hp.com/go/web-jam) tomorrow (Tuesday, April 7, 2009). Visit the site and register to join the virtual event. I'll be online during the virtual event, chatting with visitors and answering questions, so feel free to register and stop by.

**UPDATE:** Partly in conjunction with [this post][1], it has also come to light that the maximum bus speed is also affected by the wattage of the CPU. Only the 95 watt CPUs are able to run at the maximum speed of 1333MHz. In addition, HP did clarify today that the ability to maintain the bus speed when using 2 DIMMs is not yet available, but will be available shortly in a ROM update.

**UPDATE 2:** I received word from HP today that the ROM update that enables 1333MHz operation with 2 DIMMs on each bus is actually already on all shipping "Nehalem" systems. Of course, this would require a 95W CPU, since [only 95W CPUs][2] can support the 1333MHz memory bus speed.

[1]: {{< relref "2009-04-07-introduction-to-nehalem-cpus.md" >}}
[2]: {{< relref "2009-04-07-introduction-to-nehalem-cpus.md" >}}
