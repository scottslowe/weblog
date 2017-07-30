---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-16T19:05:52Z
slug: openstack-summit-2013-networking-in-the-cloud-an-sdn-primer
tags:
- Networking
- OpenStack
- Virtualization
title: 'OpenStack Summit 2013: Networking In the Cloud, an SDN Primer'
url: /2013/04/16/openstack-summit-2013-networking-in-the-cloud-an-sdn-primer/
wordpress_id: 3149
---

This is a session titled "Networking in the cloud: An SDN primer." It's led by Ben Cherian, Chief Strategy Officer for Midokura. Ben indicates he'll try to remain as impartial as possible while attempting to describe and define SDN.

Ben asserts that the basic driver pushing folks toward SDN is that the current state of networking in the cloud is too complex and too manual. As a result, the first question that people start asking is, how can I automate this? Telecom has gone through this before, and data networking is in a similar state of flux and development.

The presenter next discusses Almon Strowger, who invented the first electromechanical switch. His work---which might have been driven by some level of paranoia---led to automated phone switching and the rotary phone. Almon Strowger wanted to address privacy concerns and intentional human errors, and along the way he also solved unintentional human errors, connection speeds, and lower operational costs.

Ben indicates that the cloud---in particular, networking---is in a similar position. It's time for the "Almon Strowger" of cloud networking to solve the challenges that keep networking from scaling.

The presenter next covers the concept of a control plane and the data plane. Abstraction is a key tenet of computer science, and when the control plane and the data plane reside on the same physical device (which is typically the case in traditional networks today), there is no abstraction. Abstraction exists in other areas of computing, but not in networking. To bring that abstraction into networking, a simple example is to separate the control plane into a separate controller that exists apart from the data plane. This is basic SDN.

Ben sees three use cases for SDN:

1. IaaS

2. Data center fabrics

3. Carrier/WAN use cases

Looking at these in reverse order, examples of SDN technologies that you could see here would be a "hybrid control plane" leveraging either BGP (a distributed control plane) or OpenFlow (centralized control plane). In the data center fabric space, this involves the use of OpenFlow to manage multiple switches. Finally, in the IaaS space, you see software-based solutions and overlays. Examples of companies in this IaaS space are Midokura, VMware/Nicira, and others.

Some key requirements for IaaS "cloud networking"/SDN:

* Multi-tenancy

* L2 isolation

* L3 isolation

* Scalable control plane

* NAT (floating IP)

* ACLs

* Stateful (L4) firewall

* VPN

* BGP gateway

* RESTful API

* Integration with CMS (like OpenStack)

Looking at this list of requirements, Ben feels like you can eliminate the technologies leveraged in the carrier/WAN and data center fabric SDN use cases, because these technologies don't properly address all the IaaS/cloud networking requirements.

Ben next reviews a networking diagram that shows how these various requirements translate into cloud networks.

The candidate models to address these requirements are:

* Traditional network

* Hop=by-hop OpenFlow

* Edge-to-edge IP overlays

Using traditional network models, VLANs become a constraint (only 4096 VLANs available means only 4096 tenants). This constrains L2 isolation. L3 isolation is constrained by VRFs, which are not fault tolerant, require expensive hardware, and don't scale well.

If you wanted to use an OpenFlow fabric, the issue is more about the limitations of the physical switch itself. Storing the state in physical switches has issues with scale, not fast enough to update, and no atomicity of updates. There is also the issue of provisioning the physical switches that will then be controlled by OpenFlow. This approach also doesn't address the other "higher level" concerns of cloud networking.

Edge-to-edge IP overlays are the method that Ben (and his company) prefers. Isolation is provided without VLANs, providing additional scalability. Only IP connectivity is required. You can use a scalable IGP (iBGP, OSPF) to build a reliable multi-path underlay network. This sort of thinking is inspired by a research paper by Microsoft regarding VL2 (no link provided).

Trends that support this sort of solution:

* Faster packet processing on x86 servers at the edge

* Clos (fat tree/leaf-spine) networks for the underlay

* Merchant silicon brings down the cost of efficient IP switches

* Optical intra-DC networks for plentiful bandwidth

The presenter also alludes to the use of configuration management tools (Chef, Puppet) on merchant silicon-based switches.

Ben next shows a diagram that illustrates an overlay network and an underlay network, and he walks through how traffic flows work (both from the overlay perspective as well as the underlay perspective).

Naturally, Ben believes that overlays are the right approach---but you still need a scalable control plane.

At this point, he opens the session up for questions and answers.
