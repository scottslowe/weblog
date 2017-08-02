---
author: slowe
categories: Liveblog
comments: true
date: 2008-12-01T11:04:25Z
slug: 4198-hyper-v-on-netapp-deep-dive
tags:
- FibreChannel
- HyperV
- Insight2008
- iSCSI
- Microsoft
- NetApp
- SAN
- Storage
- Virtualization
- Windows
title: '4198: Hyper-V on NetApp Deep Dive'
url: /2008/12/01/4198-hyper-v-on-netapp-deep-dive/
wordpress_id: 1071
---

This session provided information on running Hyper-V with NetApp storage. The first part of the session focused primarily on Hyper-V basics, such as VHD types (dynamically-expanding, fixed-size, passthrough, differencing), partition alignment (which can only be guaranteed with fixed-size VHDs, by the way), SCVMM 2008, Windows Failover Clustering support, and such. If you're interested in details on those topics, I suggest you have a look at [my coverage][1] of Microsoft Tech-Ed 2008 back in the summer.

The second part of the session delved into some NetApp-specific information:

* NetApp has a PVR-only tool called HyperVIBE that helps to coordinate storage array Snapshots with the hypervisor, providing VSS integration to quiesce the VMs before taking a Snapshot on the NetApp array. This is only supported on Server Core and requires a special release of SnapDrive 6.0. (It's only available via PVR, so don't go searching the NetApp web site for a free download.)

* The various members of the SnapManager family---SnapManager for SQL, SnapManager for Exchange, and SnapManager for Sharepoint---are all fully supported on Hyper-V, but only for iSCSI LUNs.

* NetApp SnapDrive 6.x is supported both on Hyper-V hosts as well as guest VMs. On the parent partition, it can manage both Fibre Channel LUNs and iSCSI LUNs; on a child partition, it can only manage iSCSI LUNs.

* Version 5.x of the Host Utilities Kit is strongly recommended for use with Hyper-V, and supports Fibre Channel, iSCSI, and mixed connections. It runs on either the parent or child partition, although it seems to me that it would only make sense to run it on the parent partition.

* Data ONTAP DSM 3.2R1 is the supported and recommended DSM for MPIO support with Hyper-V. On the parent partition, it supports and manages Fibre Channel, iSCSI, and mixed paths, but in a child partition it only supports iSCSI paths. It's also only supported in child partitions running a server OS (so no Windows XP or Windows Vista support in child partitions).

For more information, readers can refer to [TR-3701](http://www.netapp.com/us/library/technical-reports/tr-3701.html) and [TR-3702](http://www.netapp.com/us/library/technical-reports/tr-3702.html). Note that updated versions of TR-3702 are expected to be released in the coming months to address additional product integrations.

[1]: /tags/teched2008/
