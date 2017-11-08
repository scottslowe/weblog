---
author: slowe
categories: Liveblog
comments: true
date: 2017-11-07T21:30:00Z
tags:
- OpenStack
- SDN
- Networking
- Neutron
title: Carrier-Grade SDN-Based OpenStack Networking Solution
url: /2017/11/07/carrier-grade-sdn-openstack-networking-solution/
---

This session was titled "Carrier-Grade SDN Based OpenStack Networking Solution," led by Daniel Park and Sangho Shin. Both Park and Shin are from SK Telecom (SKT), and (based on the description) this session is a follow-up to a session from the Boston summit where SK Telecom talked about an SDN-based networking solution they'd developed and released for use in their own 5G-based network.<!--more-->

Shin starts the session with some presenter introductions, and sets the stage for the presentation. Shin first provides some background on SKT, and discusses the steps that SKT has been taking to prepare their network for 5G infrastructure/services. This involves more extensive use of virtual network functions (VNFs) and software-defined infrastructure based on open software and open hardware. Shin reinforces that the SKT project (which is apparently called COSMOS?) exclusively leverages open source software.

Diving into a bit more detail, Shin talks about SONA Fabric (which is used to control the leaf/spine fabric used as the network underlay), SONA (which handles virtual network management), and TACO (which is an SKT-specific version of OpenStack). The network monitoring solution is called TINA, and this feeds into an integrated monitoring system known as 3DV.

TACO (stands for SKT All Container OpenStack) is containerized OpenStack, leveraging Kubernetes and OpenStack-Helm. The container images come from Kolla and Docker is the container engine in use. TACO is built entirely from the upstream source code, which feeds into a CI/CD pipeline that generates working images, packaging, configuration, etc.

Park now takes over to discuss ONOS. ONOS is led by the ONF and is a "carrier-grade" network operating system; the first version was released in 2015. SKT now uses a test version of ONOS. Key features of ONOS are scalability, performance, high availability, a modular architecture that supports pluggable southbound interfaces, and a set of northbound abstractions for greater interoperability.

SONA (which stands for Simplified Network Overlay Architecture) is the SKT-specific solution designed on top of ONOS for SKT. All of SONA, according to Park, has been upstreamed into the ONOS repository. With regard to OpenStack, SONA comprises three different ONOS applications:

* OpenStackNode (initializes nodes)
* OpenStackNetworking (programs switching/routing rules via OpenFlow to OVS, both on compute nodes and gateway nodes)
* OpenStackNetworking UI (user interface)

On the data plane side, SONA uses OVS and a vRouter. No details are provided on this vRouter construct, other than it was developed by SKT for SONA.

Key features of SONA include:

* Implemented as an ML2 plugin for Neutron
* Uses VXLAN-based overlays
* No agents and no network node
* Virtual router scalability
* Optimized East-West routing (no virtual router needed)
* Provides a UI-based flow tracer

The text on the slide talks about no need for network nodes, but Park repeatedly mentions gateway nodes and the ability to scale gateway capacity; I'm guessing that gateway nodes are a replacement for network nodes. Gateways are responsible for handling North-South traffic flows.

Park talks for a minute about the inspiration for the flow tracing UI, which comes out of trying to troubleshoot traffic flows using OVS-related CLI commands and `grep`.

Next, Park shows a brief video demo of the flow tracing user interface.

Following the demo, Park outlines a few use cases for SONA, all of which involve integration between OpenStack networking and physical networking for the purpose of supporting physical network slicing, vEPC (virtual Evolved Packet Core), and VNF deployments.

SONA Fabric is a pure OpenFlow-based leaf/spine fabric solution, supporting ECMP, failure detection, auto-recovery, and physical/virtual network integration. SONA Fabric has a more limited hardware support list, due to pure OpenFlow architecture. I find it interesting that SKT says their solution is completely based on "open hardware," yet Cisco and Arista are mentioned as being supported by SONA Fabric. (Perhaps there is a disconnect/difference between the SONA Fabric support and what SKT specifically is using.)

Next, Park shows a video demo of TINA (SKT Integrated Network Analyzer), which provides 3-D visualization of network status and network performance information. It's quite impressive, to be honest.

At this point, Shin takes over to talk about what's missing for true carrier grade solution? The primary concern is the data plane, where performance for VNFs still can't match PNFs. The solution is to more fully embrace data plane acceleration using technologies like DPDK, NIC offloading, and Xeon+FPGA acceleration. Shin mentions that SKT is working to conduct a commercial proof-of-concept with TACO, SONA, and some data plane acceleration solutions early next year, and hopes to share some results next year.

At this point, the presenters wrap up the session.
