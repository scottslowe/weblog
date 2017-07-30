---
author: slowe
categories: Liveblog
comments: true
date: 2011-08-31T16:33:35Z
slug: vsp2117-virtualization-aware-datacenter-networks
tags:
- Networking
- Virtualization
- VMware
- VMworld2011
- vSphere
title: 'VSP2117: Virtualization-Aware Datacenter Networks'
url: /2011/08/31/vsp2117-virtualization-aware-datacenter-networks/
wordpress_id: 2387
---

This is a session blog for VSP 2117, titled Virtualization-Aware Datacenter Networks. The presenter is Milin Desai, Group Product Manager for Networking at VMware. This is the session where I will, hopefully, get the answers I need for VXLAN.

Milin indicates that customers---he says he's talked to over 1,000 customers---want mobility. They want planned migrations, disaster recovery, and live migrations. So how is the datacenter network going to evolve to meet these customer requirements?

* In the future, workloads will be dynamic ("any app to any server") and will support massive scale (more than 4,095 VLANs).

* Services will be distributed scale-out services and will be workload-centric (programmed to the specific workload). Both policies and control plane information must travel with the workload.

* Operations must be simplified (single interface for provisioning) and the solution must provide deterministic performance with visibility and monitoring.

Summarizing these points, the solution must be elastic and extensible, performant, and programmable.

The basis of a solution that is elastic and extensible is the vSphere Distributed Switch. A customer takes the stage to talk about how he has used vSphere Distributed Switches and the benefits his organization has seen.

VXLAN is the VMware solution that enables VMs to move across subnet boundaries. VXLAN is about improved workload mobility. VXLAN (formerly VDL2) uses a MAC-in-UDP encapsulation. It looks as if VXLAN will work between different vSphere Distributed Switches (different than what I heard at the Cisco booth yesterday), and the traffic flow across the network is unaffected.

Next Milin moves into a discussion of how to use vShield Manager to provide extensible services to third party developers. vShield Edge is VMware's own product that leverages vShield Manager's extensibility to provide firewall, DHCP, NAT, VPN, and load balancing services. vShield Edge supports VXLAN, but VXLAN does not address Layer 3 concerns. Milin mentions that if we have to migrate VMs, you would still have to change the vShield Edge NAT configuration and update DNS. It would appear, based on this information, that technologies such as OTV and LISP (or their equivalents) are still going to be necessary.

With regards to performance, Milin mentions Network I/O Control, and how vSphere 5.0 adds 802.1p tagging to help with the end-to-end QoS configurations. Looking ahead, how will this evolve? He discusses the use of advanced technologies like ROCE/iWarp, SR-IOV, etc. These technologies will potentially let VMware address low latency workloads (less than 50 microseconds). On the visibility front, Milin discusses such things as NetFlow, SNMP, and the use of tools like vCenter Operations. VMware is also working on technologies that will do a better job of providing automated alerts with regard to network configuration mismatches (MTUs, VLANs, etc.).

In the area of programmability, VMware is looking at "Network as a Service"---publishing network services/capabilities to workloads. Milin again uses the "barcode" example, where a VM contains the specific requirements that it needs with regard to the network connectivity. This is a concept that VMware has been discussing for quite some time but is still **very** early in the process of actually delivering.

At this point, Milin ends his presentation and opens the floor to questions.
