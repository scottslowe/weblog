---
author: slowe
categories: Liveblog
comments: true
date: 2008-09-18T16:48:57Z
slug: po1644-vmware-update-manager-performance-and-best-practices
tags:
- ESX
- Security
- Virtualization
- VMware
- VMworld2008
title: 'PO1644: VMware Update Manager Performance and Best Practices'
url: /2008/09/18/po1644-vmware-update-manager-performance-and-best-practices/
wordpress_id: 946
---

This is PO1644, VMware Update Manager Performance and Best Practices. The presenter is John Liang.

Covering some terminology before moving forward, the presenter covered the idea of a patch store (a location where patches are stored), baseline, compliance (the state of the host or VM when compared to the baseline; falls into compliant/not compliant/unknown/not applicable), scan (either VM scan or host scan; VM scans can be either online or offline); and remediation (the idea of applying patches to a host or VM).

VUM has two deployment models. In the Internet-connected model, the VUM server knows and has connectivity to the VMware patch repository, and VUM will work closely with VirtualCenter. VUM can also be connected to multiple VC servers on multiple subnets.

In an Internet-disconnected model, VUM has no direct connectivity to the Internet and is not able to download patches for deployment. In this model, a separate Update Manager Deployment Server (UMDS) instance can download the patches. The patches can be exported to physical media and transferred to the VUM server for use in scanning and remediation.

Next, the presenter moved into a discussion of VUM sizing. VUM uses a separate database. A small deployment (20 hosts, 200 VMs, 4 scans/month for VMs, 2 scans/month for hosts) will generate 17MB/month in database storage. A medium deployment ups to 109MB/month, and a large deployment would generate 552MB/month in database storage.

The presenter provided some guidelines for patch store disk space, but I couldn't capture that information before he proceeded to the next slide.

There are a number of VUM deployment models. VUM can be deployed on the same server as VC and use the same database server as VC. However, for medium deployments, consider separating the VUM database to a different database server. Consider a medium deployment to be 500 VMs or 50 hosts. For even larger deployments, both VC and VUM should use separate servers and separate databases. The recommendation is to separate VUM from VC is there are more than 1000 VMs or 100 hosts. In addition, the VUM database should be on different disks than the VC database, use at least 2GB of RAM for caching (more is better), and finally to separate VC and VUM onto separate servers for maximal performance.

Next, the presenter discussed some performance results for VUM. The host running VC and VUM and the database was a dual socket/dual core host with 16GB of RAM, managing VMware ESX hosts with 32GB of RAM. The results that were presented:

* 8 seconds to download the VM guest agent

* 27 seconds to scan a powered-off Windows VM

* 36 seconds to scan a powered-on Windows VM

* 8 seconds to scan a Linux VM

Next, the presenter showed some results of resource consumption during these various tasks. Compared to other operations, scanning a powered-off Windows VM took the most CPU usage on the VUM server itself. For the same task, the VC server CPU was not tremendously impacted, VMware ESX CPU was not impacted, and disk and database performance was essentially equivalent across all operations. Again, these results are all for single-operation scenarios. Out of all the tasks, the most (relative) expensive operation was an offline Windows VM scan.

VUM is limited to 5 VM remediation tasks per VMware ESX host (48 per VUM server), 6 powered-on Windows scans per VMware ESX host (42 per VUM server) , 10 powered-off Windows scans per VMware ESX host (10 per VUM server), 6 powered-on Linux scans per VMware ESX host (72 per VUM server), 1 VMware ESX scan per VMware ESX host (72 per VUM server), and 1 VMware remediation task per VMware ESX host (48 per VUM server). Some of these limits make sense; you can't scan more than one VMware ESX host per VMware ESX host, for example.

The presenter gave a quick of wanting to scan 5000 Windows VM across 100 hosts, each host with 50 VMs and each scan taking 60 seconds? The answer: just shy of 105 minutes. I won't go into the math details.

Entering maintenance mode can be blocked for the following reasons:

* VMware HA is configured with only two hosts

* VMware DRS fails to VMotion a VM to another host

* VMware DRS is configured for manual mode instead of automatic mode

* Without VMware DRS, there is a VM powered on

To correct these problems, use VMware DRS and configure it to use automatic mode, and use more than 2 hosts in a cluster.

It's important to remember that the guest agent is single-threaded, and the Shavlik scan and remediation are also single-threaded. Using multiple vCPUs won't necessary help with guest OS performance with regards to patching.

What is the impact of patching on guest memory? VUM will create virtual CD-ROM images, attach them to the guest, and then issue the remediation command to the VM. This will trigger a fairly significant amount of network traffic between the VUM server and the VMs being remediated. This can have a significant impact on network performance. The remediation process itself is also memory intensive, which can be further exacerbated by larger patches (Windows XP SP3 is 331MB, for example). To help with performance, VMware recommends at least 1GB of RAM for Windows VMs.

Next, the presenter tackles the subject of the impact of high-latency networks on VUM operation. The time taken by various operations (online scans, offline scans, remediation, etc.) is directly related to network latency; the higher the latency, the longer the operation takes. Online VM scans are the only exception; they remain constant and very low.

To help address this potential problem, VMware recommends deploying the VUM server as close to the VMware ESX hosts as possible. This will help reduce network latency and packet drops. In addition, use online scans on high-latency networks to minimize the impact of network latency.

An offline scan works by having the VUM server mount the VMDKs for the offline Windows VMs and then scan them directly from the VUM server. This explains why the CPU utilization on the VUM server is so directly impacted as a result of performing offline Windows VM scans; it has to mount the VMDK and scan the Registry and disks locally on the VUM server.

To help optimize offline scan, exclude "\Device\vstor*" in any on-access anti-virus software on the VUM server itself. This will prevent the VUM server from performing more I/O operations than necessary. Making this optimization helps improve performance by reducing latency by almost 50% on a high-latency network. The impact is almost negligible on a low-latency network. The presenter walks through excluding the appropriate device/location in anti-virus, something with which most users in here are probably already familiar.

* Use VMware DRS in automatic mode for host patching.

* Separate physical disks for patch store and VUM database.

* Use at least 2GB RAM for VUM server host to cache patch files in memory.

* Separate the VUM server database from the VC database if the inventory is large enough.

* Using multiple vCPUs in guests won't necessarily help performance.

* Deploy VUM close to ESX hosts where possible.

* Prefer online scan on high-latency networks.

* Configure on-access anti-virus appropriately on the VUM server.

At this point, the presenter closes the session with a summary of VUM and its role in a VMware Infrastructure deployment.
