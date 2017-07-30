---
author: slowe
categories: Information
comments: true
date: 2016-06-21T00:00:00Z
tags:
- Docker
- OSS
- Linux
- Networking
- Security
- Storage
- DockerCon2016
title: DockerCon 2016 Vendor Meetings
url: /2016/06/21/dockercon-2016-vendor-meetings/
---

While at DockerCon 2016 in Seattle today, I took some time on the expo floor to talk to a number of different vendors, mostly focused on networking solutions. Here are some notes from these discussions. I may follow up with additional posts on some of these technologies; it will largely depend on time and the ease by which the technologies/products may be consumed.

## Plumgrid

My first stop was the [Plumgrid][link-1] booth. I'd heard of Plumgrid, but wanted to take this time to better understand their architecture. As it turns out, their architecture is quite interesting. Plumgrid is one of the primary commercial sponsors behind [the IO Visor project][link-6], a Linux Foundation project, which leverages the extended Berkeley Packet Filter (eBPF) subsystem in the Linux kernel. Using eBPF, Plumgrid has created in-kernel virtual network functions (VNFs) that do things like bridging, routing, network address translation (NAT), and firewalling.  Combined with a scale-out central control plane and leveraging the Linux kernel's built-in support for VXLAN, this enables Plumgrid to create overlay networks and apply very granular security policies to attached workloads (which could be VMs or containers).

## Project Calico

Next, I stopped by the [Calico][link-2] booth. Unlike many of the networking solutions at DockerCon, Calico does _not_ employ overlay networks. Instead, the solution leverages fully-routable network topologies and dynamic routing protocols. Containers are assigned a routable IP address, which is then propagated via BGP (Calico leverages [BIRD][link-7]). Route aggregation is leveraged wherever possible, and Calico programs IPTables on each host to handle security policies. I didn't get the opportunity to inquire about what sort of control plane (if any) Calico uses.

Calico is also set to undergo some significant changes; Calico is set to be merged with Flannel (from CoreOS) to create a new project called Canal. It's early days yet, so it's hard to say how things will shape up over time.

## StorageOS

I guess I can't fully escape my time spent in the world of storage; I'd also been asked to stop by the booth for [StorageOS][link-3], a new "software-defined storage" startup. StorageOS aims squarely at providing persistent storage for Docker containers, primarily focusing on databases in Docker containers. The StorageOS folks were careful to point out that they aren't a distributed storage solution (like ScaleIO or VSAN); aside from read-only replicas, writes can occur only on the node where the back-end storage for a volume is located. They offer both synchronous and asynchronous replication (within and between clusters, respectively), and an option to "re-home" storage if the workload using that storage gets re-scheduled on a different node. Pricing is capacity-based.

## Midonet

I also popped over to talk to [Midokura][link-4] about MidoNet. MidoNet is an open source, overlay-based network virtualization solution. MidoNet uses the Open vSwitch (OVS) kernel module that is part of the upstream Linux kernel, but does _not_ leverage the userspace portion, and doesn't employ any sort of traditional centralized control plane. Instead, MidoNet uses a "distributed controller" model similar to that used by Open Virtual Network (OVN)---though, obviously, this architecture predates OVN as it was used by MidoNet some numbers of years prior to OVN's creation. Each "local controller" performs the necessary calculations/simulations to create the programming necessary to bring out the desired state (create overlay tunnels, apply security policies, etc.).

## JFrog

Finally, I stopped in at the [JFrog][link-5] booth. JFrog was a company I'd heard about a number of times---in particular, I'd heard about Artifactory---but otherwise didn't know a whole lot about them. Artifactory is just one of their solutions; they also offer Bintray (for distributing content, like compiled binaries), Mission Control (for managing repositories), and Xray (for evaluating components used in a project). Xray is quite interesting in that they soon expect to unveil extra support for Docker images that will allow you to "follow the breadcrumbs" from a particular Docker image all the way back to the particular commits in its source repository. This is something that can be accomplished now via the use of Docker labels, but Xray's approach is more automatic.

## Summary

I suspect I have a lot more to learn about the application deployment cycle/pipeline before I can truly appreciate what JFrog has to offer, but it certainly seems pretty useful. StorageOS is (very) young yet, but is worth watching to see how it develops. As for the networking solutions---Plumgrid, Calico, and MidoNet---each of them has both strengths and weaknesses. I like Plumgrid's use of eBPF to create in-kernel VNFs, MidoNet's distributed control plane, and Calico's use of well-known building blocks like BGP. I'm a bit partial to overlays and therefore lean a bit more toward Plumgrid/MidoNet, but that's just me.



[link-1]: http://www.plumgrid.com/
[link-2]: https://www.projectcalico.org/
[link-3]: http://storageos.com/
[link-4]: http://www.midokura.com/
[link-5]: https://www.jfrog.com/
[link-6]: https://www.iovisor.org/
[link-7]: http://bird.network.cz/