---
author: slowe
categories: Liveblog
comments: true
date: 2013-04-15T13:39:03Z
slug: openstack-summit-2013-nicira-nvp-deep-dive
tags:
- Networking
- OpenStack
- NVP
- NSX
title: 'OpenStack Summit 2013: Nicira NVP Deep Dive'
url: /2013/04/15/openstack-summit-2013-nicira-nvp-deep-dive/
wordpress_id: 3134
---

This is a "201 level" technical deep dive on Nicira Network Virtualization Platform (NVP). It was supposed to be led by Brad Hedlund, but due to unforeseen circumstances Brad was unable to make it to the Summit. Instead, Dan Wendlandt is leading the session. Dan, if you aren't familiar, was the former Quantum PTL. I'm already familiar with all of this stuff (naturally), but wanted to sit in this session so that I could liveblog it for others' benefit.

Dan starts the session with an explanation of network virtualization and why its important. He provides a technical definition of network virtualization as a "faithful reproduction of physical networks' that is "fully isolated" and provides "location independence" while being "physical network state independent". This is **NOT** the same as simply running network software inside a VM. (While that is valuable, that's not the same as network virtualization.)

NVP 1.0 was released in July 2011; the latest version, NVP 3.0, was released recently. The end of April will see NVP 3.1.

The only requirement from the physical network is IP connectivity. Open vSwitch (OVS) is used in all the various components, like Service Nodes and L2/L3 Gateways, and a set of out-of-band controllers manage all the components. Dan contrasts the "non-virtualized" (more physically-oriented) and "virtualized" (more logically oriented) views of NVP and the functionality is presents.

Starting at the "bottom" of the stack:

* NVP wants to treat your network as a pool of resource capacity to be sliced on-demand for tenants.

* NVP relies only on commodity features (specifically, L3 forwarding).

* Configuration is done once (when the equipment is racked).

* No humans in the loop when provisioning occurs.

* Have the flexibility to choose/change architecture without it impacting the logical abstractions you've created for your workloads.

Next Dan shows that you could use a very scalable leaf-spine L3 network design, but you could also leverage existing network architectures---NVP only requires L3 connectivity.

The session then moves on to discuss Open vSwitch (OVS), which forms the basis for many of the Quantum plugins in OpenStack. It's important to understand OVS is really like a generic "engine" that can be programmed to provide various levels of functionality, from "dumb" L2 learning to more sophisticated OpenFlow-based and tunnel-oriented networking.

This leads into a discussion of why L2/L3 tunneling as employed by NVP is important in satisfying the definition of network virtualization provided earlier. In order to truly isolate networks, NVP leverages tunneling protocols to encapsulate the traffic and provide the isolation and decoupling properties.

It is important to note, as Dan does, that a tunneling protocol alone is not network virtualization.

Next Dan moves on to describing the NVP controllers, which are x86-based software that manages OVS southbound and uses a northbound API (to expose to Quantum, for example). NVP controllers are **NEVER** in the data plane. The NVP controllers leverage a clustered/distributed architecture that provides both high availability (HA) and scale-out functionality.

As mentioned earlier, the controllers also provide the NVP API northbound to applications and/or cloud management systems (CMSes, like OpenStack and CloudStack). In an OpenStack environment, the NVP API is consumed by Quantum to create logical networks and logical network services.

Dan next provides an overview of the communication flow between the various components in a Quantum+NVP deployment.

The session now shifts focus to gateways, which provide the mechanism to get into or out of logical networks created by NVP. An L2 gateway allows you to map physical systems or VLANs into a logical switch managed/created by NVP. (Kudos to Dan, who actually goes with a very complex example-mapping in a remote VLAN across the WAN as an adjacent L2 segment-in order to explain L2 gateways.) He also explains the use of L3 gateways, which provide routed access into and out of the NVP logical networks. One of the interesting things about L3 gateways is that they are highly available (using failure zones) and a scale-out architecture (meaning that multiple L3 gateway appliances are supported).

Service nodes are used to handle broadcast and multicast packet replication. Using service nodes allows you to offload these tasks, if you wish, from the hypervisor compute nodes. Like the L3 gateways, the service nodes are built to provide high availability and scale-out functionality.

Dan wraps up the session with a review of the management and operations tool that NVP provides. NVP provides tunnel status, has a "port connectivity" tool that shows VM-to-VM connectivity, and actively inject traffic into flows to test network connectivity.

NVP isn't just about scale. It's about:

* Data plane performance

* Fast, reliable high availability

* Rich logical capabilities (ACLs, QoS, statistics)

* Ability to bring physical workloads into logical networks, onboard remote customers

* Rich management and operations tools

At this point, Dan opens the session for questions and answers.
