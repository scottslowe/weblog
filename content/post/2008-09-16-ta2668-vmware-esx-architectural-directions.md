---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-16T17:00:10Z
slug: ta2668-vmware-esx-architectural-directions
tags:
- ESX
- ESXi
- Virtualization
- VMFS
- VMware
- VMworld2008
title: 'TA2668: VMware ESX Architectural Directions'
url: /2008/09/16/ta2668-vmware-esx-architectural-directions/
wordpress_id: 908
---

Once again, there's no wireless signal in the breakout session, so I'll have to publish this in a delayed fashion. Ugh! Someone needs to work on this wireless network.

This is session TA2668, VMware ESX Architectural Directions. This is another session that provides forward-looking information at future versions of VMware ESX.

The presenter started out the presentation by giving an overview of VMware ESX, what it's designed to do, what features it has currently, what it's capable of doing, what it knows, how it manages CPU scheduling and other resources like memory. This would include stuff like being aware of the difference between sockets, cores, and hyperthreads; and stuff like VMware ESX's advanced memory management functionality (like transparent, content-based page sharing). He also reviewed the management of I/O such as network and storage I/O.

Moving into "future features," these features will focus on security, scalability, interoperability, and performance.

In the security area, VMware is focusing on improving security in a number of areas. One of these is reducing the size of the platform, such as in ESXi; it is anticipated that this will reduce the surface area and lower the number of potential vulnerabilities. VMware is also looking at the use of ASLR (Address Space Layout Randomization) and NX support (No eXecute). Together these features will protect both applications and the kernel and make exploits much more difficult and more difficult to automate and repeat.

In addition, VMware is working on the VMsafe APIs, previously announced at VMworld Europe 2008 in February. These APIs will allow VM introspection and intervention. Security modules will run outside the guest and are strongly isolated from the guest(s) they are monitoring. However, these modules will still have full access, full visibility, and full introspection and control functionality.

On the scalability front, VMkernel will move to 64-bit. Although in practice this will be unnoticeable, it will simplify VMkernel on large memory machines and on systems with larger numbers of CPUs. This will not impact support for 32-bit and 64-bit guests running on VMware ESX. This change will enable support for up to 64 logical processors, up to 512GB of RAM, up to 512 vCPUs per host, and higher VM limits (8 vCPUs and 256GB of RAM per VM). These higher limits will also help improve support for guests with 2 and 4 vCPUs.

VMware ESX will also leverage dynamic frequency and voltage scaling, which will enable the VMkernel to manage power states for CPUs when load decreases. This will be accomplished via Intel Enhanced SpeedStep and AMD PowerNow.

Power will also be treated as a first-class resource in the VI Client, with tracking and reporting of power utilization.

Another scalability improvement is the Distributed vSwitch (DVS). The DVS is a vSwitch that spans an entire VMware ESX cluster. This brings greatly simplified network configuration across the entire cluster, stateful vSwitches (vSwitches can maintain per-port policies), and plugins. Examples of plugins would include appliance APIs (to create inline filters for per-VM traffic) or switch APIs (to modify the forwarding algorithm).

vStorage Thin Provisioning is another scalability improvement. However, as mentioned in my earlier post on BC2621, this may conflict with VMware FT. This is intended to reduce disk space utilization, improve disk-related operations like backup, cloning, etc. Obviously, new alerts need to be created to manage disk allocation and overprovisioning.

Moving ahead to interoperability, the presenter first discussed Enhanced VMotion Compatibility (EVC). The idea behind EVC is masking certain CPU features so that guests can migrate live via VMotion between hosts with dissimilar CPUs. EVC leverages functionality built into modern processors from AMD and Intel CPUs to hide CPU features so that CPUs appear to be identical to guests. EVC is available today in VMware ESX 3.5 Update 2. EVC problems are detected when a host is added to a cluster, to prevent problems before a user attempts a VMotion between hosts with incompatible CPUs.

The Service Console will be updated to a 64-bit distribution running on version 2.6 of the Linux kernel. All hardware device drivers will be in VMkernel; none of them will be in the COS. In fact, the COS filesystem will reside in a VMDK on a VMFS and uses the same storage path(s) as VMkernel. Of course, there is no Service Console for ESXi. Both ESX and ESXi support CIM-based host management.

Another area of interoperability is with storage plugins, using the VMware Pluggable Storage Architecture (PSA). This will enable partners to write plugins to enhance storage functionality. VMware ESX will ship with a plugin known as NMP (Native Multipathing Plugin), which in turn is comprised of SATP (Storage Array Type Plugin) and PSP (Path Selection Plugin). ALUA support will be added as well.

VMDirectPath I/O is another interoperability advancement. This allows the guest to directly control physical hardware. This seems to fly directly in the face of virtualization, which is intended to virtualize and abstract physical hardware away from the virtual machines. However, this could be useful in some instances. Hardware I/O memory management is required in order to isolation guest memory access and translate guest addresses to host addresses. In addition, PCI device reset capability is required.

VMDirectPath I/O does introduce its own set of challenges, such as the impact on VM suspend, checkpoint, and migration. VM memory management is also impacted. It is anticipated that the "Gen1" release of this functionality will accept these limitations and "Gen2" will begin to address them.

VMware will also support newer device drivers from partners and will also allow asynchronous device driver updates. A device driver development kit will be available to allow 3rd party developers to add device drivers to VMware ESX. Of course, with the switch to a 64-bit architecture, the drivers will also switch to 64-bit drivers.

Finally, in the area of performance, VMware will improve CPU efficiency for I/O virtualization. This covers an enormous amount of work for reducing overhead, enabling large packet send/receive, scheduling processor interrupts, and the paravirtualized SCSI device to improve SCSI performance. Future versions of ESX are also expected to include large page support. Other areas of improvement include expanded support for hardware virtualization, and VMware will focus on specific application areas to help drive performance and optimization of those applications on VMware ESX. This is in addition to helping customers optimize VMware ESX itself.

At this point, the presenter concluded the session.
