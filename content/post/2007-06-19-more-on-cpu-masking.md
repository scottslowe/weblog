---
author: slowe
categories: Explanation
comments: true
date: 2007-06-19T14:57:08Z
slug: more-on-cpu-masking
tags:
- ESX
- Hardware
- Virtualization
- VMotion
- VMware
title: More on CPU Masking
url: /2007/06/19/more-on-cpu-masking/
wordpress_id: 474
---

I just added another server to my [VMware ESX Server](http://www.vmware.com/products/vi/esx/) farm, and since this farm is being built from leftover, donated servers that aren't being used elsewhere (such is the curse for a non-revenue generating test environment), I don't have the luxury of ensuring that all the servers in the farm have the same (or compatible) CPU families. As I've discussed in earlier blog postings ([Sneaking Around VMotion Limitations][1] and [VMotion Compatibility][2]), we can use custom CPU masks to help address the issue of different CPUs in the same ESX server farm.

Because this is a non-production environment, I don't have to worry too much about the fact that VMware doesn't support these types of custom CPU masks. With that in mind, I thought it might be helpful to again walk through the process of identifying the differences in the CPUs and creating custom CPU masks to address those differences. Of course, I don't recommend doing this in production environments.

The first step is to gather the information from the CPUs themselves. Richard Garsthagen's [VMotionInfo](http://www.run-virtual.com/?page_id=155) utility should provide all the information you need, but I have found it easier to just get the raw data myself using the CPU ID boot CD. This does require that you reboot the ESX server (thus taking it down), but given that we can use VMotion to move guests to a (known compatible) peer, this is not a huge issue in my mind.

Using the "Verbose" boot option of the CPU ID boot CD, here is the data gathered for the three servers in my farm. The value in parentheses is the value returned by CPU ID; I've added the binary value after that for comparison.

	Server1 ID1EAX (0x00000F43)  0000 0000 0000 0000 0000 1111 0100 0011  
	Server2 ID1EAX (0x00000F25)  0000 0000 0000 0000 0000 1111 0010 0101  
	Server3 ID1EAX (0x000006F6)  0000 0000 0000 0000 0000 0110 1111 0110

As you can see, converting the hexadecimal values (the ones in parentheses returned by the CPU ID boot CD) to binary shows that the differences lie in the last 3 bytes. That in and of itself doesn't really help that much until we combine the information above with the standard CPU mask for that register. Here's the same information again, only this time with another line showing the standard CPU mask:

	Server1 ID1EAX (0x00000F43)  0000 0000 0000 0000 0000 1111 0100 0011  
	Server2 ID1EAX (0x00000F25)  0000 0000 0000 0000 0000 1111 0010 0101  
	Server3 ID1EAX (0x000006F6)  0000 0000 0000 0000 0000 0110 1111 0110  
	Standard CPU Mask ID1EAX     XXXX HHHH HHHH XXXX XXXX HHHH XXXX XXXX

The CPU mask shows that only the bits from byte 3 (third from the right) are significant. This is indicated by the "H" in the mask, which (according to the legend in the Virtual Infrastructure Client) means that the guest will see the value of that register and that the value of the register must match for a successful VMotion.

&lt;aside&gt;I have not yet determined if the standard CPU masks are guest OS dependent, but I suspect that they are. Be sure to check the standard CPU mask against a VM configured for the guest OS that you will actually be running, or it may not work as you expect.<&lt;aside&gt;

To fix this, we need to add a custom CPU mask that masks that bits that are different. Here's the same information again, this time with a custom CPU mask and an effective CPU mask:

	Server1 ID1EAX (0x00000F43)  0000 0000 0000 0000 0000 1111 0100 0011  
	Server2 ID1EAX (0x00000F25)  0000 0000 0000 0000 0000 1111 0010 0101  
	Server3 ID1EAX (0x000006F6)  0000 0000 0000 0000 0000 0110 1111 0110  
	Standard CPU Mask ID1EAX     XXXX HHHH HHHH XXXX XXXX HHHH XXXX XXXX  
	Custom CPU Mask ID1EAX       ---- ---- ---- ---- ---- 0--0 ---- ----  
	Effective CPU Mask ID1EAX    XXXX HHHH HHHH XXXX XXXX 0HH0 XXXX XXXX

Based on the information in [this Intel documentation](http://www.intel.com/design/xeon/applnots/241618.htm), this register (ID 1 EAX) is the CPU identification register, and masking these bits will make it difficult (or perhaps even impossible) for ESX Server (or VirtualCenter) to correctly identify the CPUs in your host servers. This is the underlying reason these kinds of changes aren't supported: no one _really_ knows what impact this will have.

This is just the ID1EAX register, though; we must now repeat this process for the other registers. First, the ID1ECX register:

	Server1 ID1ECX (0x0000641D)  0000 0000 0000 0000 0110 0100 0001 1101  
	Server2 ID1ECX (0x00004400)  0000 0000 0000 0000 0100 0100 0000 0000  
	Server3 ID1ECX (0x0004E3BD)  0000 0000 0000 0100 1110 0011 1011 1101  
	Standard CPU Mask ID1ECX     RRRR RRRR RRRR RRR0 00XR R0H0 000H 0RRH  
	Custom CPU Mask ID1ECX       ---- ---- ---- -0-- ---- --0- ---0 -0-0  
	Effective CPU Mask ID1ECX    RRRR RRRR RRRR RRR0 00XR R000 0000 00R0

Now the ID81ECX register:

	Server1 ID81ECX (0x00000000) 0000 0000 0000 0000 0000 0000 0000 0000  
	Server2 ID81ECX (0x00000000) 0000 0000 0000 0000 0000 0000 0000 0000  
	Server3 ID81ECX (0x00000000) 0000 0000 0000 0000 0000 0000 0000 0001  
	Standard CPU Mask ID81ECX    RRRR RRRR RRRR RRRR RRRR RRRR RRRR RRRX  

And the ID81EDX register:

	Server1 ID81EDX (0x20000000) 0010 0000 0000 0000 0000 0000 0000 0000  
	Server2 ID81EDX (0x00000000) 0000 0000 0000 0000 0000 0000 0000 0000  
	Server3 ID81EDX (0x20100000) 0010 0000 0001 0000 0000 0000 0000 0000  
	Standard CPU Mask ID81EDX    RRXR RRRR RRRH RRRR RRRR HRRR RRRR RRRR  
	Custom CPU Mask ID81EDX      --0- ---- ---0 ---- ---- ---- ---- ----  
	Effective CPU Mask ID81EDX   RR0R RRRR RRR0 RRRR RRRR HRRR RRRR RRRR

You'll note that there is no custom mask for ID81ECX; that's because the default CPU mask hides all the significant bits (which in this case is only bit 0).

With these changes in place, I was able to successfully VMotion several different guest operating systems (including OpenBSD 4.1, Solaris 10 x86 Update 3, and Windows Server 2003 R2) between the servers. Your mileage may vary, however, and keep in mind that these changes are _unsupported_. Use them at your own risk! (I have to say that because if I didn't someone would invariably blame me for making one of their guest VMs crash. With that disclaimer out of the way, allow me to state that I haven't yet seen any problems as a result of the custom CPU masks.)

As always, please feel free to add any thoughts or corrections in the comments below.

[1]: {{< relref "2006-09-25-sneaking-around-vmotion-limitations.md" >}}
[2]: {{< relref "2006-11-23-vmotion-compatibility.md" >}}
