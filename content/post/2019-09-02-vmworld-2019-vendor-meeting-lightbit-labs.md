---
author: slowe
categories: Information
comments: true
date: 2019-09-02T08:00:00Z
tags:
- Storage
- Networking
- Kubernetes
- vSphere
- VMware
- VMworld2019
title: "VMworld 2019 Vendor Meeting: Lightbits Labs"
url: /2019/09/02/vmworld-2019-vendor-meeting-lightbits-labs/
---

Last week at [VMworld][link-2], I had the opportunity to meet with [Lightbits Labs][link-1], a relatively new startup working on what they called "disaggregated storage." As it turns out, their product is actually quite interesting, and has relevance not only in "traditional" VMware vSphere environments but also in environments more focused on cloud-native technologies like [Kubernetes][link-3].<!--more-->

So what is "disaggregated storage"? It's one of the first questions I asked the Lightbits team. The basic premise behind Lightbits' solution is that by taking the storage out of nodes---by decoupling storage from compute and memory---they can provide more efficient scaling. Frankly, it's the same basic premise behind storage area network (SANs), although I think Lightbits wants to distance themselves from that terminology.

Instead of Fibre Channel, Fibre Channel over Ethernet (FCoE), or iSCSI, Lightbits uses NVMe over TCP. This provides good performance over 25, 50, or 100Gbps links with low latency (typically less than 300 microseconds). Disks appear "local" to the node, which allows for some interesting concepts when used in conjunction with hyperconverged platforms (more on that in a moment).

Lightbits has their own operating system, LightOS, which runs on industry-standard x64 servers from Dell, HP, Lenovo, etc. To further enhance the performance of a server running LightOS (a "brick" in their terminology), Lightbits also offers a hardware acceleration card called the LightField. This card offers a number of benefits:

* 100Gbps compression/decompression at wire speed
* NVMe/TCP acceleration
* Global FTL (Flash Translation Layer) acceleration
* Encryption
* Deduplication

Lightbits is currently working on certifying their NVMe/TCP implementation for vSphere, at which point vSphere users would be able to  use Lightbits bricks as storage for vSphere hypervisors. The Lightbits folks also discussed the idea of using Lightbits with vSAN in what was called "disaggregated hyperconverged storage." (How's that for a marketing term!) In this arrangement, the disks used by vSAN would actually be volumes from a Lightbits brick (not local disks). It solves the storage portion of the basic problem behind hyperconverged platforms, which is the inability to independently scale compute, memory, and storage. I must admit, however, that it took a bit of head-scratching to understand what immediately seems to be a counter-intuitive arrangement.

With regard to Kubernetes, Lightbits offers a CSI plugin, which allows Kubernetes clusters to directly access and provision Persistent Volumes (PVs) on a Lightbits brick. The Lightbits team stated that NVMe/TCP support is already present in the upstream Linux kernel, although I haven't verified this and I don't have any information as to what version of the kernel added this support.

More information is available from [the Lightbits Labs web site][link-1].

Feel free to [hit me up on Twitter][link-99] if you have any questions or comments.

[link-1]: https://www.lightbitslabs.com/
[link-2]: https://www.vmworld.com/
[link-3]: https://kubernetes.io/
[link-99]: https://twitter.com/scott_lowe
