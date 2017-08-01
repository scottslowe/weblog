---
author: slowe
categories: Explanation
comments: true
date: 2006-09-25T14:32:24Z
slug: sneaking-around-vmotion-limitations
tags:
- Hardware
- Virtualization
- VMotion
- VMware
title: Sneaking Around VMotion Limitations
url: /2006/09/25/sneaking-around-vmotion-limitations/
wordpress_id: 339
---

First and foremost allow me to make this disclaimer: **Do not do this on production systems.** While it may work for testing (I haven't seen any ill effects yet), it's definitely not recommended for production use. I seriously doubt that VMware would provide any support for this kind of hack.

Now, with that serious warning out of the way, let's get into the details. Based on the information found in [this VMTN forum discussion](http://www.vmware.com/community/thread.jspa?threadID=50828), I was able to find the information I needed to determine exactly what differences were causing VirtualCenter (VC) to report that it couldn't VMotion between the two hosts.

The real trick here is the use of the CPUID ISO image, found on the ESX 3.0 CD. Burn this ISO image to a CD and then boot from the CD; the results from this command will give you detailed information on the exact functionality and capabilities of the CPUs in your system. Write this information down _exactly_ as it appears on the screen when running the test; you'll need it later.

For example, on one of the hosts I received information like this:

    ID1ECX 0x0000641d
    ID1EDX 0xbfebfbff
    ID81ECX 0000000000
    ID81EDX 0x20000000

This all looks like jibberish, but using [this document from Intel](http://www.intel.com/design/xeon/applnots/241618.htm) gives the breakdown on what those numbers mean. On a second ESX host, I received these values from CPUID:

    ID1ECX 0x00004400
    ID1EDX 0xbfebfbff
    ID81ECX 0000000000
    ID81EDX 0000000000

As you can see, the differences lie in the ECX register, which is exactly what VC was reporting during its "validation" process. Having now identified the specific differences between the hosts, we proceed to mask (or hide) those values from the guest VMs by modifying the VM's configuration.

For the VM that you want modified, right-click and go to Edit Settings > Options tab > Advanced > Advanced button. From here, you'll see a listing of the CPU registers (EAX, EBX, ECX, and EDX) and the masks that will be applied to each register at various levels. Because all that was returned by CPUID was ECX and EDX and levels 1 and 81, that's all we need to worry about.

ESX Server will supply a default mask that shows or hides certain flags to the VMs. We can modify that mask to hide additional values, thus increasing VMotion compatibility at the expense of possibly lower performance (because VMs won't be able to take full advantage of some higher-level functions of the CPUs). For example, the default mask for ID1ECX for a Windows-based host is this (there's a "Legend" button in the same dialog box to explain what these values mean):

    RRRR RRRR RRRR RRR0 00XR R0H0 000H 0RRH

To make this work, what we need to do is change the R or H to a zero (0) where the differences between the hosts are. However, in order to really understand where the differences are between the hosts, you must first convert the hexadecimal values above to binary. In my particular example, here are the converted values (again, for ID1ECX):

    RRRR RRRR RRRR RRR0 00XR R0H0 000H 0RRH (mask)
    0000 0000 0000 0000 0110 0100 0001 1101 (first system)
    0000 0000 0000 0000 0100 0000 0000 0000 (second system)

The only bits we need to mask are the bits not already masked (i.e., those marked with an R or an H in the mask) and where the values are different between the two systems. In this case, bits 0, 2, and 4 are different and are not already masked. (You'll note that bit 14 is also different between the two hosts, but it's already hidden in the mask and therefore is not an issue.) To mask these bits, we'll edit the line in this dialog box to put a "0" in the position for bits 0, 2, and 4, leaving all other bits intact (use a dash for all the other bits). When you're done, the custom mask in the dialog box will look something like this:

    ---- ---- ---- ---- ---- ---- ---0 -0-0

When this custom mask is added to the default mask, you get an effective mask that hides the different bits between the hosts:

    RRRR RRRR RRRR RRR0 00XR R0H0 000H 0RRH (default mask)
    ---- ---- ---- ---- ---- ---- ---0 -0-0 (custom mask)
    RRRR RRRR RRRR RRRR 00XR R0H0 0000 00R0 (effective mask)
    0000 0000 0000 0000 0110 0100 0001 1101 (first system)
    0000 0000 0000 0000 0100 0000 0000 0000 (second system)

As you can see, because none of the different bits between the two hosts are marked with an R or an H, then they are all masked from VirtualCenter and therefore won't prevent VMotion from working. (Keep in mind that an X means it's unused or ignored; only the R and the H will come into play for computing VMotion compatibility.)

So far, I've tested this on VMs running Windows Server 2003, Windows 2000 Server, Solaris 10, and Linux (CentOS 4.3). I've not seen any problems thus far, and I've VMotion'ed these VMs back and forth a number of times trying to uncover some compatibility problems. If I discover any, I'll be sure to post more information here.

**UPDATE:** Also see [this article][1] for more information and a link to a tool that has been released to help with this process.

**UPDATE 2:** Thanks to an astute commenter, I've updated the article accordingly to note that bit 1 (the bits are numbered starting with 0) does not need to be masked, since it is the same between the two CPU families.

[1]: {{< relref "2006-11-23-vmotion-compatibility.md" >}}
