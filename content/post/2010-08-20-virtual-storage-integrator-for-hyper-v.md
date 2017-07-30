---
author: slowe
categories: News
comments: true
date: 2010-08-20T17:39:44Z
slug: virtual-storage-integrator-for-hyper-v
tags:
- EMC
- Microsoft
- Storage
- Virtualization
title: Virtual Storage Integrator for Hyper-V
url: /2010/08/20/virtual-storage-integrator-for-hyper-v/
wordpress_id: 2037
---

Most of my virtualization focus centers on VMware and its product portfolio, but VMware isn't the only virtualization solution in town. I'm sure they (VMware) probably wish they were the only solution in town, but competition keeps everyone on their toes. (Consider Proverbs 27:17.)

With that thought in mind, I wanted to bring everyone's attention to a new Hyper-V plug-in from EMC: the Virtual Storage Integrator (VSI) for Hyper-V. Much like VSI for vSphere, the VSI for Hyper-V provides additional visibility from System Center Virtual Machine Manager (SCVMM) into the storage layer. The VSI for Hyper-V has two components: Storage Viewer and Disaster Restart:

* The Storage Viewer component provides mappings from NTFS volumes to the underlying CLARiiON or Symmetrix devices, mappings from LUNs to VMs, and mappings from storage array to Hyper-V hosts, including array target ports. In this regard, it is quite similar to the Storage Viewer component of VSI for vSphere.

* The Disaster Restart component displays disaster recovery sites, groups of VMs online at each site, and enables live migration/quick migration of individual VMs or the ability to migrate cluster groups.

PowerShell cmdlets are available to automate the complete functionality of the VSI for Hyper-V.

If you're interested, you can download the VSI for Hyper-V **for free** from PowerLink (login needed). Here's a link to the [download on PowerLink](http://bit.ly/d8wSLp).
