---
author: slowe
categories: Liveblog
comments: true
date: 2013-09-12T15:48:41Z
slug: idf-2013-enabling-nfvsdn-with-intel-onp-server-reference-design
tags:
- Hardware
- IDF2013
- Networking
- OVS
- SDN
title: 'IDF 2013: Enabling NFV/SDN with Intel ONP Server Reference Design'
url: /2013/09/12/idf-2013-enabling-nfvsdn-with-intel-onp-server-reference-design/
wordpress_id: 3295
---

This is session COMS003, titled "Enabling Network Function Virtualization and Software Defined Networking with the Intel Open Network Platform Server Reference Architecture Design." (How's that for a mouthful!) The speakers are Frank Schapfel, Senior Product Line Manager with Intel, and Brian Skerry, Open Networks System Architect with Intel. This session is slightly related to [IDF 2013 session COMS0002][1], which focused more on the Intel ONP Switch reference design.

Frank kicks the session off with a quick review of the agenda, then dives right into the content. He starts first with reviewing what SDN and NFV are; I won't repeat all that here again since it's already been covered multiple times. (See [the liveblog from the COMS002 session][1] for more details, if you need them.)

Next, Frank moves into Intel's role in enabling SDN/NFV. The key takeaway is that Intel's CPUs are gradually "eating away" at typically-proprietary functions like packet processing and signal processing. With these function now possible in x86_64 CPUs, they can be moved into a VM to help achieve NFV. (It could be argued that full machine virtualization might not be the most efficient way of handling NFV. Lightweight containers might be more efficient.) According to Frank, once NFV has been addressed this enables SDN, which he describes as greater automation across the network via a separated control plane. Naturally, a series of Intel ingredients underpin this: Intel CPUs, Intel NICs, switch silicon (the FM6700), Intel DPDK, and Open Networking Software.

This leads Frank into a discussion of how Intel will address this market moving forward. In this year, Intel has the Intel platform for Communications Infrastructure, leveraging Xeon and Avoton CPUs. Next year and in 2015, you can expect Intel to leverage the Haswell microarchitecture to refresh this platform. Beyond that, future microarchitectures will deliver more capabilities and more capacity that can be brought to bear on the SDN/NFV market.

At this point, Frank transitions into a more detailed and specific discussion of the ONP Server reference platform (code-named "Sunrise Trail"). The platform leverages Xeon E5-2600 v2 CPUs, plus a host of other Intel technologies (SR-IOV and packet acceleration via DPDK). Of particular note is the use of the Intel QuickAssist Services Acceleration Card (QASAC), which has its own PCI connection to the CPU cores and are designed to help accelerate tasks like encryption and compression. QASAC can offer up to 50Gbps of encryption/compression acceleration, with higher levels available via additional PCIe Gen 3 add-in cards.

Both Seacliff Trail (ONP Switch) and Sunrise Trail (ONP Server) will evolve over time as rack-scale architecture (RSA) matures and evolves as well. Eventually, Seacliff Trail and Sunrise Trail could eventually merge as part of RSA (referred to as Intel ONP for Hybrid Data Plane). Note that the merging of ONP Server and ONP Switch is something that I postulated last year after IDF 2012.

Sunrise Trail will leverage one of a number of potential enterprise Linux distributions, integration with OpenStack, the Intel DPDK for packet acceleration, various hypervisors will be supported (KVM, Hyper-V, KVM), support for OpenFlow (which will undoubtedly come via [Open vSwitch [OVS]](http://openvswitch.org/)). For telco environments, ONP Server will likely leverage Wind River Systems' real-time Linux distribution along with other components.

Frank now turns it over to Brian, who will discuss some of the software pieces involved in ONP Server. He first shows a high-level architecture (I tweeted a picture of this separately). Brian notes that this architecture does not map directly to the ETSI NFV architecture.

Some key challenges that this architecture faces:

* Integration of legacy OSS/BSS systems

* Element management needs to work in a virtualized environment

* Infrastructure orchestration such as OpenStack has industry momentum, but challenges still remain

* SDN controller architectures and the marketplace are still evolving

* Service orchestration is being addressed through a number of organizations with lots of opportunity for commercial and open source solutions

Brian takes a moment to zoom in a bit on OpenStack as an infrastructure orchestrator. He calls out enhanced platform awareness (making OpenStack aware of underlying platform capabilities, such as TCP) and passthrough of PCI devices and VF assignment (when using SR-IOV). He really focuses on platform awareness, which makes sense since Intel needs to differentiate at the platform level.

The discussion now shifts to a more focused discussion on the software that actually runs inside Sunrise Trail. Brian mentions the importance of an Intel DPDK vSwitch (which is typically a DPDK-accelerated version of OVS). The reason a DPDK-accelerated virtual switch is so important is because the virtual appliances that are leveraged by NFV will quickly become a bottleneck if the virtual switch isn't getting accelerated by the underlying hardware. Brian mentions some performance figures: stock OVS gets about 300K small packets per second, but doesn't yet provide any DPDK-accelerated numbers. Source code for DPDK acceleration is available at http://01.org, although it is missing some features (it is _not_ feature comparable with stock OVS right now). Brian issues a call to contributors to their effort, but I wonder why they don't just contribute their effort to stock OVS and leverage that community.

DPDK also enables other functions like deep packet inspection (DPI) and Quality of Service (QoS) fine-grained control.

Brian now turns it back over to Frank, who provides more information on where attendees can get more information on Intel's SDN/NFV enables. He points attendees to Intel's Network Builders program, provides a summary of the key points from the session, and then opens for questions and answers.

[1]: {{< relref "2013-09-12-idf-2013-future-of-sdn-with-the-intel-onp-switch-reference-design.md" >}}
