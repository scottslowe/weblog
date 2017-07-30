---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-11T14:38:56Z
slug: vir361-introducing-map-for-windows-server-2008-hyper-v
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: 'VIR361: Introducing MAP for Windows Server 2008 Hyper-V'
url: /2008/06/11/vir361-introducing-map-for-windows-server-2008-hyper-v/
wordpress_id: 735
---

Based on rapidly dwindling battery life, I thought that I might not be able to liveblog this session. Fortunately, I was able to find a power plug between the last session and this session, and I was able to find a seat near a power plug in this session. You're in luck!

This session is hosted by Jay Sauls and Baldwin Ng. Jay is a Senior Program Manager; Baldwin is a Senior Product Manager. This session will focus on the Microsoft Assessment and Planning (MAP) Solution Accelerator for Windows Server 2008, Hyper-V, and Virtual Server 2005 R2. The MAP Toolkit bears some similarity to Capacity Planner from VMware, as I understand it, so I'm very interested in learning more.

Both the MAP toolkit and the IPD Guides (discussed yesterday) fall into the Plan phase of the IT lifecycle as defined by Microsoft.

What are some challenges when considering a migration to Windows Server 2008 and virtualization? These include the accurate number of servers (some organizations, believe or not, actually have a problem having an accurate count of servers), compatibility (Does the hardware support Windows Server 2008? What about device compatibility? Does any of the hardware need BIOS or firmware upgrades?), and overall consolidation ratios. The MAP toolkit is designed to help address these questions.

The MAP toolkit consists of automated discovery tools and guidance. It provides an agent-less inventory of PCs, servers, applications, devices, and roles to provide migration readiness reports. Two ways in which the MAP toolkit might be used are 1) to assess hardware and device compatibility for Windows Server 2008; and 2) to assist with server placement in virtualization/consolidation scenarios.

MAP runs agent-less but does use WMI to gather information, so you will need to be sure that host-based firewalls (or network firewalls) allow the proper traffic to/from the systems, and you will need proper credentials on the host systems from which information is being gathered. After meeting these requirements, MAP can generate a report for different migration scenarios.

There are generally two types of documents that are produced by MAP. One is a migration proposal, which is a Word document that contains an executive summary and detailed data gathered from the environment. The second report is an Excel document that provides detailed information on every system discovered in the environment. This Excel document contains details on system components, configuration, installed devices, etc., and is intended to provide "deep detail" to back up the proposal document.

Next, Jay moves into a demo of MAP for three different scenarios. In the first scenario, we are using MAP to discover and document the existing environment. In the second, MAP will gather performance and utilization information to assist in recommendations for server consolidation/virtualization candidates. The third scenario involves some capacity planning, where we can specify a hypothetical host system and MAP will place virtualization candidates onto the hypothetical host in order to determine how many hosts will be needed.

MAP uses a local instance of SQL Server Express on the system on which it is installed. A separate database can be used for each instance that MAP is run.

For the first scenario, we need to discover the servers in an environment. There are a couple different ways to discover the servers, including enumerating Active Directory, IP address scanning, Windows networking (for servers in workgroups), or importing from a flat file. MAP should reconcile servers found in multiple methods, so a server found in AD as well as discovered via an IP address scan should be reconciled to the same server object.

Once the systems are discovered or imported, we need to specify credentials used to connect to the list of systems. These credentials need sufficient permissions to query WMI. It's not clear if this means administrative credentials.

MAP currently does not have any scheduling functionality. This is being considered for a future version.

MAP also gathers a list of all the device drivers installed on all the assessed systems, and attempts to identify if those device drivers are compatible with Windows Server 2008. Updates are published by Microsoft that are automatically downloaded by MAP with the latest device driver information.

The Word and Excel documents produced by MAP are rather extensive and detailed, and of course can be easily customized.

MAP can also gather performance statistics so as to be able to provide guidance in a server consolidation/virtualization scenarios. Much like with the discovery process, you can specify how you want to provide the systems that should be monitored and how long it should gather information. While you can schedule the stop date/time, you can't schedule the start date/time.

Once that data is gathered, you can use MAP to prepare a report that will make recommendations for how workloads will be consolidated onto either Virtual Server 2005 R2 or Hyper-V. During this wizard, you can select what types of CPUs, how many, how many cores per CPU, the L2/L3 cache amounts, and the bus speed. The wizard also allows you to specify the disk storage subsystem. This part of the wizard allows you to select from some predefined disk types, the number of disks, the RAID type (if you are using RAID), and the size of the disks. MAP uses this information to attempt to estimate the throughput of the disk subsystem. This method may or may not be applicable to SANs, since the predefined disk types and their corresponding information don't really map to most SAN types, especially Fibre Channel SANs and arrays using Fibre Channel drives.

The presenter answered a few questions regarding storage subsystem estimates by referring attendees to the System Center Capacity Planner application, which apparently could do a better job of estimating how a disk subsystem operates and how it responds to particular applications.

Continuing along with the wizard in MAP, we can specify the number and type of Ethernet adapters in the projected systems (the system upon which we will consolidate or virtualize candidates that have been identified earlier by MAP).

An artificial limit can be assigned to limit the number of VMs per host.

Finally, MAP will ask for a list of computers that should be considered virtualization candidates. I believe this list can be easily produced or exported from MAP based on earlier steps in the application.

As in other tasks with MAP, it produces a Word document and some matching Excel workbooks with supporting detailed information. In one of the Excel documents is a breakdown of which workloads are assigned to which virtualization hosts. This is all very useful information to have, and while the MAP toolkit does have a fair number of limitations, it is free. (Of course, VMware Capacity Planner is [now free](http://www.virtualization.info/2008/06/vmware-to-offer-capacity-planner-for.html) as well.)

The MAP toolkit can use the full-blown version of SQL Server, and it is possible to run multiple MAP instances against the same database so as to scale out to larger environments.

Looking forward at the future of MAP, version 3.1 will add new support for Hyper-V server placement (version 3.1 is in beta right now) and will also provide information on migrations to SQL Server 2008 from earlier versions of SQL Server. Some basic desktop security assessments will also be provided. MAP 4.0 may expand into disaster recovery and business continuity assessments as well as hardware assessments for next-generation Windows (Windows 7?) deployments.

That wraps up this session. My next session is MGT374, Offline Servicing of Virtual Machines.
