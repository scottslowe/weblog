---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-11T14:31:59Z
slug: vir250-advanced-storage-connectivity-for-vms
tags:
- HyperV
- Microsoft
- SAN
- Storage
- TechEd2008
- Virtualization
- Windows
title: 'VIR250: Advanced Storage Connectivity for VMs'
url: /2008/06/11/vir250-advanced-storage-connectivity-for-vms/
wordpress_id: 734
---

I'll do my best to liveblog as much content from this session as possible, but I arrived late (you'll understand why soon) and my battery is running low. Sorry all!

I arrived in the session as the speaker was reviewing some Hyper-V architecture with regards to the placement of the storage drivers and the use of Virtualization Service Clients (VSCs) and Virtualization Service Providers (VSPs). I think I covered those in my earlier [liveblogging session from Day 1][1]; if I didn't, I'll revise this post with more information later.

One key limitation with virtualized servers with regards to storage is how to handle Fibre Channel; you can't really have multiple VMs using the same HBA because they would all share the same WWPN (World Wide Port Name), and SAN zoning is all built on the use of WWPNs. NPIV (N_Port ID Virtualization) is the answer here, which allows an HBA to register multiple fabric addresses. Each VM can use one of those multiple addresses. This allows us to return to zoning on a per server/per VM basis and eliminates this problem.

Next the presenter discussed the idea of virtual fabrics, a T.11 standard implemented by Cisco as VSAN and by Brocade as LSAN. Each HBA can only reside in a single virtual fabric, but traffic can be routed to other fabrics as needed.

Regarding virtual HBAs and fabric QoS, this is handled on a per-initiator WWPN basis. (I guess this means we can apply QoS to individual VMs when using NPIV, then?) I'm not exactly sure why the presenter is discussing this particular topic; it doesn't seem to tie back to the main topic of storage connectivity for virtual machines.

So what is required to do virtual HBAs (again, I'm assuming this means using NPIV to present a virtual HBA to a Hyper-V hosted VM)? Next month, a newer version of Emulex VMPilot and the Storport Miniport driver will provide support for Windows Server 2003 SP2 and Hyper-V on Windows Server 2008. This will be compatible with 4Gpbs HBA from Emulex. Depending upon the HBA, the number of virtual ports (VPorts) may be limited (only 16 VPorts on midrange 4Gbps HBAs, for example).

There is no performance impact for using virtual HBAs, by the way. The number is so small as to defy measurement.

You also need FC switch support for NPIV. This requirement only applies to "edge switches" where NPIV-enabled HBAs will actually connect. There are also no other requirements on storage devices.

Best practices for storage access:

* For consolidating file/print servers or desktops, place all the VMs on a single shared LUN. (Note that this prevents Quick Migration.) You can't use fabric QoS on a per-VM basis also, because all the VMs share the same WWPN of the physical HBA.

* For virtualizing application servers, use a dedicated LUN for each VM. This will allow the use of Quick Migration and can used virtual HBA, LUNs can be masked only to this specific VM, and you can create single-initiator zones using the virtual HBA and the applicable storage targets.

System Center Virtual Machine Manager (SCVMM) supports VDS (Virtual Disk Service) for Fibre Channel and iSCSI. VMM allows partners to extend built-in functionality by exposing NPIV WMI interfaces and allowing NPIV WMI methods to enumerate, create, and delete VPorts. In addition, VMM automatically manages VM and VPort migration in SANs.

Emulex VMPilot is the UI for SAN configuration with VMM. The current version is version 1.1; version 1.2 will be released next month and will provide support for Hyper-V. VMPilot with VMM enables VPort creation according to the T.11 NPIV standard, provides a graphical and command line interface, and provides automatic generation of virtual WWPNs. (I need to stop by the Emulex booth downstairs to get more information on this.) VMPilot also supplies a VMM PRO pack to report HBA health up to System Center Operations Manager and then back to SCVMM.

Recapping the benefits:

* LUN optimization through VM to LUN assignment

* Fabric QoS and prioritization at the VM level

* Single-initiator zoning possible, returns to a storage best practice

* Array-level LUN masking to control LUN access on a per-VM basis

* Accelerated VM migration (not sure about this one)

* VSAN integration and routing

* Eliminates duplication of storage administration tasks

I think that's about going to do it for this liveblog; my battery is almost gone. I'll be back with more information as soon as possible!

[1]: {{< relref "2008-06-10-vir367-hyper-v-security-and-best-practices.md" >}}
