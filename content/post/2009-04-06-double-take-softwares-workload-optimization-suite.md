---
author: slowe
categories: News
comments: true
date: 2009-04-06T15:00:40Z
slug: double-take-softwares-workload-optimization-suite
tags:
- ESX
- HyperV
- Microsoft
- Virtualization
- VMware
- Windows
title: Double-Take Software's Workload Optimization Suite
url: /2009/04/06/double-take-softwares-workload-optimization-suite/
wordpress_id: 1268
---

Today Double-Take Software announces their new Workload Optimization Suite. All of Double-Take's flagship software products are now organized and unified within the idea of workload optimization, and new products are being announced to help provide a more complete set of solutions.

The products in the Workload Optimization Suite include:

* Double-Take Move
* Double-Take Flex
* Double-Take Backup
* Double-Take Availability

Of these four products, two of them---Move and Flex---are new products also being announced today. These new products are available today. Double-Take Backup and Double-Take Availability are available under existing Double-Take, Livewire, TimeData, and GeoCluster brands. For example, Double-Take for Windows will fold into the Double-Take Availability and Double-Take Backup products later this year with an update.

Double-Take Move is one of the new products that was announced today. Double-Take Move leverages Double-Take's existing replication technologies to provide a platform-independent X2X migration engine. X2X means any-to-any: physical-to-virtual (P2V), physical-to-physical (P2P), virtual-to-physical (V2P), and virtual-to-virtual (V2V) are all supported. In addition, Double-Take Move will automatically create a VMware ESX or Microsoft Hyper-V virtual machine when the destination is a virtual machine. Pricing is available per-use or for an entire site. Personally, I've never been a fan of tools that are licensed on a per-use basis, but with a product like this there are only so many ways to license it. For larger projects, I hope Double-Take's site licensing won't be too unattractive.

The second new product, Double-Take Flex, allows organizations to boot systems via an iSCSI SAN without the need for expensive iSCSI hardware initiators. Any ordinary Ethernet NIC within a server or desktop can be used to iSCSI boot and enable diskless operation. In addition, Double-Take Flex contains an iSCSI target for Windows Server, allowing smaller organizations to build their own iSCSI SANs. When using the Double-Take Flex iSCSI target, organizations also gain the ability to share base images, so that the storage requirements are greatly reduced and management of the OS image is simplified. I would love to have seen this sort of support with enterprise iSCSI targets like those provided by NetApp, EMC, HP, and others, but for now the shared image support is limited to Double-Take's own iSCSI target implementation. Double-Take assured me that APIs are available to allow other vendors to add this support to their systems; only time will tell if anyone actually takes advantage of those APIs.

Both Double-Take Move and Double-Take Flex provide management consoles for ease of administration.

More information on Double-Take Move, Double-Take Flex, and the rest of the Workload Optimization Suite is available from [Double-Take's web site](http://www.doubletake.com/).
