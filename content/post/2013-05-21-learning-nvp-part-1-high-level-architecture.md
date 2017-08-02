---
author: slowe
categories: Explanation
comments: true
date: 2013-05-21T16:01:51Z
slug: learning-nvp-part-1-high-level-architecture
tags:
- Networking
- Nicira
- NVP
- OpenFlow
- OVS
- Virtualization
title: 'Learning NVP, Part 1: High-Level Architecture'
url: /2013/05/21/learning-nvp-part-1-high-level-architecture/
wordpress_id: 3188
---

This blog post kicks off a new series of posts describing my journey to become more knowledgeable about the Nicira Network Virtualization Platform (NVP). NVP is, in my opinion, an awesome platform, but there hasn't been a great deal of information shared about the product, how it works, how you configure it, etc. That's something I'm going to try to address in this series of posts. In this first post, I'll start with a high-level description of the NVP architecture. Don't worry---more in-depth information will come in future posts.

Before continuing, it might be useful to set some context around NVP and NSX. This series of posts will focus on NVP---a product that is available today and is currently in use in production. The architecture I'm describing here will also be applicable to NSX, which VMware announced in early March. Because NSX will leverage NVP's architecture, spending some time with NVP now will pay off with NSX later. Make sense?

Let's start with a figure. The diagram below graphically illustrates the NVP architecture at a high level:

![High-level NVP architecture diagram](/public/img/nvp-architecture.png)

The key components of the NVP architecture include:

* _A scale-out controller cluster:_ The NVP controllers handle computing the network topology and providing configuration and flow information to create logical networks. The controllers support a scale-out model for high availability and increased scalability. The controller cluster supplies a northbound REST API that can be consumed by cloud management platforms such as OpenStack or CloudStack, or by home-grown cloud management systems.

* _A programmable virtual switch:_ NVP leverages Open vSwitch (OVS), an independent open source project with contributors from across the industry, to fill this role. OVS communicates with the NVP controller clusters to receive configuration and flow information.

* _Southbound communications protocols:_ NVP uses two open communications protocols to communicate southbound to OVS. For configuration information, NVP leverages OVSDB; for flow information, NVP uses OpenFlow. The management (OVSDB) communication between the controller cluster and OVS is encrypted using SSL.

* _Gateways:_ Gateways provide the "on-ramp" to enter or exit NVP logical networks. Gateways can provide either L2 gateway services (to bridge NVP logical networks onto physical networks) as well as L3 gateway services (to route between NVP logical networks and physical networks). In either case, the gateways are also built using a scale-out model that provides high availability and scalability for the L2 and L3 gateway services they host.

* _Encapsulation protocol:_ To provide full independence and isolation of logical networks from the underlying physical networks, NVP uses encapsulation protocols for transporting logical network traffic across physical networks. Currently, NVP supports both Generic Routing Encapsulation (GRE) and Stateless Transport Tunneling (STT), with additional encapsulation protocols planned for future releases.

* _Service nodes:_ To offload the handling of BUM (Broadcast, Unknown Unicast, and Multicast) traffic, NVP can optionally leverage one or more service nodes. Note that service nodes are optional; customers can choose to have BUM traffic handled locally on each hypervisor node. (Note that service nodes are not shown in the diagram above.)

Now that you have an idea of the high-level architecture, let me briefly outline how the rest of this series will be organized. The basic outline of this series will roughly correspond to how NVP would be deployed in a real-world environment.

1. In the next post (or two), I'll be focusing on getting the controller cluster built and diving a bit deeper into the controller cluster architecture.

2. Once the controller cluster is up and running, I'll take a look at getting NVP Manager up and running. NVP Manager is an application that consumes the northbound REST APIs from the controller cluster in order to view and manage NVP logical networks and NVP components. In most cases, this function is part of a cloud management platform (such as OpenStack or CloudStack), but using NVP Manager here allows me to focus on NVP instead of worrying about the details of the cloud management platform itself.

3. The next step will be to bring hypervisor nodes into NVP. I'll focus on using nodes running KVM, but keep in mind that Xen is also supported by NVP. If time (and resources) permit, I may try to look at bringing up Xen-based hypervisor nodes as well. Because NVP leverages OVS as the edge virtual switch, I'll naturally be discussing some OVS-related tasks and topics as well.

4. Following the addition of hypervisor nodes into NVP, I'll look at creating a simple logical network, and we'll examine how this logical network works with the underlying physical network.

5. To add more flexibility to our logical network, we need to be able to bring physical resources into NVP logical networks. To enable that functionality, we'll need to add gateways and gateway services to our configuration, so I'll discuss gateways and L2 gateway services, how they work, and how we add them to an NVP configuration.

6. The next step is to enable L3 (routing) functionality within NVP, and that is enabled by L3 gateway services. I'll spend some time talking about the L3 gateway services, their architecture, adding them to NVP, and including L3 functionality in an NVP logical network. I'll also explore distributed L3 routing, where the L3 routing is actually distributed across hypervisors in an NVP environment (this is a new feature just added in NVP 3.1).

7. Now that we have both L2 and L3 gateway services in NVP, I'll take a look at building more intricate logical networks.

Beyond that, it's hard to say where the series will go. I'll likely also take a look at some of NVP's security features, and examine a few more complex NVP use cases. If there are additional topics you'd like to see beyond what I've outlined above, please feel free to speak up in the comments below.

I'm excited about this journey to learn NVP in more detail, and I'm looking forward to taking all of you along with me. Ready? Let's go!
