---
author: slowe
categories: Liveblog
comments: true
date: 2009-05-06T17:37:19Z
slug: iforum-218-what-the-other-virtualization-guys-dont-want-you-to-know
tags:
- Citrix
- HyperV
- Storage
- Virtualization
- Xen
title: 'iForum 218: What the Other Virtualization Guys Don''t Want You to Know'
url: /2009/05/06/iforum-218-what-the-other-virtualization-guys-dont-want-you-to-know/
wordpress_id: 1321
---

This is iForum 218, titled "XenServer 5: What the Other Virtualization Guys Don't Want You to Know". The presenters are Roger Klorese and Jill Skok.

Klorese starts out the presentation with XenCenter, the centralized multi-node management tool that ships with XenServer. As I've noted here on this site before, XenCenter's real-time replication and multi-node behavior is, I believe, superior to the highly-centralized model that VMware uses with vCenter Server. He then goes through some of the features available in XenServer (which Klorese tells us really means "XenServer + Citrix Essentials for XenServer") like XenMotion, High Availability, Dynamic Provisioning Services (which I believe refers to Citrix Provisioning Server aka Ardence), and Lab Management (OEM'd from VMlogix).

XenServer 5.5 is in beta currently. What are the features introduced in XenServer 5.5?

* Active Directory authentication: This eliminates the need to login as root to manage XenServers via XenCenter. The granularity is a bit limited at the moment, but it is a big step forward.

* Workload balancing: This feature, similar to VMware DRS, includes both live workload balancing as well as optimized (or intelligent) placement. Workload balancing is policy-driven, allowing users to select maximum performance or maximum density. Workload balancing can make decisions not only on CPU and memory, but also on network and disk statistics. Workload balancing does require a separate, Windows-based server (is this server in addition to the server running Essentials?).

* Enhancements to the Xen hypervisor to add support for Intel EPT and AMD RVI virtualization extensions.

* Worklow automation and orchestration: Workflow Studio gets incorporated into some editions of Citrix Essentials. I'm not sure how this is a new feature of XenServer 5.5.

* XenCenter now adds organization view, to provide a different view of objects within XenCenter. This lays the foundation for more role-based administration and role-based views.

Klorese then moves into a discussion of XenServer's storage integration technologies. This is StorageLink. Underneath it, XenServer underwent some changes. This enables snapshots on all types of storage repositories. A new feature, called LVHD, brings VHD layout to existing LVM storage repositories. This is a fast and simple upgrade to add new features.

Another new storage-related feature is backup enablement with Symantec NetBackup. From Klorese's description, this essentially sounds like VMware Consolidated Backup (VCB)--in other words, it's not a backup solution but a framework for enabling backups with other products. When XenServer 5.5 is finally released, there will be documentation and best practices available to configure this backup enablement with NetBackup.

Delving back into StorageLink, Klorese describes some of the functionality of StorageLink. StorageLink requires a separate server (this may share hardware with the Workload Balancing server mentioned earlier) in order to function; this is called the StorageLink Gateway Server. StorageLink Manager is where you can manage and configure StorageLink. Most of what is done in StorageLink is handled inside XenCenter (you do have to use StorageLink Manager when using Essentials with Hyper-V). Use of the command-line interface (CLI) is required for initial setup of StorageLink with the storage array.

StorageLink is evolving into Citrix Ready Open Storage, which opens up StorageLink to work with many more different storage vendors and their functionality with XenServer 5.5. Products that participate in Citrix Ready Open Storage will work with Essentials for XenServer as well as Essentials for Hyper-V.

What about VM portability? Klorese indicates that multi-hypervisor interoperability is made possible by StorageLink and Citrix Essentials. This allows a VM to be moved between XenServer and Hyper-V. Klorese also mentions XenConvert 2.0, which provides extensive P2V and V2V functionality.

The next portion of Klorese's discussion focuses on total cost of ownership (TCO) of XenServer versus other virtualization solutions. Naturally, VMware is in his targets here (as fully expected). Klorese feels that free XenServer with a support contract meets the needs of the majority of the users. For all the other users, Citrix Essentials provides workload balancing, high availability, StorageLink, etc. It would be interesting to me to see the pricing of XenServer plus Citrix Essentials versus VMware Infrastructure 3 (or VMware vSphere 4).

As if Klorese read my mind, the next slide is exactly that. Although he doesn't mention VMware by name, it's clear that's who they are talking about. Some points Klorese mentions are absolutely valid---using more RAM in the servers instead of worrying about memory overcommitment may make a lot of sense in some cases---other points aren't quite so clear. For example, Klorese lists almost 10x the "advanced virtualization management" costs for the opponent, but not for Citrix XenServer. The basis for that is that High Availability and other advanced features are needed by all the servers in the farm, therefore there's no need to license them and you can save money by not buying those licenses. In my mind, that's a weighted comparison.

At this point, Jill Skok of Accenture takes the podium. Her discussion is about building a virtualization practice on XenServer. I was most interested in the technical aspects of the XenServer discussion, so at this point I wrapped up my coverage.
