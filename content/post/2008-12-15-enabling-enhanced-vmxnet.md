---
author: slowe
categories: Tutorial
comments: true
date: 2008-12-15T17:14:02Z
slug: enabling-enhanced-vmxnet
tags:
- ESX
- Networking
- Virtualization
- VMware
- Windows
title: Enabling Enhanced VMXNet
url: /2008/12/15/enabling-enhanced-vmxnet/
wordpress_id: 1098
---

I've been doing some additional digging into jumbo frames and other networking configurations, and along the way I came across references to VMware's Enhanced VMXNet network adapters. These were apparently introduced in VMware ESX 3.5 and are necessary in order for virtual machines to take advantage of features like TCP Segmentation Offload (TSO) or jumbo frames.

The funny thing is this: even though the Enhanced VMXNet adapters are supported on both 32-bit and 64-bit versions of Windows Server 2003 and a few different flavors of Linux, the only way to be able to specify that you want to use an Enhanced VMXNet driver is if the virtual machine is configured as 64-bit Windows Server 2003 Enterprise Edition. This is outlined in [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1007195) describing to how enable enhanced VMXNet drivers in Windows Server 2003.

Fortunately, there's a way to also do this in the .VMX file. You'll need to remove the VM from the VirtualCenter inventory (right-click, Remove From Inventory), then edit the .VMX file to add these entries:

	ethernet0.virtualDev = "vmxnet"  
	ethernet0.features = "15"

Once these lines have been added---changing ethernet0 to ethernet1 or whatever the appropriate value is for the VM---you can use the Datastore Browser to add the VM back into the inventory. The NICs will now be listed as "Enhanced vmxnet". In addition, Windows itself will now recognize greater offload functionality. The two screenshots below show the output of the command `netsh int ip show offload`; the first one is from a non-Enhanced VMXNet adapter:

![](/public/img/offload-regvmxnet.png)

Now for the output of the same command from a VM configured with an Enhanced VMXNet adapter:

![](/public/img/offload-enhvmxnet.png)

This technique allows you to preserve the "integrity" of the guest OS selection by enabling Enhanced VMXNet without requiring users to change their guest OS selection to 64-bit Windows Server 2003 Enterprise Edition. Once the Enhanced VMXNet adapter is enabled, then users are free to begin working with jumbo frames and TSO. More on that in a future post...

**UPDATE:** I learned that in vCenter 2.5 Update 3, the option to select Enhanced VMXNet is now a selectable option for various flavors of Windows and Linux. I guess this information isn't really needed any longer. Thanks for the information, Wade!
