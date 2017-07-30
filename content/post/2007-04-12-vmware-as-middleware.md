---
author: slowe
categories: Rant
comments: true
date: 2007-04-12T20:17:50Z
slug: vmware-as-middleware
tags:
- ESX
- Hardware
- Interoperability
- Virtualization
- VMware
title: VMware as Middleware
url: /2007/04/12/vmware-as-middleware/
wordpress_id: 440
---

The author of this [recent post](http://blog.rvpc.com/index.php/vmware-as-an-abstraction-layer-may-not-be-that-far-off/) suggests that using [VMware](http://www.vmware.com/) as "middleware for the OS" provides an abstraction layer that can help organizations in a variety of ways:

>A layer between the hardware and OS could drastically reduce the complexity of upgrades and help to future proof a changing environment.

In that regard, I agree. Years ago, Dave Cutler and the Windows NT development team envisioned the HAL (Hardware Abstraction Layer) as a tool to do the same thing that VMware's virtualization layer is doing today, and that is to hide many of the platform dependencies. In Windows NT's heyday, you could run the OS on x86, MIPS, PowerPC, and DEC Alpha systems---all based on the same high-level code and derived from the same code base, made possible by the HAL. Now VMware has stepped in to perform a similar function, abstracting workloads from the underlying hardware so that workloads can be transparently scheduled across CPU cores both within and between server chassis. With the rise of x86/x64 and the fall of RISC chips, the need to be portable across processor platforms has decreased, and so instead of providing portability across CPU architectures, VMware's virtualization layer provides portability across CPUs and CPU cores. It's a tradeoff I'll take.

So, in one regard, I agree with the author in that using VMware as an abstraction layer _can_ reduce complexity and provide flexibility and protection against future upgrades.

Where I differ from the author's perspective is the idea of a "1:1 ratio" of operating system to hardware, with the virtualization layer in the middle. In my mind, one of the key benefits of the virtualization layer is higher utilization of the underlying hardware. As multi-core processors proliferate, many mainstream operating systems (not all, but many) are unable to effectively utilize the processing power. This will become more and more evident as the number of cores and the parallel processing power of each core increases. Why then not multiplex workloads onto the hardware? In this case, you still gain the abstraction benefits but you also gain a better return on investment.
