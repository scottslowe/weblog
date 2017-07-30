---
author: slowe
categories: Review
comments: true
date: 2009-08-17T13:01:09Z
slug: netapp-rcu-and-vsc
tags:
- NetApp
- Storage
- VMware
- vSphere
title: NetApp RCU and VSC
url: /2009/08/17/netapp-rcu-and-vsc/
wordpress_id: 1533
---

I spent some time last week at the NetApp RTP office getting a special sneak preview of a couple of software products getting announced at VMworld 2009 in San Francisco. One of these is the Rapid Cloning Utility (RCU); the other is the Virtual Storage Console (VSC). Both of these software products are intended to plug into vCenter Server to provide more centralized access to both storage and virtualization management.

The Rapid Cloning Utility (RCU) has actually been around for a while, but it wasn't an officially supported tool. This new version of RCU changes that; it now becomes a free tool but an officially supported tool for NetApp customers with active support agreements. The primary purpose of the RCU is to automate the use of NetApp's FlexClone functionality for rapidly provisioning virtual machines. The scope and scale of virtual desktop deployments lends itself well to the use of RCU, but RCU could also be applicable in server virtualization environments as well.

Some of the functionality brought to the table by the RCU includes:

* Full support for cloning block-based datastores accessed via Fibre Channel or iSCSI

* Full support for file-level FlexCloning on NFS datastores

* Automated import of virtual machines into View Manager, where applicable

* Bulk import into XenDesktop, where applicable

* Ability to store virtual machine swap file in separate VMDK in a separate datastore

* Support for MetroCluster

In addition, the RCU incorporates support for deduplication (options to enable deduplication and report on it from within vCenter Server), NFS datastore resizing, creating/deleting/cloning block-based datastores, and support for basic role-based administration control (RBAC) in that user permissions are checked before tasks are launched. In addition, use of the RCU mitigates many of the drawbacks I've discussed in the past with regard to use array-based cloning in virtualized environments. All in all, it's a useful addition to vCenter Server for environments using NetApp storage.

The Virtual Storage Console (VSC) replaces the old NetApp Host Utilities Kit (HUK), which NetApp used to fine-tune and configure certain host and HBA parameters. Now those same settings can be managed from within vCenter Server. The VSC also provides access to NetApp's `mbrscan` and `mbralign` tools, which are designed to identify and correct problems with VMDK alignment.

Both of these utilities, if I recall correctly, require that you are running the very latest version of Data ONTAP.
