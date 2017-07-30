---
author: slowe
categories: News
comments: true
date: 2008-06-04T21:41:22Z
slug: storage-array-snapshots-with-vmware
tags:
- Snapshots
- Storage
- Virtualization
- VMware
title: Storage Array Snapshots with VMware
url: /2008/06/04/storage-array-snapshots-with-vmware/
wordpress_id: 722
---

A new article of mine has been published on [SearchVMware.com](http://searchvmware.techtarget.com/)! This article discusses the use of [storage array snapshots with VMware](http://searchvmware.techtarget.com/tip/0,289483,sid179_gci1316225,00.html), specifically focusing on ensuring that storage array snapshots are consistent and usable:

>When used in conjunction with a VMware infrastructure, storage array-based snapshots are touted for their ability to create point-in-time pictures of virtual machines (VMs) for business continuity, disaster recovery and backups. While this can be true, it's important to understand how virtualization affects storage array snapshot use. Incorrect usage can render storage array snapshots unreliable and generally defunct.

The article provides a few guidelines on making sure that storage array snapshots are usable. Keep in mind, too, that some storage array vendors have applications that are specifically designed to help with this particular issue. NetApp, for example, has [SnapManager for Virtual Infrastructure](http://www.netapp.com/us/products/management-software/snapmanager-virtual.html); this product is specifically designed to address this problem (among other problems). I would imagine that other vendors also offer a software solution to this problem, but I'm not particularly familiar with those. I'd love to hear from readers as to their experience or knowledge with any such software solutions.
