---
author: slowe
categories: Liveblog
comments: true
date: 2013-09-11T15:53:41Z
slug: idf-2013-rack-scale-architecture-for-cloud
tags:
- Hardware
- IDF2013
title: 'IDF 2013: Rack Scale Architecture for Cloud'
url: /2013/09/11/idf-2013-rack-scale-architecture-for-cloud/
wordpress_id: 3288
---

This is IDF 2013 session CLDS001, titled "Rack Scale Architecture for Cloud." The speaker is Mohan Kumar, a Sr Principal Engineer with Intel. Kumar works in the Server Platforms Group.

Kumar mentions that Krzanich mentions rack-scale architecture (RSA, not to be confused with a security company of the same name) as one of three pillars of accelerating the data center, and this session will be diving a bit deeper on rack-scale architecture. He'll start with an overview of the motivation for RSA, then provide an overview of RSA and how it works.

The motivation for developing RSA is really rooted in the vision of the "Internet of Things," which Intel estimates will reach approximately 30 billion devices by 2020. This means there will be tremendous need for servers in data centers to support this vast number of connected devices. However, the current architectures aren't sufficient. Resources are locked into individual servers, making it more difficult to shift resources as workloads change and adapt. (I'd contend that virtualization helps address most of this concern.) Thermal inefficiencies and a lack of service-based (software-defined?) configurability of resources are other motivations for RSA. (Again, I'd contend that the configurability of resources is mitigated somewhere by the extensive use of virtualization.) Finally, individual resources within a server can't be upgraded. To address these concerns, Intel believes that a rack-level architecture is needed.

So where does RSA stand today? Today, RSA can offer shared power (a single power bus instead of multiple power supplies in each server), shared cooling, and rack management (more intelligent in the rack itself). In the near future, Intel wants RSA to include a "rack fabric," using optical interconnects that allows for a much greater level of disaggregation and much greater modularity. The ultimate goal of RSA is completely modularized servers with pooled CPUs, pooled RAM, pooled I/O, and pooled storage. This is going to be a key point of Kumar's presentation.

So what are the key Intel technologies involved in RSA?

* Reference architectures and orchestration software

* Intel's Open Network Platform (ONP)

* Storage technologies, like PCIe SSD and caching

* Photonics and switch fabrics

* CPUs and silicon (Atom, Xeon, Quark?)

Intel wants RSA to align with Open Compute Platform (OCP) efforts as well. Kumar also mentions something called Scorpio, which I hadn't heard before (this is similar to OCP in some way).

In looking at how these components come together in RSA, Intel estimates that cloud operators would see the following benefits:

* 3x reduction in cable requirements using silicon photonics

* 2.5x improvement in network uplinks

* 25x improvement in network downlinks

* Up to 1.5x improvement in server/rack density

* Up to 6x reduction in power provisioning

Most of these improvements come from the use of silicon photonics, according to Kumar.

Looking ahead into the future of RSA, what are some of the key problems that remain to be solved? Kumar points to the following challenges:

1. There is no service-based configurability of memory. (What about virtualization here? I could see this argument for the virtualization hosts, but that scale will be vastly smaller than the scale for the VMs/instances themselves.) Kumar believes that pooled memory is the answer to this challenge.

2. Similarly, there is no service-based configurability for direct-attached storage. (My same comments regarding the pervasive use of virtualization applies here as well.) Kumar's response to this is a high-density Ethernet JBOD he calls a PBOD (pooled bunch of disks).

With RSA, a rack becomes the unit of scaling in a cloud environment. The management domain will aggregate multiple racks together in a pod. Looking specifically at the OCP implementation, within a rack the sub-unit of scaling is a tray. A tray consists of multiple nodes. The tray contains compute, memory, storage, and networking modules; a node is a compute module.

Diving a bit deeper on this idea, server CPU(s) will be connected to resource modules (memory, storage, networking) and will be managed by a tray manager. All this occurs within a tray. Between trays, Intel would look at using silicon photonics (or possibly a ToR switch); from there, the uplink goes out to the end-of-row (EoR) switch.

The resource modules (memory, storage, networking) are referred to as RSA pooled functions. A pooled memory controller would manage pooled memory. Pooled networking would be SDN-ready network connectivity. Pooled storage is the PBOD (Ethernet-connected JBOD, not using iSCSI but over straight Ethernet---are we talking ATA over Ethernet? Something else entirely?). The tray manager, mentioned earlier, ensures that resources are properly allocated and enforced.

Next, Kumar shifts his attention to pooled memory in particular. The key motivations for pooled memory include memory sharing, memory disaggregation, and right-sizing memory to workloads running on the node. If you were to also enable memory sharing---assign memory to two nodes at the same time---then you could enable new forms of innovation (think very fast VM migration or tightly-coupled OS clustering). It seems to me that memory sharing would require changes to operating systems and hypervisors in order for it to be supported, though.

Looking closer at how pooled memory works, it requires something called a pooled memory controller, which manages all the centrally pooled RAM. The pooled memory controller is responsible for managing memory partitions and allocation. (Would it be possible for this pooled memory controller to do "memory deduplication" too?) This is the piece that will enable shared memory partitions, but Kumar doesn't elaborate on changes required to today's OSes and hypervisors in order to support this functionality.

Kumar next shows a recorded demo of some of the RSA technologies in action.

At this point, Kumar shifts gears to discuss pooled storage in a bit more detail. The motivation for doing pooled storage is similar to the reasons for all of RSA---more flexibility in allocating resources, eliminating bottlenecks, and on-demand allocation.

Intel's RSA addressed pooled storage through a PBOD, which would be accessed over Ethernet using RDSP (Remote DAS Protocol). Individual compute nodes will use RDSP to communicate with the PBOD controller, which acts like the shared memory controller in that it handles partitioning storage and allocating storage to the individual compute nodes. Kumar tries to show a recorded demo of pooled storage but runs into a few technical issues.

At this point, Kumar provides a summary of the work that has been done toward RSA, reminds attendees that RSA technologies can be seen in the Technology Showcase, and opens the session up for questions and answers.
