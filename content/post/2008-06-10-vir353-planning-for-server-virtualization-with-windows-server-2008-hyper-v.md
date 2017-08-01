---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-10T16:31:25Z
slug: vir353-planning-for-server-virtualization-with-windows-server-2008-hyper-v
tags:
- HyperV
- Microsoft
- TechEd2008
- Virtualization
- Windows
title: 'VIR353: Planning for Server Virtualization with Windows Server 2008 Hyper-V'
url: /2008/06/10/vir353-planning-for-server-virtualization-with-windows-server-2008-hyper-v/
wordpress_id: 729
---

My final session of the day is VIR353, Planning for Server Virtualization with Windows Server 2008 Hyper-V Using the New Infrastructure Planning and Design Guides. I got into this session about 15 minutes late as I was finishing up a one-on-one interview with Jeff Woolsey, Senior Program Manager for Hyper-V. I'll have more on my interview with him later today or tonight. I'm not attending the next session of the day because it turns out that VIR366/VIR367 are essentially identical.

As I entered the session, the speaker was discussing the steps or tasks involved in creating the list of applications that will be run virtualized. He underscored the need to understand the applications' requirements with regards to CPU, CPU type (32-bit or 64-bit), RAM requirements, and networking and storage requirements. The example given by the speaker is trying to run Exchange Server 2007 on Virtual Server. This won't work, of course, because Exchange requires 64-bit support and Virtual Server only provides 32-bit support. Users also need to quantify support for applications running on a Hyper-V installation. Upon compiling the list of applications and their requirements, users will then need to identity the candidates for virtualization and prioritize them for virtualization.

The guide (I'm assuming this means the Infrastructure Planning and Design guide) makes the assumption that a single application on a physical server is transferred to a single application on a single VM. The guide does not take into account combining applications during the consolidation process.

After identifying the applications, users should then determine resource requirements. This includes CPU load, memory load, disk I/Os per second (IOPS), disk capacity, and network bandwidth. Users will also need to consider backup requirements and high availability requirements. Using the information on resource requirements, users can then group applications together and ensure that the aggregate requirements don't exceed the capabilities of the physical host. This obviously can't be done without first identifying the host platform, either by using company standards or by custom designing a virtualization hardware platform that suits your needs.

The speaker stresses the importance of mapping the five key resource requirements---CPU load, memory load, disk I/O per second, disk capacity, and network bandwidth---against the host hardware. This can be done manually or using the Microsoft Assessment and Planning (MAP) Toolkit. I'm attending a session on MAP tomorrow, so I'll have more information on that later. It's my understanding that MAP is similar to VMware Capacity Planner. The speaker also mentions that we can use System Center Virtual Machine Manager (SCVMM) to help with this process as well.

There are some considerations to keep in mind if using the tools. MAP currently does not include memory load or disk capacity in its assessments; SCVMM does not include disk capacity. The manual method will also typically not take CPU frequency scaling into consideration (i.e., 15% busy on a 2.2GHz CPU does not mean 15% busy on a 3.0GHz CPU).

The speaker announced that the IPD Guide for SCVMM is now available in beta.

SCVMM 2008, the next version of SCVMM that is currently in beta, is considered essential for managing a Hyper-V installation of any size. In this regard, it is very similar to VirtualCenter. Most companies installing any reasonable size implementation of VMware Infrastructure are also going to install VirtualCenter; the same goes for Hyper-V and SCVMM 2008.

A design consideration for deploying SCVMM is the placement of the VMM library. The VMM library is where ISOs, VM templates, Sysprep files, etc., are stored. Anytime SCVMM instantiates a VM, files must be copied from the VMM library to the host server, so good network connectivity is absolutely critical. In fact, the speaker said, "right next to each other" if at all possible.

In today's [keynote liveblog][1], you'll recall that I mentioned PRO, which is an integration point between SCVMM and SCOM. If this is something that an organization desires, this is a decision to make early in the process. This decision can affect the number of SCVMM instances; this is due to the fact that multiple SCVMMs can connect to a single SCOM management group (MG), but a single SCVMM instance can't connect to multiple SCOM MGs. If there are multiple SCOM MGs, then you'll need to install multiple SCVMM instances (one for each MG), or you'll need to forgo integration of the consoles.

The session wrapped up with an overview of what's coming next in the Infrastructure Planning and Design guides. In particular, the speaker provided a list of the various IPD guides that are available from Microsoft. Some of them include:

* Selecting the Right Virtualization Technology
* Windows Server Virtualization
* SoftGrid 4.2
* Terminal Services in Windows Server 2008
* Windows Deployment Services

There are about 10 IPD guides available, with more in the works.

And that's it for today's sessions! I have a few other posts to prepare; I'll try to get them published tonight but it may be tomorrow. Hopefully readers are finding the coverage of the conference helpful. Feel free to add your thoughts in the comments below, and thanks for reading!

[1]: {{< relref "2008-06-10-tech-ed-2008-keynote-liveblog.md" >}}
