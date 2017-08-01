---
author: slowe
categories: Information
comments: true
date: 2011-01-20T15:16:52Z
slug: clearing-up-the-bad-reputation-of-vmwares-paravirtualized-drivers
tags:
- Networking
- Storage
- Virtualization
- VMware
- vSphere
title: Clearing Up the Bad Reputation of VMware's Paravirtualized Drivers
url: /2011/01/20/clearing-up-the-bad-reputation-of-vmwares-paravirtualized-drivers/
wordpress_id: 2222
---

In 2009, not too long after the release of VMware vSphere 4.0, I blogged about the use of PVSCSI and VMXNET3; specifically, I mentioned [reasons not to use PVSCSI and VMXNET3][1]. A lot has changed since then, so---prompted by a reader who shall remain nameless but knows who he/she is---I thought it might be prudent or useful to post a brief update.

While vSphere 4.0 did not support either PVSCSI or VMXNET3 for use with VMware Fault Tolerance (FT), those restrictions were lifted with the release of VMware vSphere 4.1. This was mentioned a couple of times in the comments to the original article, but I did want to clarify it so that readers knew for sure. For more information, see pages 35 and 36 of the [VMware vSphere 4.1 Availability Guide](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_availability.pdf). PVSCSI is not explicitly called out there, but it also isn't mentioned; VMXNET3 is specifically called out as supported with VMware FT.

In addition, the recommendation against using PVSCSI with low I/O workloads was also removed with vSphere 4.1. See the brief note in the Solution section of [this VMware KB article](http://kb.vmware.com/kb/1017652).

Feel free to post any corrections or clarifications in the comments. In particular, if you have links to articles or documents with explicit mention of support for these paravirtualized drivers, feel free to share them for the benefit of all readers. Thanks!

[1]: {{< relref "2009-07-05-another-reason-not-to-use-pvscsi-or-vmxnet3.md" >}}
