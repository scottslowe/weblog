---
author: slowe
categories: Explanation
comments: true
date: 2006-11-23T15:06:49Z
slug: vmotion-compatibility
tags:
- Hardware
- Virtualization
- VMotion
- VMware
title: VMotion Compatibility
url: /2006/11/23/vmotion-compatibility/
wordpress_id: 370
---

Richard Garsthagen of [www.run-virtual.com](http://www.run-virtual.com) has published an application (I believe he's calling it [VMotion Info](http://www.run-virtual.com/?page_id=155)) that clearly identifies the differences between CPUs across servers in a farm of ESX servers. This application talks to the VirtualCenter server and then gather information from all the ESX servers represented in VirtualCenter. After gathering all the information, it will then graphically present the differences and actually decode the raw bits to tell you exactly what the differences are (i.e., NX/XD supported/not supported, SSE2/SSE3, etc.).

This is _extremely_ useful information. The missing piece, unfortunately, is the conversion to the binary bits that are necessary to actually mask that information from the virtual machine. I provided some of that information in my earlier article, but for completeness' sake thought I might present it here again. Hopefully, this will prove helpful to someone.

For example, here are the raw binary values I obtained on two servers in my server farm:

    ID1ECX 0x0000641d (server one)  
    ID1ECX 0x00004400 (server two)

Converted to binary, that becomes:

    0000 0000 0000 0000 0110 0100 0001 1101 (server one)  
    0000 0000 0000 0000 0100 0100 0000 0000 (server two)

Observing the binary values, we can easily see the differences between the two CPU families and why (by default) VMotion won't work across them. But what do these values mean? And how does this information play into Richard's VMotion Info tool? Good questions.

Taken from [this Intel document](http://www.intel.com/design/xeon/applnots/241618.htm), here are the meanings of those binary values (bit 0 would be the rightmost bit in the binary values above):

0 - SSE3  
1 and 2 - Reserved  
3 - MONITOR  
4 - DS-CPL  
5 - VMX (Intel Virtualization Technology)  
6 - Reserved  
7 - EST (Enhanced Intel SpeedStep Technology)  
8 - TM2  
9 - SSSE3 (Supplemental SSE3)  
10 - CID  
11 and 12 - Reserved  
13 - CX16  
14 - xTPR  
15 through 17 - Reserved  
18 - DCA (Direct Cache Access)  
19 through 31 - Reserved

In the example provided above, the differences between the CPUs were in bits 0, 2, 3, 4, 10, and 13.  That translates into differences in SSE3, MONITOR, DS-CPL, CID, and CX16 support between the two CPUs.

However, not all those features are necessarily significant to VMotion. In order to know what features are significant to VMotion, we'll need to review the default CPU mask that's provided by VirtualCenter to new virtual machines. Based on my research, the default mask provided to a new Windows Server 2003-based virtual machine is this:

    RRRR RRRR RRRR RRR0 00XR R0H0 000H 0RRH

According to the legend from VirtualCenter, any bit marked R or H must match for successful VMotion. This means that although bits 0, 2, 3, 4, 10, and 13 are different, only bits 0, 2, 4, and 10 are actually significant to VMotion (bits 3 and 13 are already marked 0 [Hide feature from guest software] or X [Unused by guest software]). In order to get VMotion working, we'll need to mask those bits that are different _and_ are significant to VMotion compatibility.

In addition, there are multiple registers---ECX and EDX---and different values (0, 1, 80000000h, and 80000001h).

I'm assuming that Richard's tool only identifies those flags that actually impair VMotion compatibility. Those flags, and their corresponding register bits are listed below:

NX/XD - Level 80000001 EDX bit 20  
FFXSR - (unknown)  
RDTSCP - (unknown)  
SSE - Level 1 EDX bit 25  
SSE2 - Level 1 EDX bit 26  
SSE3 - Level 1 ECX bit 0  
SSSE3 - Level 1 ECX bit 9  
CMPXCHG16B - Level 1 ECX bit 13

I couldn't find the feature flag values for FFXSR and RDTSCP in the Intel documentation. Not shown above (as far as I can tell) is the flag for the Intel 64-bit extensions, which is found at Level 80000001 EDX bit 29.

So, if after running the Virtual Info tool you find differences in NX/XD and Intel 64 support, then you can create a custom CPU mask that hides (using 0 in the mask) level 80000001 EDX bit 20 and level 80000001 EDX bit 29. By placing a zero in the mask, then these values are hidden from the VM and therefore won't affect VMotion compatibility.

Using the Virtual Info tool, you can now easily and clearly identify the differences in your CPUs. Then, using the breakdown listed above, you can easily create the necessary CPU masks to hide those differences and increase VMotion compatibility across hosts. Keep in mind, though, that most of the CPU masks are currently **_unsupported_** by VMware. (I just noticed that the supported vs. unsupported "relaxations," or masks, are listed at the bottom of Richard's tool.)

Also, there's an ongoing VMTN forum discussion occurring about CPU compatibility and VMotion; Mike Laverick (of [RTFM Education](http://www.rtfm-ed.co.uk/)) also [mentioned it on his weblog](http://www.rtfm-ed.co.uk/?p=314) as well (see the section marked "New Community Initiatives").
