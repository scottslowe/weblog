---
author: slowe
categories: Liveblog
comments: true
date: 2012-09-12T19:38:11Z
slug: clds006-exploring-new-xeon-e5-optimizations-for-10-gb-ethernet
tags:
- FCoE
- IDF2012
- Intel
- Networking
- Virtualization
title: 'CLDS006: Exploring New Xeon E5 Optimizations for 10 Gb Ethernet'
url: /2012/09/12/clds006-exploring-new-xeon-e5-optimizations-for-10-gb-ethernet/
wordpress_id: 2858
---

This is session CLDS006, "Exploring New Intel Xeon Processor E5 Based Platform Optimizations for 10 Gb Ethernet Network Infrastructures." That's a long title! The speakers are Brian Johnson from Intel and Alex Rodriguez with Expedient.

The session starts with Rodriguez giving a (thankfully) brief overview of Expedient and then getting into the evolution of networking with 10 Gigabit Ethernet (GbE). Rodriguez provides the usual "massive growth" numbers that necessitated Expedient's relatively recent migration to 10 GbE in their data center. As a provider, Expedient has to balance five core resources: compute, storage (capacity), storage (performance), network I/O, and memory. Expedient found that migrating to 10 GbE actually "unlocked" additional performance headroom in the other resources, which wasn't expected. Using 10 GbE also matched upgrades in the other resource areas (more CPU power, more RAM through more slots and higher DIMM densities, larger capacity drives, and SSDs).

Rodriguez turns the session over to Brian Johnson, who will focus on some of the specific technologies Intel provides for 10 GbE environments. After briefly discussing various form factors for 10 GbE connectivity, Johnson moves into a discussion of some of the I/O differences between Intel's 5500/5600 processors and the E5 processors. The E5 processors integrate PCI Express root ports, providing upwards of 200 Gbps of throughput. This is compared to the use of the "Southbridge" with the 5500/5600 series CPUs, which were limited to about 50 Gbps.

Integrated I/O in the E5 CPUs has also allowed Intel to introduce something like Intel Data Direct I/O (DDIO). DDIO allows PCIe devices to DMA information directly to cache---instead of main memory---where it can then be fetched by a processor core. This results in reduced memory transactions and, as a result, greater performance. The end result is that the E5 CPUs can support more throughput on more ports than previous generation CPUs (up to 150 Gbps across 16 10 GbE ports with an E5-2600 CPU).

Johnson also points out that the use of AES-NI helps with the performance of encryption, and turns the session back over to Rodriguez. Rodriguez shares some details on Expedient's experience with Intel AES-NI, 10 GbE, and DDIO. In some tests that Expedient performed, throughput increased from 5.3 Gbps at ~91% CPU utilization with a Xeon 5500 (no AES-NI) to 33.3 Gpbs at ~79% CPU utilization on an E5-2600 with AES-NI support. (These tests were 256-bit SSL tests with Open SSL.)

Rodriguez shares some of the reasons why Expedient felt 10 GbE was the right choice for their data center. Using 1 GbE would have required too many ports, too many cables, and too many switches; 10 GbE offered Expedient a 23% reduction in cables and ports, a 14% reduction in infrastructure costs, and offered a significant bandwidth improvement (compared to the previous 1 GbE architecture).

Next the presentation shifts focus a little bit to discuss FCoE. Rodriguez goes over the reasons that Expedient is evaluating FCoE for their data center. Expedient is looking to build the first Cat6a-based 10GBase-T FCoE environment leveraging FC-BB-6 and VN2VN standards.

Johnson takes back over again to discuss some of the specific technical items behind Expedient's FCoE initiative. Johnson shows a great diagram that reviews all the various types of VM-to-VM communications that can exist in modern data centers:

* VM-to-VM (on the same host) via the software-based virtual switch (could be speeds of 30 to 40 Gbps in this use case)

* VM-to-VM (on the same host) via a hardware-based virtual switch in an SR-IOV network interface card (NIC)

* VM-to-VM (on a different host) over a traditional external switch

One scenario that Johnson didn't cover was VM-to-VM traffic (on different hosts) over a fabric extender (interface virtualizer) environment, such as a Cisco Nexus 2000 connected up to a Nexus 5000 (there are some considerations there; I'll try to discuss those in a separate post).

Intel VT-c actually provides a couple of different ways to work in virtualized environments. VMDq can provide a hardware assist when the hypervisor softswitch is involved, or you can use hypervisor bypass and SR-IOV to attach VMs directly to VFs (virtual functions). Johnson shows that the E5 processor provides higher throughput at lower CPU usage with VMDq compared to a Xeon 5500 CPU (tests were done using an Intel X520 with VMware ESXi 5.0). Using SR-IOV---support for which is included in vSphere 5.1 as well as Microsoft Windows Server 2012 and Hyper-V---allows VMware customers to use DirectPath I/O to assign VMs directly to a VF, bypassing the hypervisor. (Note that there are trade-offs as a result.) In this case, the switching is done in hardware in the SR-IOV NIC. The use of SR-IOV shows dramatic improvements in throughput with small packet sizes as well as significant reductions in CPU utilization. Because of the trade-offs associated with SR-IOV (no hypervisor intervention, no vMotion on vSphere, etc.), it's not a great general-purpose solution. It is, however, very well-suited to workloads that need predictable performance and that work with lots of small packets (firewalls, load balancers, other network devices).

Going back to the earlier discussion about PCIe root ports being integrated into the E5 CPUs, this leads to a consideration for the placement of PCIe cards. Make sure your high-speed cards aren't inserted in a slot that runs through the C600 chipset southbridge. Make sure that you are using Gen2 x8 slot, and make sure that the slot is actually wired to support a x8 card (some slots on some systems have a x8 connector but are only wired for x4 throughput). Johnson recommends using either LoM, slot 2, slot 3, or slot 5 for 10 GbE PCIe NICs; this will ensure direct connections to one of the CPUs and not to the southbridge chipset.

Johnson next transitions into a discussion of VF failover using NIC teaming software. There's a ton of knowledge disclosed (too much for me to capture; I'll try to do a separate blog post on it). The key takewaway: don't use NCI teaming in the guest when using SR-IOV VFs, or else traffic patterns could vary dramatically and create unpredictable results without very careful planning. Johnson also mentions DPDK; see [this post][1] for more details.

At this point, Johnson wraps up the session with a summary of key Intel initiatives with regard to networking (optimized drivers and initiators, intelligent use of offloads, everything based on open standards) and then opens the floor to questions.

[1]: {{< relref "2012-09-11-coms002-next-generation-cloud-infrastructure-with-data-plane-virtualization.md" >}}
