---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-16T16:59:29Z
slug: bc2621-fault-tolerant-vms-in-vi-operations-and-best-practices
tags:
- Virtualization
- VMotion
- VMware
- VMwareFT
- VMwareHA
- VMworld2008
title: 'BC2621: Fault-Tolerant VMs in VI: Operations and Best Practices'
url: /2008/09/16/bc2621-fault-tolerant-vms-in-vi-operations-and-best-practices/
wordpress_id: 907
---

Well, it looks like wireless coverage pretty much stinks, so this will have to be published post-session as well.

This session is BC2621, titled "Fault Tolerant VMs in VMware Infrastructure: Operations and Best Practices", presented by Dan Scales, Principal Engineer with VMware. It's a pretty full session, but I'm lucky (unlucky?) enough to get a seat toward the front of the session.

This is a technical preview of VMware FT, a new feature that is slated to be included in VDC-OS. This is the evolution of "Continuous Availability," which was demo'ed by Mendel Rosenblum in San Francisco at VMworld 2007. VMware FT is part of the availability application vServices, with also include such things as VMware HA, VMotion, Storage VMotion, NIC teaming, and storage multipathing.

VMware FT is one of two new application vServices that are being discussed this week; the other is vCenter Data Recovery, a full backup solution.

VMware FT is based on vLockstep technology that keeps a primary and secondary machine in virtual lockstep with no special hardware. Like the rest of VMware Infrastructure, VMware FT will run on standard, x86 commodity hardware, provides zero downtime and zero data loss. This is just another level of availability that can be provided. When compared with hardware-based availability, which does not run on commodity hardware, or standard clustering solutions, VMware FT allows for more protection with less complexity at a lower cost.

Again, VMware FT is simply a way to provide a higher level of protection than VMware HA for some VMs and workloads.

VMware FT involves two VMs: a primary VM and a secondary VM. The secondary VM is doing exactly the same thing as the primary, but does not communicate across the network. (What about storage?) In the event of a hardware failure, the secondary VM will become the primary VM and will assume network connections. Like VMotion, there should not be any interruption in network connectivity. After a host failure, a new host will be selected to run a new secondary VM, and there is a brief window in which the VM can't be fully protected with VMware FT.

VMware FT is fully integrated with VMotion and VMware DRS. Multiple FT pairs can run on a single host, and a host can run both FT-enabled and non-FT-enabled VMs on the same host. FT can be dynamically enabled or disabled dynamically, and VMware DRS is leveraged for the placement of the secondary VM.

VMware FT is based on VMware's Record/Replay technology, which was first introduced in VMware Workstation in 2006. When the data stream is recorded, only non-deterministic events are recorded, and the replay will occur deterministically. This creates instruction-for-instruction, memory-for-memory identical results.

Deterministic means that a processor will execute the same instruction stream and will end up in the exact same state. By recording non-deterministic events, VMware ensures that the record and the reply are identical. Non-deterministic stuff involves things like network/disk/keyboard I/O and hardware interrupts.

Using record/replay, VMware FT keeps two VMs in lockstep. These two VMs will share a common disk, although in the future this may move to a non-shared disk. Only the primary VM responds across the network; the secondary VM is a silent partner. If the primary VM fails, the secondary VM takes over immediately. In the event of a secondary VM failure, then redundancy will be re-established by restarting the secondary and re-syncing the two VMs.

When using VMware FT, this must be done in a VMware HA cluster. Once in a VMware HA cluster, a VM may be protected by FT, HA, or both. When both HA and FT are in operation, then both technologies come into play. In the event of hardware failure, HA will restart VMs and FT will make secondary VMs take over and become primary while HA restarts the former primary VM.

When a user enables VMware FT for a VM, a special kind of VMotion is used to create a secondary VM on a second host (basically a copy of the configuration). Then the two VMs are kept in virtual lockstep via VMware FT. If the primary VM is powered off, the secondary VM is powered off as well; the secondary VM will also be powered off if VMware FT is disabled.

Now, looking at the hardware and software requirements, VMware FT requires CPUs that support hardware virtualization (AMD-V, Intel VT). These features sometimes need to be enabled in the BIOS of the server. All hosts must be running the same build of VMware ESX, shared storage is required (NAS or SAN), and all hosts must be in an HA-enabled cluster. In addition, a separate FT logging NIC and a separate VMotion NIC are required. This means a minimum of 4 NICs are necessary (Service Console, VM traffic, VMotion, FT logging). Gigabit Ethernet is required for the FT logging NIC (just like the VMotion NIC). There's mention of "dedicated" or "separate" NICs; I wonder how firm that recommendation is? I'd be interested to know how this impacts best practices for NIC configurations.

VMware FT can't protect VMs that are using thin provisioned disks; disks must be "thick." Disks will be automatically made thick when VMware FT is enabled. (How does this impact vStorage Thin Provisioning?) VMs can't have any non-replayable devices (USB, sounds, physical CD-ROM, physical floppy, physical-mode RDMs) and paravirtualization-based VMs are not supported. Otherwise, all VMs are supported, both 32-bit and 64-bit guest operating systems.

Once all the prerequisites are met, VMware FT can be enabled by simply right-clicking on a VM and selecting "Turn Fault Tolerance On". After FT is fully ready to protect the VM, the icon color will change and the FT status will read "Protected". A new "Fault Tolerance" pane within VirtualCenter will show all the VMware FT statistics and information, like the location of the secondary VM, the amount of secondary VM CPU and memory usage, latency, and log bandwidth.

The Fault Tolerance status will have a number of different states:

* Enabled-Running (fully protected, this is the desired state)

* Enabled-Starting (FT is getting started)

* Enabled-Needs Secondary (a failure has occured, a new secondary needs to be created)

* Disabled (VMware FT is disabled)

Disabling FT is better than turning off FT. When FT is disabled, all the underlying infrastructure is maintained and makes it easier to re-enable FT at a later date or time. Turning off FT removes all the underlying infrastructure and setup.

The secondary VM shows up as "VM Name (secondary)" in VirtualCenter. It will not show up in the inventory, but it will show up in the list of VMs for a cluster or in the list of VMs for the secondary host. This may be confusing but is based on customer feedback. There will be only certain places where the secondary VM will appear.

The Maps tab will have a way to show the link between the primary VM and the secondary VM.

There will be a number of FT-related events and alarms; the "Enabled-Needs Secondary" state, for example, is one place where an alarm already exists.

When considering VM migrations, either the primary or the secondary can be moved via VMotion, but both cannot be moved at the same time. There is a built-in rule in DRS to keep them on separate hosts. The DRS mode for fault-tolerant VMs is set to "Manual"; this means the user must explicitly choose to initiate a VMotion. FT must be temporarily disabled to do a Storage VMotion, or you could power off the VMs and do a datastore migration at that time.

Again, no network connections are lost during a failover.

Dan covered again the interplay between VMware HA and the placement of the secondary VM, and the use of a modified VMotion to provision the secondary VM. That is a very quick process, meaning that the FT status will return to "Enabled-Running" very quickly.

In the event of a multiple host failure, VMware HA will restart the primary and VMware FT will recreate the secondary VM to establish redundancy. In the event of a guest OS software failure, VMware FT won't do anything because the primary and secondary are in sync; both will fail at the same time and in the same place. VMware HA failure monitoring can restart the primary; the secondary will then be recreated via special VMotion to re-establish redundancy.

What applications are suitable for VMware FT?

* Applications that run well on uniprocessor VMs

* Applications that can tolerate a small increase in network latency

* Applications that have medium network bandwidth requirements (less than 600Mbps)

Examples of this would include medium-sized database applications, messaging applications, or important custom applications.

It's important to keep in mind that the bandwidth of the FT logging NIC may become a bottleneck; watch how many FT pairs on placed on a single host or move to 10Gbps if available. You may also want to reconfigure some guest OSes (Linux with 1000Hz timer interrupts) to use a slower interrupt timer.

The session wrapped up with a customer portion by Mark Vaughn of The First American Corporation. Due to other schedule requirements, I didn't stay for that portion of the session. Next up is some time in the Solutions Exchange. Stay tuned for more updates as soon as I can get network coverage.
