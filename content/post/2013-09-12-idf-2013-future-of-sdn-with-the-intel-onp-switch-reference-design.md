---
author: slowe
categories: Liveblog
comments: true
date: 2013-09-12T13:36:15Z
slug: idf-2013-future-of-sdn-with-the-intel-onp-switch-reference-design
tags:
- IDF2013
- Networking
- OpenFlow
- SDN
title: 'IDF 2013: Future of SDN with the Intel ONP Switch Reference Design'
url: /2013/09/12/idf-2013-future-of-sdn-with-the-intel-onp-switch-reference-design/
wordpress_id: 3293
---

This is session COMS002, titled "The Future of Software-Defined Networking with the Intel Open Network Platform Switch Reference Design." The speakers are Recep Ozdag, a PME with Intel, and Gershon Schatzberg, a PLM with Wind River Systems. The Intel Open Network Platform (ONP) Switch is the official name for Seacliff Trail, which I [talked about last year at IDF 2012][1].

Recep starts the session with some brief background; he came to Intel through the Fulcrum acquisition a couple of years ago. Recep will cover the hardware side of ONP; Gershon will cover the software side of ONP (referred to as ONS). First, though, Recep takes a few minutes to review some SDN concepts.

The first concept that Recep discusses is network virtualization. He believes that network virtualization is primarily about decoupling network services from the underlying network infrastructure. The virtual network must provide the same services as the physical network, but can provide greater flexibility, automation and programmability, and isolation. From the operator's point of view, network virtualization allows each tenant (or customer) to see a single network switch that is specific to that tenant or customer.

So how does this work? Recep talks about how the predominant architecture for network virtualization involves the use of overlay networks created and managed at the edge by virtual switches in the hypervisors. This means using an encapsulation protocol to decouple the logical network topology from the physical network topology. The specific terminology involved is that the virtual switches become a VTEP, a virtual tunnel end-point. These VTEPs are responsible for encapsulation and decapsulating traffic.

ONP fits into this picture by performing VTEP functions in hardware (at line rate). This is particularly helpful for bridging physical/non-virtualized workloads into the virtual networks. ONP also helps by being a "tunnel-aware" switch, which can extract Quality of Service (QoS) information from the encapsulated traffic and direct traffic accordingly across the network. (Network virtualization solutions can also copy QoS information into the outer headers as well.)

Also related to network virtualization, according to Recep, is Network Functions Virtualization (NFV). NFV is intended to address the problem caused by having to route/direct traffic from various sources through physical appliances designed to provide services like content filtering, security, content delivery/acceleration, and load balancing. Rather than having physical appliances scattered throughout the network, NFV allows us to provision these services as a VM, which provides greater agility, greater scale, and greater efficiency. Some of these services naturally should run on the top-of-rack (ToR) switch, like load balancing or security services.

In summary, the goal of SDN/NFV is to enable architectural transformation that moves from dedicated hardware appliances to software running on x86 servers.

Next, Recep discusses the need for separated control and data planes to enable greater automation of network configuration tasks so as to allow the network to respond more quickly to changing business needs.

That leads Recep into a discussion of what ONP actually is. ONP is an SDN-optimized reference platform that combines Intel x86 CPUs plus the switching-optimized FM6700 silicon and software from Wind River Systems (referred to as Open Network Software [ONS]). The Seacliff Trail reference platform uses the ToR form factor, configured with 48 10GbE ports and 4 40GbE ports. This is the first form factor for Intel ONP, but not the only form factor planned:

* ONP Rack Scale Architecture (RSA) switching module

* ONP micro-server switching module

From a roadmap/timeline perspective, you might expect to see micro-server switching modules next year and RSA switching modules in 2015.

At this point, Gershon from Wind River takes over to discuss the software side of ONP (ONS). So, what is ONS? ONS was designed from the ground up to be optimized for SDN, offering a complete network software environment. ONS offers both native layer 2/2+ and layer 3 protocol support, supports OpenFlow, supports a range of management interfaces (XML-RPC, SNMP, NETCONF), and offers a "legacy" CLI.

So what is offered to customers as a product? In this case, it sounds like Wind River's customers are OEMs/ODMs, not end users. What's offered as a product is the ONS itself, SimSwitch (a switch simulator), a basic test suite, Wind River Linux (which provides the basis for ONS), and optional professional services to help customers develop additional features and functionality.

Gershon claims that ONS is the first and only SDN-optimized software environment. (I'm not sure I agree with that statement.) It was designed to be easily integrated with switching hardware, extensible via APIs, scalable programmability, and easy maintenance/troubleshooting/support. ONS is object-oriented and compiles on every major operating system (Linux and OS X specifically called out). It is hardware switch independent. Wind River worked hard to make it easy for developers to use this as the basis for their customization and extensibility efforts. To help with that, Wind River also offers an Application Development Kit (ADK), which will allow customers to develop additional applications, additional protocols, additional tools that will run over ONS (the Wind River software) on ONP (the Intel hardware reference platform).

Gershon now reviews a diagram that shows the ONS software architecture. I separately tweeted a picture of this software architecture. Not unsurprisingly, the ONS software architecture features [Open vSwitch (OVS)](http://openvswitch.org/) as a core component. ONS also integrated Quagga for layer 3 functions like OSPF, ARP, and BGP.

Next, Gershon reviews some additional details about the various layers of the ONS software architecture, most of which was directed primarily toward developers who would be building applications and/or services on top of ONS for their end-customers.

At this point, Recep and Gershon provide a quick summary of the key points, and then open the session up for questions and answers.

[1]: {{< relref "2012-09-12-idf-2012-day-1-summary-and-thoughts.md" >}}
