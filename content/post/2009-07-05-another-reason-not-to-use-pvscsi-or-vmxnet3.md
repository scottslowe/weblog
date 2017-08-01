---
author: slowe
categories: Explanation
comments: true
date: 2009-07-05T14:54:35Z
slug: another-reason-not-to-use-pvscsi-or-vmxnet3
tags:
- Virtualization
- VMware
- VMwareFT
- vSphere
- Networking
- Storage
title: Another Reason Not to Use PVSCSI or VMXNET3
url: /2009/07/05/another-reason-not-to-use-pvscsi-or-vmxnet3/
wordpress_id: 1440
---

You might have read the article I wrote here titled [vSphere Virtual Machine Upgrade Process][1], in which I described a process whereby you could upgrade your VMs to VM hardware version 7 (the version used with vSphere) as well as use the latest paravirtualized network and SCSI drivers (VMXNET3 and PVSCSI). Both PVSCSI and VMXNET3 offer greater performance with the same CPU utilization.

Rightfully so, some readers and other bloggers pointed out that PVSCSI isn't supported for boot disks (Rich Brambley put up [a really good post](http://vmetc.com/2009/06/22/tap-into-vsphere-pvscsi-performance-with-separate-vm-boot-and-data-drives/), for example). Rich, among others, suggested moving virtual machines back to a "two disk model," with a boot disk and a separate data disk; this would allow for the greater performance of the PVSCSI controller on the data disk. This seemed to be a reasonable workaround. I don't recall hearing about any significant issues with VMXNET3. Using the newer network driver seemed to be a good move all the way around.

Unfortunately, there is another drawback to both of these devices. Rich caught this drawback in his article, but relegated it to a small mention at the very end of the article that even I overlooked at first (emphasis mine):

>There are some other factors to consider as well. For example, **vSphere Fault Tolerance cannot be enabled on a VM using PVSCSI.**

That's right---you cannot use VMware Fault Tolerance (FT) on a virtual machine that is using the PVSCSI device. However, this restriction doesn't just apply to the PVSCSI device; it also applies to VMXNET3! VMware FT cannot be enabled on a virtual machine using either the VMXNET3 or PVSCSI devices; vCenter Server will simply report an error that the network interface or disk controller isn't supported for VMware FT.

In my opinion, this is a significant enough limitation that I felt it warrants its own post. If you are planning on using VMware FT in your environment, be sure **not** to configure any virtual machines to use VMXNET3 or PVSCSI if they might need to be protected with VMware FT. In this case, you'll have to choose from either maximum performance or maximum protection---you don't get both.

**UPDATE:** Rich Brambley shared links to two resources that describe the incompatibility between VMware FT and PVSCSI and VMXNET3:

[VMware Communities: Unable to configure FT with error "Unsupported virtual machine configuration for Fault Tolerance. Device 'Network adapter 1' is not supported"](http://communities.vmware.com/thread/217845)  

[VMware Fault Tolerance Requirements and Limitations](http://communities.vmware.com/blogs/vmroyale/2009/05/18/vmware-fault-tolerance-requirements-and-limitations)

[1]: {{< relref "2009-06-01-vsphere-virtual-machine-upgrade-process.md" >}}
