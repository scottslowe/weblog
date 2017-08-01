---
author: slowe
categories: Liveblog
comments: true
date: 2014-09-09T15:18:29Z
slug: idf-2014-virtualizing-the-network-to-enable-sdi
tags:
- Hardware
- IDF2014
- Intel
- Networking
title: 'IDF 2014: Virtualizing the Network to Enable SDI'
url: /2014/09/09/idf-2014-virtualizing-the-network-to-enable-sdi/
wordpress_id: 3515
---

This is a liveblog of IDF 2014 session DATS002, titled "Virtualizing the Network to Enable a Software-Defined Infrastructure (SDI)". The speakers are Brian Johnson (Solutions Architect, Intel) and Jim Pinkerton (Windows Server Architect, Microsoft). I attended [a similar session last year][1]; I'm hoping for some new information this year.

Pinkerton starts the session with a discussion of why Microsoft is able to speak to network virtualization via their experience with large-scale web properties (Bing, XBox Live, Outlook.com, Office, etc.). To that point, Microsoft has over 100K servers across their cloud properties, with >200K diverse services, first-party applications, and third-party applications. This amounts to $15 billion in data center investments. Naturally, all of this runs on Windows Server and Windows Azure.

So why does networking need to be transformed for the cloud? According to Pinkerton, the goal is to drive agility and flexibility for your business. This is accomplished by pooling and automating network resources, ensuring tenant isolation, maximizing scale/performance, enabling seamless capacity expansion and workload mobility, and minimizing operational complexity.

Johnson takes over here to talk about how Intel is working to address the challenges and needs that Pinkerton just outlined. This breaks down into three core areas that have unique requirements and capabilities: network functions virtualization (NFV), network virtualization overlays (NVO), and software-defined networking (SDN).

Johnson points out that workload optimization is more than just networking; it also involves CPU (E5-2600 v3 CPU family), network connectivity (Intel XL710, now offering support for next-generation Geneve encapsulation), and storage (Intel SSDs). Johnson dives deep on the XL710, which was specifically designed to address some of the needs of cloud networking. Particularly, support for a variety of encapsulation protocols (NVGRE, IPinGRE, MACinUDP, VXLAN, Geneve), support for 40Gbps or 4x10Gbps connectivity in the same card, support for up to 8000 perfect match flow filters stored on die (this is Intel Ethernet Flow Director), and support for SR-IOV and VMDq are all areas where this card helps with NVO and SDN applications.

Next up Johnson walks through some behaviors in traditional networking as compared to network virtualization using an encapsulation protocol. Johnson uses two examples, one with VXLAN and one with NVGRE, but the basics between the two examples are very similar. Johnson also talks about why the stateless offloads in the XL710 (now supporting stateless offloads for both VXLAN and NVGRE, as well as next-generation Geneve) is important; this offloads some amount of work from the host CPU. The impact of network overlays on NIC bonding and link aggregation is another consideration; adapters and switches may not be aware of the encapsulation headers and therefore may not fully utilize all the links in a link aggregation group. The Intel X520/X540 had some offloads; the XL710 increases this support.

That wraps up the NVO portion, and now Johnson switches gears to talk about NFV. According to Johnson, service function chaining (SFC) is a key component of NFV. There are two options for SFC: Network Services Header (NSH), or Geneve. Johnson points out that Geneve was co-authored by Intel, MIcrosoft, VMware, and Red Hat, and is considered to be the next-generation encapsulation protocol. This leads Johnson into a live demo of Geneve and the importance of RSS. (Without RSS, bandwidth is constrained on the receiving system.)

One other key area for support of NFV is being able to transmit large numbers of small packets. This is enabled by Intel's work on the Data Plane Development Kit (DPDK).

Johnson points out that 40Gbps Ethernet will not offer a BASE-T option; to help address 40Gbps connectivity, Intel is introducing new, low-cost optics (both transceivers and cables). Estimated cost for Intel Ethernet MOC (Modular Optical Connectors) is around $400--well down from costs like $1300 today.

Pinkerton now takes over again, talking about VM density and the changes that have to take place to support higher VM density in private cloud environments (although I would contend that highly virtualized data centers are _not_ private clouds). In particular, Pinkerton feels that SMB3 and SMB Direct (RDMA support) are important developments. According to Pinkerton, these protocols address the need for lower network and storage CPU overhead, higher throughput requirements, lower variances in latency and throughput, better fault tolerance, and VM workload isolation.

Pinkerton insists that using file sharing semantics is actually a much better approach for cloud-scale properties than using block-level semantics (basically, SMB3 is better than iSCSI/FC/FCoE). That leads to a discussion of RDMA (Remote Direct Memory Access), and how that helps improve performance. Standardized implementations of RDMA include iWARP (RDMA over TCP/IP) and RoCE (RDMA over Converged Ethernet). InfiniBand also typically leverages RDMA. In the context of private cloud, having the ability to route traffic is important; that's why Pinkerton believes that iWARP and RoCE v2 (not mentioned on the slide) are important.

That leads to a discussion of some performance results, and Pinkerton calls out incast performance (many nodes sending data to a single node) as an important metric in private cloud environments. In reviewing some performance metric for using RDMA, Pinkerton states that average latency is no longer satisfactory as a metric---instead, organizations should focus on 95th percentile and 99th percentile measurements instead of average. The metrics Pinkerton is using (based on tests with a Chelsio T580) show latency with SMB3 and RDMA to be very stable up to 90% load, and throughput is near line-rate.

Johnson takes back over now to announce that iWARP support will be built into the next generation of Intel NIC chipsets as a default for server environments.

At this point the session wraps up.

[1]: {{< relref "2013-09-11-idf-2013-virtualizing-the-network-to-enable-sdi.md" >}}
