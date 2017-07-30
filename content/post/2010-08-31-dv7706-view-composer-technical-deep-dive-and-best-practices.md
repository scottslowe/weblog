---
author: slowe
categories: Liveblog
comments: true
date: 2010-08-31T18:07:06Z
slug: dv7706-view-composer-technical-deep-dive-and-best-practices
tags:
- VDI
- Virtualization
- VMware
- VMworld2010
title: 'DV7706: View Composer Technical Deep Dive and Best Practices'
url: /2010/08/31/dv7706-view-composer-technical-deep-dive-and-best-practices/
wordpress_id: 2079
---

This is a liveblog of VMworld 2010 session DV7706, titled "View Composer Technical Deep Dive and Best Practices," in Moscone West 2004. The presenter(s) are Jeff Whitman and Jim Yanik, both with VMware.

We start out the session with a quick review of some limitations. View Composer has a limit of eight ESX/ESXi hosts in a cluster. This is a VMFS limit involving the number of hosts that are accessing a read-only file at the same time. I wonder if VAAI hardware-assisted locking will affect this limit. As for the total number of VMs, you are limited by the usual suspects---HA failover time, vMotion time to put a host in maintenance most, HA limits, etc.

View Composer is installed as a service on the vCenter Server computer. You can connect View Manager to the View Composer service inside the View Manager configuration dialog box. The presenters do recommend using the fully qualified domain name (FQDN) when configuring the connection between the View Manager and the View Composer service on a vCenter Server instance.

The start of every linked clone is the parent VM. Follow the usual best practices for building the parent VM as included in the documentation from VMware. I couldn't record any of their recommendations because they didn't leave on the screen long enough.

The parent VM needs a snapshot before you can create linked clones. Be sure to shut down the VM so that memory state isn't included. View 4.5 has a new checkbox that allows you to show incompatible images; this was added as a way to help administrators troubleshoot potential problems with incorrectly-taken snapshots. (As an example, a snapshot taken while the VM is running would be incompatible.)

Linked clones can be stored on local or shared storage. You can have multiple linked clones per storage pool, and replica and linked clones can be on the same datastore or different datastores. This is new to View 4.5 and it allows you to store the replica on SSD/EFD for maximum performance but place the linked clone on slower-performing storage. Be aware that this is a potential single point of failure.

View terminology appears to be changing again; what was once the user data disk is now called the persistent disk. In my opinion, VMware needs to settle into some consistent terminology.

Some datastore recommendations include using similarly-sized datastores so that View can load balance the linked clones across the datastores (using round robin) fairly evenly. The number of VMs per datastore is really driven by IOPS; best practices run around "50-64 or maybe 128" (exact verbiage from the presentation).

Quick definition: A _replica_ consists of a clone of the parent VM plus the selected snapshot. Replicas are thin provisioned. Persistent disks (aka user data disks) are also thin provisioned. View 4.5 also introduces a "disposable" or temporary disk that allows View 4.5 to destroy the temporary disk and reclaim that space on a regular basis. The presenters think that the temporary disk is destroyed every time the user logs off. How does it handle the Windows swapfile then? Finally, View 4.5 also stored the Windows machine password in a separate "internal disk" that simplifies the process of refreshing linked clones when they are member of an Active Directory domain.

The presenter next walks through a comparison of storage utilization both without and with linked clones. It's a comparison that most people have seen multiple times, nothing terribly new or surprising here.

QuickPrep is included with View Composer, and 4.5 also includes Sysprep. You should use Sysprep only in those instances where you specifically need a new SID; in most cases, having a unique SID isn't as big of a deal as many people suspect that it is. Sysprep is a lot slower than QuickPrep, so be aware. The selection of QuickPrep/Sysprep on a pool is permanent for the life of that pool; you can't switch it later.

VDMAdmin.exe is a tool provided with View Manager; it was necessary with previous versions of View to attach/detach user data disks. Persistent disks (the equivalent in View 4.5) can be managed directly inside the View Manager GUI. You can also script the interaction with the persistent disks for greater automation.

The speakers just confirmed, as I already knew, that centralized profile management is not included in VMware View 4.5.

Some troubleshooting tips:

* All machines have same name and hang on customization - typically caused by a missing agent.

* If customization fails, check the QuickPrep domain setup in View Manager, Also be sure user has permissions to add and remove computers in Active Directory.

* DNS, DNS, DNS---name reoslution is critical!

* Be sure that you have adequate host resources for large refresh or recompose operations.

* Use View Manager to manipulate View desktops, not vCenter!

* Don't use static IP addressing in the parent VM.

* Use SVIConfig to help troubleshoot View database issues.

You can't use Storage vMotion with linked clones; it's not supported.

What's the best way to handle Patch Tuesday? You can manually apply the patches, test, snapshot, and then recompose. You can also use automatic updates, test, power down and snapshot, and then recompose. Finally, if you are using a third-party agent, remove the agent before snapshotting and recomposing (you don't want the agent included in the linked clones).

What about antivirus? The traditional method was to install the A/V engine and update definitions only; you would use a recompose to roll out a new engine. You could also _not_ use A/V. Because linked clones are disposable, the impact of not using A/V isn't as great as you might initially think. With vSphere 4.1 you could use vShield Endpoint, which is an extension of the VMsafe APIs that allow the A/V vendors to completely pull their agents out of the guest VMs.

When planning for business continuity, don't forget to plan for the View Manager database. For DR, be sure to replicate the View Server and install View Composer on the DR vCenter Server.

That's it!
