---
author: slowe
categories: Liveblog
comments: true
date: 2012-09-11T16:46:19Z
slug: coms002-next-generation-cloud-infrastructure-with-data-plane-virtualization
tags:
- IDF2012
- Intel
- Networking
- Virtualization
title: 'COMS002: Next Generation Cloud Infrastructure with Data Plane Virtualization'
url: /2012/09/11/coms002-next-generation-cloud-infrastructure-with-data-plane-virtualization/
wordpress_id: 2844
---

This is session COMS002, titled "Next Generation Cloud Infrastructure with Data Plane Virtualization." The speaker is Edwin Verplanke, a System Architect with Intel.

Verplanke believes that DPDK (Data Plane Development Kit) and virtualization are key to virtualizing workloads that move around lots and lots of packets, such as firewalls, routers, and other similar functions. Late in the session Verplanke will discuss some Open vSwitch optimizations.

Verplanke first goes over some of the challenges that are driving the industry to look at data plane and control plane virtualization solutions in next-generation infrastructure. Next, he discusses the evolution of network devices so far. Devices first started as tightly-coupled hardware and software solutions. In recent years, we've seen more devices running off-the-shelf software (like Linux). The future, Verplanke believes, is fully virtualized network devices.

Verplanke describes three use cases for virtualized communications platforms: security appliance virtualization, router and edge device virtualization, and service delivery platform virtualization.

Each of these use cases has some challenges. For the security appliance, all traffic is actually intercepted by the hypervisor, which will generate context switches for every I/O packet received. This will drive down performance. This is quite different from virtualizing an endpoint device.

When virtualizing router and edge devices, not only do you encounter the same I/O performance issue as with security devices, you also need to enable very efficient intra-VM communications to enable multiple networking services (like SSL/TLS or IPSec).

The third use case, virtualizing a virtual service delivery platform, needs an efficient programmable interconnect for deployment of new services. Is the Linux bridge efficient enough? Intel is looking at Open vSwitch as a way to address that; more information on that later. This is similar to the issue described with virtualizing security devices.

These challenges can be summarized as:

1. I/O-intensive application virtualization

2. Intra-VM communication

3. Efficient VMM softswitch implementation

For challenge #1, we'll look at data plane virtualization. For #2, we'll examine the Intel DPDK. And for #3, we'll examine some Open vSwitch optimizations.

Intel DPDK is a standard set of libraries to move packets around a system. The DPDK is BSD licensed and source code is available from Intel. The DPDK contains data plane libraries, optimized NIC poll mode drivers, a runtime environment, a new queuing environment for the Linux kernel, and an Environment Abstraction Layer (EAL) that automatically optimizes for various Intel architectures (Atom, Core, Xeon).

Verplanke next reviews some of the I/O optimizations and architecture of the Xeon E5 2600 series platform. These optimizations include things like Data Direct I/O (DDIO), VT-d, QPI, and QuickAssist (this helps with pattern matching, crypto, and compression).

The Intel hardware virtualization assists (things like Extended Page Tables, large VT-d page support, reduced context switch latency) also greatly improve with the virtualization of communications-related infrastructure. EPT and VT-d large page support are particularly important. (Note from my earlier Haswell session blog that Intel is further reducing virtualization-related context switch latency even more.)

To help with intra-VM communication, Intel DPDK offers several benefits. First, DPDK provides optimized pass-through support. Second, DPDK offers SR-IOV support and allows L2 switching in hardware on Intel's network interface cards (estimated to be 5-6x more performant than the soft switch). Third, DPDK provides optimized vNIC drivers for Xen, KVM, and VMware.

Verplanke shows off how the SR-IOV virtual function (VF) support works with an Intel 82599 10 Gigabit Ethernet NIC.

Finally, Verplanke discusses some Open vSwitch optimizations. The current Open vSwitch architecture is fine for endpoint virtualization, but not for network data plane virtualization. What Intel has been working on is replace the VirtIO drivers with DPDK-enabled VirtIO drivers, and use DPDK to replace the Linux bridging utilities with a DPDK-enabled forwarding application. The DPDK-enabled forwarding application is a "zero copy" operation, thus reducing latency and processing load when forwarding packets. Intel is also creating shims between DPDK and Open vSwitch, so that an OVS controller can update Open vSwitch, which can then update the DPDK forwarding app to modify or manipulate forwarding tables.

At this point, Verplanke summarized what he's discussed in the session and describes the benefits of DPDK for data plane virtualization.
