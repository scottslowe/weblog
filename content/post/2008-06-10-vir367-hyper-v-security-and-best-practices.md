---
author: slowe
categories: Liveblog
comments: true
date: 2008-06-10T11:21:38Z
slug: vir367-hyper-v-security-and-best-practices
tags:
- HyperV
- Microsoft
- Security
- TechEd2008
- Virtualization
- Windows
title: 'VIR367: Hyper-V Security and Best Practices'
url: /2008/06/10/vir367-hyper-v-security-and-best-practices/
wordpress_id: 727
---

My first breakout session of Tech-Ed 2008 is a session led by Jeff Woolsey, Senior Program Manager for Hyper-V, and the session is about Hyper-V security and best practices. I've met Jeff before at VMworld 2007, although he probably doesn't remember me. I'll have more information from Jeff this afternoon, as I have a meeting scheduled with him. I'll republish what I can from that conversation here this afternoon or this evening. I'll do my best to liveblog this session, but the wireless signal is really weak in here, and I suspect that as soon as they close the doors to the room the wireless signal will drop off completely. We'll see.

The Wi-Fi connection has pretty much dropped off the map, and my cellular signal rapidly deteriorated from 3G to GPRS. Ouch. No liveblogging of this session.

Jeff starts out the session with a list of virtualization requirements, which are basic requirements for any virtualization solution: scheduler, memory management, VM state machine, virtualized devices, storage stack, network stack, ring compression (optional), drivers, and a management API. Jeff speaks in a bit more detail about synthetic devices ("ripping fast"), which I understand to be the same as the PV drivers that other solutions use.

We next see a brief overview of Virtual Server's architecture, and Jeff went into a bit of detail as to why ring compression (which he described as "software gymnastics") was no longer necessary with Hyper-V and hardware assists from Intel and AMD. In addition, Jeff discussed in a little more detail about the I/O problems with Virtual Server.

In the new architecture for Hyper-V, Ring 1 (the "software gymnastics" or ring compression referenced earlier) is gone. This is made possible by hardware assists. Instead of all VMs threading in a single process, each VM with Hyper-V gets its own VM worker process in the parent partition. I/O is improved by the use of synthetic devices. Synthetic devices talk to VMBus, which is a high-speed interconnect between partitions, which then use Virtualization Service Providers (VSPs). The VSPs talk to device drivers in the parent partition and then to the underlying hardware.

With regards to security, Microsoft has tried to model every possible avenue of attack from the VM to the parent partition. With regards to VMBus, it is a point-to-point interconnect connecting a child partition to the parent partition. OK, this prevents inter-VM communication, but how does it secure child-to-parent communication? The VM worker processes that run in the parent partition run in user mode (Ring 3), run in separate processes, and run with stripped-down privileges. This again is done in an attempt to protect the parent partition and the hypervisor from attack.

The "parent partition" is a privileged partition. It's the only partition that is allowed to see all the physical hardware. Jeff discusses the need for the parent partition. In reviewing the components required for virtualization, dropping the parent partition means that all these components (see above) must be integrated into the hypervisor itself. According to Jeff, this is analogous to plugging your core network into the Internet with no firewall. No "defense in depth." Instead, using a parent partition allows the hypervisor to be as thin as possible and provides "defense in depth" by running stuff like the management API and the VM state machine (the VM worker processes) in user mode.

What security assumptions were made when designing Hyper-V?

* All guests are untrusted

* The parent is trusted by the hypervisor, and the parent is trusted by all the children

* Code in guests can run in all modes, rings, and segments

* The hypercall interface will be widely documented and available, and will be available to attackers

* All hypercalls can be attempted by guests

* Guests can detect they are running on a hypervisor

* The design of Hyper-V will be well understood

What were the security goals for Hyper-V?

* Strong isolation between partitions

* Guest confidentiality and integrity

* Separation using unique hypervisor resource pools, separate worker processes, unique child-to-parent communications channels (VMBus)

* Non-interference to prevent any guest from affecting the contents or computations of other guests or the parent, and there is no guest-to-guest VM interface communications

Coming back to the idea of isolation again, Jeff reinforces the steps taken to ensure VM isolation. Separate virtual devices, separate VMBus per VM to the parent, no memory sharing, no guest-to-guest VM communications except by virtual networks, no guest DMA attacks because physical devices are not available, and neither the guests nor the parent are capable of writing to the hypervisor.

Hyper-V went through the Microsoft Secure Development Lifecycle (SDL), like other products, to get features like Address Space Layout Randomization (ASLR), support for NX/XD, etc. As a side note, NX/XD _must_ be enabled in order for Hyper-V to even work. If NX/XD is disabled, Hyper-V won't start or run.

Hyper-V's security model uses Authorization Manager to provide fine-grained authorization and access control. This allows users to define specific roles and functions. For example, VM administrators don't have to be administrators of the parent partition. This functionality appears to bring delegated access with Hyper-V on par with VirtualCenter with regards to role-based access. Again, it's not an apples-to-apples comparison, since you also need SCVMM to do some things that VirtualCenter does, but you get the idea. At least with the Hyper-V MMC, we appear to get delegated access on the host without the separate management server; if this is the case, this is an advantage for Microsoft.

Server Core is recommended for use with Hyper-V. Server Core is the CLI-only version of Windows Server 2008; the idea is that by removing stuff like Internet Explorer, the shell, etc., we can reduce the attack surface and reduce the need to patch the host. Enabling Hyper-V with Server Core is a bit complicated. The key thing is `ocsetup Microsoft-Hyper-V`; this is the command line that will install Hyper-V on Server Core. Once Hyper-V is installed and enabled, you can use MMC to manage it remotely. (Note that the MMC doesn't include any support for P2V, V2V, PowerShell, etc. It's very basic.)

Jeff next moves into a discussion of his "dream virtualization environment". He describes a "pie in the sky" and "money is no object" environment. I don't know that I actually find this particular exercise useful, since most organizations have a limited budget and can't design the "dream environment." They have to design the "real environment." Still, it's interesting to see Jeff's suggestions on the ideal virtualization farm.

(Note to VMware: By the way, Jeff indicates that you recommend turning off memory page sharing and idle page reclamation in production environments. I thought you might want to know that.)

Jeff continues his description of the "dream environment" by building out the extra components:

* System Center Configuration Manager to provide bare metal provisioning for the hosts themselves, patching (host or guest), and compliance

* System Center Virtual Machine Manager to provide VM provisioning and VM management

* System Center Operations Manager to provide workload management and monitoring

* System Center Data Protection Manager to provide VM backups and VSS integration and provide WAN-based replication

My question is this: how many additional servers are required to provide all this extra functionality? And how much does all this extra software cost? And how complicated is it to get all these components running together as intended? This is all good stuff, yes, but you can't compare a fully fleshed out System Center infrastructure and Hyper-V with a traditional VI3 implementation. (More on that later.)

Jeff provides some tips and tricks for Hyper-V:

* Minimize the risk to the parent partition. This means run Server Core, and don't run anything in the parent that doesn't _absolutely_ have to be there.

* When moving VMs from Virtual Server to Hyper-V, uninstall the VM Additions first.

* Use _at least_ two physical adapters in the host. One must be dedicated to host management. When using iSCSI, use dedicated NICs for iSCSI.

* Connect the host to "back-end" management networks, but connect the guests to "front-end" production networks.

With regards to clustering, the setup of clusters in Windows Server 2008 has been greatly simplified. This helps provide high availability for virtual machines via the use of Failover Clustering with Hyper-V. I won't go into any more detail on that as it's been discussed extensively elsewhere in the Quick Migration vs. Live Migration threads.

The Hyper-V Integration Components are the equivalent of the PV drivers that XenServer and VMware also use; the idea is to provide vastly superior performance over emulated hardware devices.

Hyper-V is or will be localized in all server languages.

If you are going to run antivirus (AV) software on the parent partition, be sure to exclude the .VHD files. Be sure to run AV within the guest VMs; this will help ensure that unpatched offline VMs won't inadvertently spread malware. On one slide, Jeff indicates that BitLocker is not yet fully supported with Hyper-V; Microsoft is still performing testing; on the next slide, he says that you can run BitLocker together with Hyper-V. I'm not sure which stance is correct. Can anyone clarify?

Next, Jeff moved into a discussion of various items. Mitigating bottlenecks is pretty straightforward and simple; same rules apply here as with other technologies. VHD Expansion/Compaction, especially compaction, is not recommended on a production system. Use ISOs wherever possible. Use the SCVMM library (think VirtualCenter templates) to simplify VM provisioning. Jeff recommends creating 2-way VMs so that the MP HAL is used; the MP HAL will work with single CPU VMs as well as multi-CPU VMs, but not vice versa.

At this point, Jeff concluded the session with a brief demo of Windows Server 2008 and Hyper-V.
