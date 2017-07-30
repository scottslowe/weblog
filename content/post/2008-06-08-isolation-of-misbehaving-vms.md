---
author: slowe
categories: Information
comments: true
date: 2008-06-08T14:25:59Z
slug: isolation-of-misbehaving-vms
tags:
- ESX
- Linux
- Solaris
- Virtualization
- VMware
- Xen
title: Isolation of Misbehaving VMs
url: /2008/06/08/isolation-of-misbehaving-vms/
wordpress_id: 725
---

I came across an interesting paper discussing how various virtualization environments protect well-behaved VMs from misbehaving VMs. The paper is available [here](http://people.clarkson.edu/~jnm/publications/isolationOfMisbehavingVMs.pdf).

In the tests described in the paper, researchers used virtual machines on Xen 3.0 (the open source hypervisor, not the commercial XenServer product, as far as I can tell), VMware Workstation 5.5, and "Open Solaris 10" (quotes mine). As pointed out in the paper, these three environments represent paravirtualization, full virtualization, and OS virtualization (or containers). I'm not sure if the researchers actually meant OpenSolaris; I suspect not since that's a very recent release. Instead, I believe they probably just meant Solaris 10. On Xen and VMware Workstation, both running under Linux, they used Linux-based VMs; on Solaris, they used additional instances of Solaris. Each VM or instance ran Apache 2 and was tested using physical clients to connect to the HTTP server in each VM.

The results are interesting; VMware showed the best protection of well-behaved VMs from a misbehaving VM, followed by Xen with Solaris Containers providing the least protection. The level of protection was tested using a memory consumption stress test, a CPU stress test, a disk I/O stress test, and a network I/O stress test. I'd encourage you to have a look at the full paper for all the details.

These results are very interesting, but I wonder how much the results would change if we were to use VMware's ESX server product line instead of one of the hosted products like VMware Workstation? As a product representative of "full virtualization" solutions, I'd be curious to know if the results seen with VMware Workstation were also seen with ESX.

In any case, the results are a validation of what we, as consultants, have been talking about: full virtualization provides the best isolation of well-behaved workloads from ill-behaved workloads, preventing a workload in one VM from affecting other workloads due to mishandling of CPU, RAM, disk, or network resources. As the researchers conclude in the paper:

>"...it is clear that VMware completely protects the well-behaved VMs under all stress tests. Its performance is sometimes substantially lower for the misbehaving VM, but in a commercial hosting environment this would be exactly the right tradeoff to make."
