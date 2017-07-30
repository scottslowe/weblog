---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-11T09:33:04Z
slug: vir253-microsoft-system-center-vmm-2008-part-1-of-2
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: 'VIR253: Microsoft System Center VMM 2008, Part 1 of 2'
url: /2008/06/11/vir253-microsoft-system-center-vmm-2008-part-1-of-2/
wordpress_id: 731
---

It's Day 2 of Tech-Ed 2008, and my first session of the day is VIR253, Microsoft System Center Virtual Machine Manager 2008 Overview, Part 1 of 2. Part 2 will be later this morning. I've got a seat right by the doors to the breakout session, so I'm hoping that the wireless LAN will hold out for liveblogging the session.

I've actually been here at the conference center since 7:00 AM this morning; I spent some time in the Hands-On Labs this morning going through Hyper-V setup, configuration, Authorization Manager, etc. Useful stuff; I'll blog more on that later.

The presenter, Edwin Yuen, starts with defining virtualization as the "isolation of one computing resource from the other." This is a good generic definition and allows us to view virtualization as more than just server virtualization. It allows us to extend the idea of virtualization to things like VPNs (which I've long described as a form of network virtualization), virtual storage, virtual presentation (think Terminal Services). While virtualization creates flexibility, it can also create issues. Edwin states that "Virtual machines are machines first, virtual second." That's an important point to remember---while virtual machines can be very useful and extremely flexible, they are still machines that must be managed, monitored, and supported.

Edwin then reviews Microsoft's virtualization product suite, encompassing server virtualization (Hyper-V), application virtualization (SoftGrid), desktop virtualization (VirtualPC), and presentation virtualization (Terminal Services).

In reviewing Microsoft's "growing momentum" for virtualization, Edwin reminded attendees that as of last Friday, Microsoft released a patch that will fix SCVMM 2008 Beta 1 to allow it to manage Hyper-V RC1. There's also a disk available in the Technical Learning Center (TLC) downstairs in the conference center.

Next we discuss the four major members of the System Center family that apply to a virtualization environment. SCVMM, of course, has already been mentioned. There's also System Center Configuration Manager (SCCM) for patch management and deployment, software upgrades, and OS and application configuration management; System Center Operations Manager (SCOM), which provides end-to-end service management, server and application health monitoring and management, and performance reporting and analysis; and System Center Data Protection Manager (SCDPM), which enables the VSS support for live VM backups with in-guest consistency. However, the focus of this session is SCVMM 2008.

Key things to remember about SCVMM 2008 is the management of VMware ESX servers through VirtualCenter (VirtualCenter is still required to manage ESX servers via SCVMM 2008) as well as Virtual Server and Hyper-V. SCVMM supports intelligent placement, although I believe at startup only since there is no live migration support within any MS virtualization product. SCVMM 2008 also provides P2V and V2V support. Edwin also mentions that you can use SCVMM's P2V (what he likes to call "VP2V") to convert a VMware VM to a Hyper-V VM _live_. There's also application and service-level monitoring integration with SCOM; the idea of Performance and Resource Optimization (PRO) came up again. More on PRO will be available in Part 2 later this morning. The presenter reinforced again that SCVMM 2008 is completely built on PowerShell, and that there are some things you can do in PowerShell that can't be done in the SCVMM GUI. And the PowerShell scripts will generally work equally against both Hyper-V and managed VMware hosts.

(Note to readers: You may see some information overlap from sessions I attended yesterday. Sorry about that; I'm trying to present each session as completely as possible. Just ignore repeated information wherever you can.)

RTM for SCVMM 2008 is dependent upon the RTM for Hyper-V; projected RTM for SCVMM 2008 is expected in the Q3/Q4 timeframe.

Edwin next launched into a live demo of the SCVMM 2008 GUI. SCVMM allows you to maintain the VirtualCenter logical groups, allowing users to place Hyper-V hosts and ESX hosts in the same logical groupings. The list of hosts and virtual machines can be grouped by guest operating system, or date added, or a custom tag, and again this is true for VMs on either virtualization platform. SCVMM 2008 also offers a quick "snapshot" of the selected VM at the bottom, including a Windows Task Manager-style overview of current CPU usage. Many of the UI elements are hyperlinked and take you to other areas of the application for more information or more details. Overall, the UI looks pretty nice, although I'm not necessarily a huge fan of the three-pane interface that Microsoft has adopted with Windows Server 2008 and the latest version of MMC.

SCVMM integrates Sysprep, so that templates can be customized not only with regard to virtual hardware, but also with regard to the guest OS parameters as well. This brings SCVMM's templates on par with VirtualCenter.

Edwin again shows off the automatic generation of PowerShell scripts. At the end of each wizard (and all GUI operations are wizard-driven) there's an option to display the underlying PowerShell script. PowerShell scripts can be modified and customized, and then added to the SCVMM library to be run whenever needed.

As for SCVMM libraries, these are just ordinary file shares. This means that the placement of SCVMM libraries should be carefully considered in the design and implementation of SCVMM; you don't want to be pulling library information across the WAN.

Next Edwin displays the conversion capabilities of SCVMM 2008 by performing a "VP2V". This installs an agent into the VM that allows the conversion to take place while the source system is live. In the conversion process, we can select which drives we'd like to convert (the C: drive is always required) or we can modify the number of CPUs or the memory assigned to the VM. The P2V process, like everything else in SCVMM 2008, is driven by PowerShell and can optionally display a PowerShell script at the end of the GUI wizard.

SCVMM also has a web interface that allows for delegated administration, so that certain tasks can be delegated out to end users. This can allow business owners to have the ability to reboot a virtual machine, but not necessarily change the configuration of the VM. In addition, the web interface can also give end users console access to the VMs as well. VM provisioning, with limits via policy, is also available.

Next we moved on to discuss VMware management in more detail. It's driven by customer demand and is intended to provide a unified management experience, for both physical and virtual, Hyper-V and VMware. The SCVMM feature set encompasses the ability to invoke VMotion, manage Resource Pools, etc. SCVMM can perform intelligent placement against VMware servers (I'm guessing that this does _not_ leverage VirtualCenter's existing intelligent placement algorithms), use PowerShell scripts, etc. SCVMM can manage multiple VirtualCenter instances, allowing users to consolidate multiple VCs and become a "manager of managers."

When dealing with Windows Server-based hosts, SCVMM can remotely enable the Hyper-V role, which makes it easier to add Hyper-V hosts to SCVMM. Entire Hyper-V clusters can be added in a single step, and SCVMM will automatically detect node additions/removals. SCVMM also provides enhanced management of Hyper-V clusters.

The SCVMM library can contain VHDs, ISOs, offline VMs (difference between VHDs and offline VMs?), and PowerShell scripts. The SCVMM library is just a file share.

SCVMM's conversion functionality supports live conversions for Windows Server 2003, Windows Server 2008, Windows XP, and Windows Vista. Offline conversions are available for Windows 2000 Server. Both SMP and x64 sources are also supported. The entire process is automatable via PowerShell.

Edwin next presented some more detail on user roles, delegated administrators, self-service users, the default profiles available in SCVMM 2008, and how they are used. The scope for delegated administrators and self-service users can be modified, but the scope for administrators can't be changed.

Intelligent placement uses some of the capacity planning functionality in SCVMM along with some weightings of the various resource requirements. SCVMM performs checks to ensure that potential target hosts are even capable of running the VM, i.e., it won't recommend an x64 VM on an x86 host. The star rating is generated by comparing available resources, used resources, and the weightings to create the final recommendation. Based on this information, it looks definitely like SCVMM does not leverage any VirtualCenter intelligent placement functionality when managing VMware hosts.

PowerShell is the SCVMM API; that is how you access SCVMM functionality from other applications or from scripts. Edwin showed some simple examples of PowerShell scripts.

When deploying VMs via the LAN, BITS is used. Hyper-V uses Quick Migration; VMware uses VMotion. NPIV support is included, which will allow VMs to independently zoned. VDS support is included for both Fibre Channel and iSCSI SANs. I hope to get more information on the VDS support and NPIV in a session later today on advanced storage connectivity for VMs.

Part of the SCVMM integration with SCCM is the Offline Virtual Machine Servicing Tool to provide automated patching of VM libraries. This provides integration with WSUS and SCCM. Real-time status reporting will be available as well. A beta of this is now available, and it will RTM after SCVMM 2008 goes RTM (which of course won't happen until Hyper-V goes RTM). The tool works by moving offline VMs to "maintenance hosts", where they are patched by WSUS/SCCM. Once the patching is complete, the VMs are powered back down and returned to the library.

Getting SCVMM 2008 is done by getting System Center Management Suite Enterprise. This includes VMM as well as Operations Manager, Data Protection Manager, and Configuration Manager.

Licensing is handled much like in previous versions of Windows. Standard Edition grants 1 additional VM instance; Enterprise Edition grants 4 additional VM instances; and Datacenter Edition grants unlimited VM instances. This is not a software limitation, this is a licensing limitation. Hyper-V does have an artificial limit of 256 VMs. Datacenter Edition is licensed per socket; Standard and Enterprise are licensed per server.

With regards to the virtualization roadmap, Hyper-V RC1 is available now and will RTM no later than August. SCVMM 2008 will RTM after Hyper-V goes final.

According to Edwin, management is the key. In his words, "Using virtualization with no management is worse than not using virtualization at all."

Other SCVMM-related content at Tech-Ed 2008 includes VIR360 (I'll be blogging that later this morning), VIR59-TLC, VIR350, VIR52-TLC, and VIR356. I'm not attending most of those sessions, but for those that I am I'll blog as much content as possible.

At this point, Edwin wrapped up the session. I'm heading clear across the convention center to attend Part 2 of the 2 SCVMM series.
