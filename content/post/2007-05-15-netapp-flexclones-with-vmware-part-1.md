---
author: slowe
categories: Explanation
comments: true
date: 2007-05-15T09:35:54Z
slug: netapp-flexclones-with-vmware-part-1
tags:
- NetApp
- Storage
- Virtualization
- VMware
title: NetApp FlexClones with VMware, Part 1
url: /2007/05/15/netapp-flexclones-with-vmware-part-1/
wordpress_id: 455
---

The ability to quickly and easily create new virtual servers in [VMware VirtualCenter](http://www.vmware.com/products/vi/vc/) (using templates and cloning) is a key feature that benefits a lot of VMware customers. A new server running [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx) in less than 10 minutes? Who wouldn't like that functionality? (Some other day, perhaps we'll discuss that very question.)

VMware's cloning functionality is great in that it is completely storage-agnostic; it works the same on an HP EVA, a NetApp storage system, an EMC Clariion, or a homebrew iSCSI target. At the same time, VMware's cloning functionality is not so great in that it is completely storage-agnostic. It doesn't take advantage of any of the hardware-specific functionality. In this article, I'd like for us to look at one particular vendor Network Appliance and how using some vendor- and hardware-specific functionality (namely [FlexClones](http://www.netapp.com/products/enterprise-software/storage-system-software/provisioning-volume-management/flexclone.html)) can provide some benefits.

Note that we won't discuss the mechanics of how we actually use FlexClones to provision VMs; that detailed technical information can be found in [this earlier article][1]. We'll be focusing on "Should we use FlexClones?" rather than "How do we use FlexClones?"

(By the way, check out these pages if you aren't familiar with NetApp [Snapshots](http://www.netapp.com/products/enterprise-software/storage-system-software/resiliency/snapshot.html) and [FlexClones](http://www.netapp.com/products/enterprise-software/storage-system-software/provisioning-volume-management/flexclone.html).)

As I see it, there are two key benefits to using FlexClones in a VMware environment:

* First, FlexClones are _space conservative_, meaning that they occupy very little space. In fact, they only occupy the space required to store the changes to the FlexClone from the original. If you clone a 100GB volume and only 10% of that volume changes, you only need 10GB of disk space. I provide an example of the space utilization below.

* Second, the time is takes to create a FlexClone is minimal and does not increase significantly with the amount of data involved. Creating a FlexClone of a 100GB volume is not significantly different in time required than creating a FlexClone of a 500GB volume.

The storage space savings can be very significant, especially in larger deployments, and especially when extra care is taken to minimize changes from the original to the clone. Here's an example taken from my test lab:
    
    Volume               Allocated          Used    Guarantee
    mstr_vdi_fvol       16956680KB     3024696KB       volume
    vdi_fvol_clone1       399148KB      399148KB         none
    vdi_fvol_clone2       704680KB      704680KB         none
    vdi_fvol_clone3       440220KB      440220KB         none

As you can see above, each of the clones occupies less than 1% of the original volume's size. As the number of FlexClones multiplies, then the space savings grow proportionally. In very large deployments (VDI deployments, for example, where there may be 500, 700, or even 1,000 VMs), the storage reduction can be very substantial, and the cost savings associated with that storage reduction can be very sizable. These aren't just "pie in the sky" savings, either---we're talking real, measurable savings in the amount of SAN storage that must be purchased to host the VMs.

Both of these benefits stand in direct contrast to VirtualCenter cloning:

* Each clone is a 1:1 copy of the original and occupies the same amount of space as the original. Clone a 50GB VM and it will require 50GB of disk space.

* The amount of time it takes to clone a VM is directly proportional to the size of the VM. A 50GB VM will take proportionately longer to clone than a 10GB VM.

At first glance, it's easy to look at these benefits and say, "Why _wouldn't_ I want to use FlexClones? I can create additional VMs in seconds instead of minutes, and save tons of expensive Fibre Channel SAN storage. What's not to like?" Those are very good questions. In [Part 2][2] of this series, we'll take a look at some of the disadvantages of using FlexClones in VMware environments. In the meantime, feel free to add any clarifications, corrections, or questions in the comments below.

[1]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[2]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
