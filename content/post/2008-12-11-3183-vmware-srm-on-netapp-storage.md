---
author: slowe
categories: Liveblog
comments: true
date: 2008-12-11T09:20:28Z
slug: 3183-vmware-srm-on-netapp-storage
tags:
- Insight2008
- NetApp
- Storage
- Virtualization
- VMware
- VMwareSRM
title: '3183: VMware SRM on NetApp Storage'
url: /2008/12/11/3183-vmware-srm-on-netapp-storage/
wordpress_id: 1091
---

This session described VMware Site Recovery Manager (SRM) on NetApp storage. The session started out with a review of VMware SRM, its features and functionality, and some of the requirements. I was not aware, for example, that SRM cannot use SQL Server Express like VirtualCenter can; you must use a full-blown instance of SQL Server. Given VMware's development history, I should not have been surprised to find that Perl 5.8 is required (it's included in the distribution and installed automatically).

On the NetApp side, it's important to note that users must first configure SecureAdmin in order for VMware SRM to use HTTPS when communicating with the NetApp storage arrays. If this isn't done first, then the NetApp Site Recovery Adapter (SRA) will drop back to plain HTTP. The storage controllers must also have licenses for SnapMirror, iSCSI (included with the storage controllers), FCP (where applicable), and FlexClone. Without FlexClone, it's impossible to do failover testing. NetApp again re-iterated that they anticipate seeing NFS support in VMware SRM somewhere in the March 2009 timeframe.

Note that there is no support for SnapVault or MetroCluster in SRM, although there are some interesting synergies between MetroCluster and VMware HA that are being explored. It will be interesting to see where, if anywhere, that may lead. NetApp admins may use either Volume SnapMirror (VSM) or Qtree SnapMirror (QSM), although VSM is preferred since it preserves deduplication with replication. QSM does not.

The presenters referred attendees to [TR-3671](http://www.netapp.com/us/library/technical-reports/tr-3671.html), "VMware Site Recovery Manager in a NetApp Environment," for more detailed information.

At the Recovery Site, users must configure an additional, non-replicated datastore. This additional datastore does not have to be very large, but it's required for storing the "shadow VMs" (or "placeholder VMs") that are created and maintained by VMware SRM.

At present, there is no integration between SnapManager for Virtual Infrastructure (SMVI) and VMware SRM. There are numerous technical questions, and I'm not entirely sure that I fully understand the implications just yet. This will be an area that I will be exploring further so that I can better understand the considerations of using these technologies together. NetApp is working with VMware to try to resolve some of the technical concerns around SMVI-SRM integration, but that will take some time. In other words, don't hold your breath.

Finally, if you've downloaded the NetApp SRA prior to the last week or so (this was back in the middle of November), download it again. There were some issues fixed that have been addressed in a more recent release of the SRA. Unfortunately, VMware would not let NetApp increment the version number on the SRA, so it's a bit difficult to tell what version you are running. If anyone has more information on that---I don't recall or have any notes from the session on how to do this---it would be greatly appreciated.

Other miscellaneous notes from the session:

* There are issues backing up a VMware SRM recovery plan; it's not currently possible to export it to CSV/XML and then import it back in again)

* VMware SRM and the NetApp SRA support dissimilar protocols between the Protected and Recovery Sites (e.g., FCP at Protected and iSCSI at Recovery) and dissimilar storage (e.g., FC disks at Protected and SATA disks at Recovery)

* The appropriate iGroups must exist at the Recovery Site and the VMware ESX servers must be in the correct iGroups, but VMware SRM will handle mapping the LUNs to the iGroups

I think that's all I have for this session. If any other session attendees have more information, please add it in the comments below.
