---
author: slowe
categories: Information
comments: true
date: 2007-12-31T11:44:02Z
slug: hyper-v-architectural-issue
tags:
- HyperV
- Microsoft
- Virtualization
title: Hyper-V Architectural Issue
url: /2007/12/31/hyper-v-architectural-issue/
wordpress_id: 597
---

Alerted to this information by Alessandro at virtualization.info, it appears that [Hyper-V will not boot VMs via a SCSI virtual disk](http://www.virtualization.info/2007/12/hyper-v-will-not-boot-virtual-scsi.html). Instead, Hyper-V virtual machines will only boot from an IDE virtual disk.

In case you're wondering why this really matters---as I was when I first saw this headline---[this post](http://blogs.msdn.com/tvoellm/archive/2007/12/12/which-is-better-ide-or-scsi-windows-server-virtualization-08-code-name-viridian-controller-performance.aspx) from Tony Voellm at Microsoft may shed some light on the issue:

>The IDE controller implements a well-known IDE controller and this means there is extra processing before the I/O is sent to the disk. This processing occurs in vmwp.exe (a user mode process that exists for each started VM. More on this in a later post). Once the IDE emulation is complete the I/O is sent into the Root Partition's I/O Stack. I/O completion requires a trip back to vmwp.exe.

And, as confirmed in the comments to that article, Tony confirms that Hyper-V VMs will be _required_ to have the OS installed on a virtual IDE disk.

This latest revelation comes hard on the heels of a less-than-favorable review of Hyper-V by InfoWorld, where Hyper-V---which is supposed to compete with ESX Server---is instead compared with VMware's hosted product VMware Server. This is in addition to numerous delays and feature cuts. I guess Hyper-V's start is going to be rockier [than I thought][1].

**UPDATE:** Thanks to some information from Ben Armstrong in the comments, it appears that the performance impact from the use of virtual IDE disks may not be as significant as suspected. Check out Ben's comment below and read the blog posting referenced in his comment for more information.

[1]: {{< relref "2007-12-27-hyper-v-off-to-a-rocky-start.md" >}}
