---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-11T18:19:25Z
slug: mgt374-offline-virtual-machine-servicing-tool
tags:
- HyperV
- Microsoft
- Security
- TechEd2008
- Virtualization
- Windows
title: 'MGT374: Offline Virtual Machine Servicing Tool'
url: /2008/06/11/mgt374-offline-virtual-machine-servicing-tool/
wordpress_id: 736
---

This session couldn't be published live because I had no wireless signal and no cellular signal in the breakout room. However, I did want to capture the information and publish it at the next available opportunity for the benefit of the readers.

This session was hosted by Luis Camara Manoel, Satish Mathew, and Jay Sauls (he was also one of the presenters in the session prior to this one). The focus of the session, quite obviously, is the Offline Virtual Machine Servicing Tool, which is designed to help in the maintenance and patching of offline VMs. Offline VMs are typically cited as one of the major security concerns with virtualization projects, in that they likely will not as up-to-date with patches and malware protection as online VMs; thus, when they finally do come online they could present a security risk to the organization.

The session starts off with an overview of the various Solutions Accelerators that are available from Microsoft, and then Jay Sauls takes over and begins to talk about the MAP toolkit again. Of course, I've just finished an extensive session on the MAP toolkit, so this is completely redundant and absolutely useless for me. I tuned him out until the session changed focus again to the Offline VM Servicing Tool.

When the session switches focus back to the Servicing Tool, the question is asked: Why are offline VMs such a problem? Many attendees in the session indicate that they have sizable numbers of offline VMs sitting in a library. The typical problem, as I mentioned earlier, is that the offline VMs miss patches, miss compliance scans, and miss other updates.

The solution to this problem is the Offline Virtual Machine Servicing Tool. This tool is designed to automate the application of OS patches as well as application patches. This is accomplished by integrating with existing System Center products like System Center Virtual Machine Manager (SCVMM) and System Center Configuration Manager (SCCM) _or_ Windows Server Update Services (WSUS). I appreciate the fact that Configuration Manager is not required; otherwise, this tool would be far less useful.

Note that "true offline" patching will be available in the next version of Configuration Manager, but it will only service VMs running Windows Vista and Windows Server 2008.

The Offline VM Servicing Tool takes four steps in its operation:

1. Identify

2. Assess

3. Patch

4. Report

The overall process of how the Offline VM Servicing Tool works looks something like this:

1. The tool reads the SCVMM library and gets a list of VMs

2. A VM group is created

3. The user must select a group of maintenance hosts; these maintenance hosts will be where the offline VMs will be moved to be patched

4. It will schedule a job on these maintenance hosts

5. The VMs will be moved from the library to the maintenance hosts and started

6. The VMs will be patched using Configuration Manager or WSUS (see below)

7. Upon confirmation of the patching of the VMs, they will be shut down and moved back to the SCVMM library

The tool works by utilizing PowerShell to automate a series of tasks like starting the VM, moving the VM, applying patches, etc. The UI screens for the tool were developed to match the SCVMM UI screens. Windows Server 2003, Windows XP, and Windows Vista are currently supported; Windows Server 2008 is not yet supported.

The requirements for using the tool:

* All VMs must be under SCVMM control

* It's strongly recommended to setup a separate VLAN for the maintenance hosts

* If using Configuration Manager, all VMs must have the Configuration Manager client

* If using WSUS, all clients must be configured to use WSUS

* The server running the Offline VM Servicing Tool must be dual homed to talk to both SCVMM and Configuration Manager/WSUS

At this point Satish, one of the presenters, took over with a demo of the tool. As mentioned earlier, the tool looks and acts a lot like SCVMM.

The presentation consistently referenced SCVMM 2007, the currently shipping version; support for SCVMM 2008 will be included in the next version of the tool. Also slated to inclusion in the next version is support for Windows Server 2008, Hyper-V, Configuration Manager 2007 SP1, and WSUS 3.0 SP1. Unfortunately, this next version isn't due until 2009, leaving quite a sizable gap in time between the availability of Windows Server 2008 and Hyper-V and the ability of the tool to work with those products. It seems to me that the Offline VM Servicing Tool, while useful right now, will become much less relevant and much less useful once Hyper-V and SCVMM 2008 go RTM.

At this point, Stephen Anderson with Compellent took the stage and began to discuss his company's products. I'm not really clear why Compellent was given time to advertise their products, unless it was by virtue of the fact that Compellent provided Microsoft with some tools and equipment to assist in the development of the Offline VM Servicing Tool. In any event, I found this to be completely inappropriate and left the session.
