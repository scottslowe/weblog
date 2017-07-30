---
author: slowe
categories: News
comments: true
date: 2008-09-15T09:02:34Z
slug: citrix-announces-xenserver-5
tags:
- Citrix
- Microsoft
- Virtualization
- Xen
title: Citrix Announces XenServer 5
url: /2008/09/15/citrix-announces-xenserver-5/
wordpress_id: 896
---

Today Citrix made a couple of significant product announcements. Citrix announced details around the next version of their Xen-based virtualization solution, XenServer, now at version 5.

XenServer 5 includes version 3.2 of the Xen hypervisor---Xen 3.3 was released only a few weeks before XenServer 5--and adds a few new features:

* XenServer HA provides protection against host-level failures, similar to VMware HA, but settable on a per-VM basis

* New APIs to allow applications to quiesce VMs for the purposes of cloning, replication, snapshots, or backups

* XenCenter offers the ability to assign metadata and virtual tags to virtual machines and workloads; these tags can then be used to easily locate resources within large deployments, to create policies, and to deliver service level reporting

* XenCenter also adds persistent performance monitoring to allow for a historical view of virtual machines and physical host performance

The product editions for XenServer 5 continue as with previous versions of the product. XenServer Express is free (like ESXi and Hyper-V Server), but provides only basic virtualization functionality. XenServer Standard includes resource pools. Although these share a name with a feature provided by VMware Infrastructure, they aren't the same; think of XenServer resource pools as configuration pools. Enterprise Edition adds XenMotion (live migration), and the Platinum Edition adds Citrix Provisioning Server. XenCenter is included with all editions at no additional charge, and as with previous versions operates in a "multi-master" sort of fashion to prevent a single point of failure.

In addition, XenServer 5 has been validated by Microsoft under the Server Virtualization Validation Program (SVVP), allowing Citrix and Microsoft to jointly support Microsoft operating systems and applications running on Citrix XenServer 5.
