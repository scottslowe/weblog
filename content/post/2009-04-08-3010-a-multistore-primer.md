---
author: slowe
categories: Liveblog
comments: true
date: 2009-04-08T12:00:02Z
slug: 3010-a-multistore-primer
tags:
- ActiveDirectory
- CLI
- Deduplication
- Insight2008
- iSCSI
- NetApp
- NFS
- ONTAP
- SAN
- Storage
- Virtualization
- VMware
- WAFL
title: '3010: A MultiStore Primer'
url: /2009/04/08/3010-a-multistore-primer/
wordpress_id: 1297
---

This session describes NetApp's MultiStore functionality. MultiStore is the name given to NetApp's functionality for secure logical partitioning of network and storage resources. The presenters for the session are Roger Weeks, TME with NetApp, and Scott Gelb with Insight Investments.

When using MultiStore, the basic building block is the _vFiler_. A vFiler is a logical construct within Data ONTAP that contains a lightweight instance of the Data ONTAP multi-protocol server. vFilers provide the ability to securely partition both storage resources and network resources. Storage resources are partitioned at either the FlexVol or Qtree level; it's recommended to use FlexVols instead of Qtrees. (The presenters did not provide any further information beyond that recommendation. Do any readers have more information?) On the network side, the resources that can be logically partitioned are IP addresses, VLANs, VIFs, and IPspaces (logical routing tables).

Some reasons to use vFilers would include storage consolidation, seamless data migration, simple disaster recovery, or better workload management. MultiStore integrates with SnapMirror to provide some of the functionality needed for some of these use cases.

MultiStore uses vFiler0 to denote the physical hardware, and vFiler0 "owns" all the physical storage resources. You can create up to 64 vFiler instances, and active/active clustered configurations can support up to 130 vFiler instances (128 vFilers plus 2 vFiler0 instances) during a takeover scenario.

Each vFiler stores its configuration in a separate FlexVol (it's own root vol, if you will). All the major protocols are supported within a vFiler context: NFS, CIFS, iSCSI, HTTP, and NDMP. Fibre Channel is not supported; you can only use Fibre Channel with vFiler0. This is due to the lack of NPIV support within Data ONTAP 7. (It's theoretically possible, then, that if/when NetApp adds NPIV support to Data ONTAP that Fibre Channel would be supported within vFiler instances.)

Although it is possible to move resources between vFiler0 and a separate vFiler instance, doing so may impact client connections.

Managing vFilers appears to be the current weak spot; you can manage vFiler instances using the Data ONTAP CLI, but vFiler instances don't have an interactive shell. Therefore, you have to direct commands to vFiler instances via SSH or RSH or using the vFiler context in vFiler0. You access the vFiler context by prepending the `vfiler` keyword to the commands at the CLI in vFiler0. Operations Manager 3.7 and Provisioning Manager can manage vFiler instances; FilerView can start, stop, or delete individual vFiler instances but cannot direct commands to an individual vFiler. If you need to manage CIFS on a vFiler instance, you can use the Computer Management MMC console to connect remotely to that vFiler instance to manage shares and share permissions, just as you can with vFiler0 (assuming CIFS is running within the vFiler, of course).

IPspaces are a logical routing construct that allow each vFiler to have its own routing table. For example, you may have a DMZ vFiler and an internal vFiler, each with their own, separate routing table. Up to 101 IPspaces are supported per controller. You can't delete the default IPspace, as it's the routing table for vFiler0. It is recommended to use VLANs and/or VIFs with IPspaces as a best practice.

One of the real advantages of using MultiStore and vFilers is the data migration and disaster recovery functionality that it enables when used in conjunction with SnapMirror. There are two sides to this:

* `vfiler migrate` allows you to move an entire vFiler instance, including all data and configuration, from one physical storage system to another physical storage system. You can keep the same IP address or change the IP address. All other network identification remains the same: NetBIOS name, host name, etc., so the vFiler should look exactly the same across the network after the migration as it did before the migration.

* `vfiler dr` is similar to `vfiler migrate` but uses SnapMirror to keep the source and target vFiler instances in sync with each other.

It makes sense, but you can't use `vfiler dr` or `vfiler migrate` on vFiler0 (the physical storage system). My own personal thought regarding `vfiler dr`: what would this look like in a VMware environment using NFS? There could be some interesting possibilities there.

With regard to security, a Matasano security audit was performed and the results showed that there were no vulnerabilities that would allow "data leakage" between vFiler instances. This means that it's OK to run a DMZ vFiler and an internal vFiler on the same physical system; the separation is strong enough.

Other points of interest:

* Each vFiler adds about 400K of system memory, so keep that in mind when creating additional vFiler instances.

* You can't put more load on a MultiStore-enabled system than a non-MultiStore-enabled system. The ability to create logical vFilers doesn't mean the physical storage system can suddenly handle more IOPS or more capacity.

* You can use FlexShare on a MultiStore-enabled system to adjust priorities for the FlexVols assigned to various vFiler instances.

* As of Data ONTAP 7.2, SnapMirror relationships created in a vFiler context are preserved during a "vfiler migrate" or "vfiler dr" operation.

* More enhancements are planned for Data ONTAP 7.3, including deduplication support, SnapDrive 5.0 or higher support for iSCSI with vFiler instances, SnapVault additions, and SnapLock support.

Some of the potential use cases for MultiStore include file services consolidation (allows you to preserve file server identification onto separate vFiler instances), data migration, and disaster recovery. You might also use MultiStore if you needed support for multiple Active Directory domains with CIFS.

**UPDATE:** Apparently, my recollection of the presenters' information was incorrect, and FTP is not a protocol supported with vFilers. I've updated the article accordingly.
