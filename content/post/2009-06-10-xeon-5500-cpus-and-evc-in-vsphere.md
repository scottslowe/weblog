---
author: slowe
categories: Explanation
comments: true
date: 2009-06-10T10:43:04Z
slug: xeon-5500-cpus-and-evc-in-vsphere
tags:
- Hardware
- HP
- Virtualization
- VMotion
- VMware
- vSphere
title: Xeon 5500 CPUs and EVC in vSphere
url: /2009/06/10/xeon-5500-cpus-and-evc-in-vsphere/
wordpress_id: 1392
---

In early April when I traveled to Houston to meet with HP about the ProLiant G6 servers---a trip which resulted in [this blog post][1]---I got into a discussion with representatives from HP and Intel regarding the Xeon 5500 CPUs and Enhanced VMotion Compatibility (EVC).

As you probably already know, EVC builds upon hardware extensions provided by AMD and Intel (AMD in the AMD-V extensions, Intel in FlexMigration) that allows a CPU to "alter" its personality or identification so as to allow live migration (i.e., VMotion) between CPUs that would otherwise be incompatible. Without these hardware extensions, you would have had to resort to unsupported CPU masking, which I've also discussed (here's a [summary of articles][2] I've written on the topic). With the hardware extensions and EVC, the hypervisor can now take over the process of creating custom CPU masks that take all CPUs in the cluster down to the "lowest common denominator" (i.e., EVC masks off all features except those that are common to all CPUs in the cluster).

Ed Haletky provides a good [overview of VMware EVC](http://www.itworld.com/virtualization/56292/understanding-vmware-evc), in case you need more information.

Anyway, that's enough background information. When I was in Houston, HP and Intel were discussing all the great new features that were available in the Xeon 5500. Knowing that introducing new CPU features generally meant introducing VMotion boundaries (like I [describe here][3]), I asked this question: "What happens to all these new features when you put a Xeon 5500-based server into a cluster with older servers and use EVC to guarantee VMotion compatibility?" My thought was that EVC would _downgrade_ the Xeon 5500 by masking new features, thus negating many of the performance benefits offered by the Xeon 5500.

Obviously, the architectural changes introduced by the Xeon 5500 (specifically, Quick Path Interconnect) wouldn't be affected, but what about Extended Page Tables (EPT)? Or VMDq? Or FlexPriority?

There were folks there from HP and Intel, and they were all stumped. They had to call in a few experts, but finally we got an answer from an Intel engineer on the VMware Alliance Team who was able to provide more information:

>... The only features that would be disabled are some new instructions that are accessible to end-user applications living in Ring 3. For NHM-EP those would be the SSE4.2 and SSE4.1 instructions... No other features would be compromised. All the features of NHM-EP that provide performance and are under the direct control of the OS or hypervisor in Ring 0 will be fully usable: EPT, HT, NuMA, FlexPriority, VMDq, VT-x, VT-d, etc.

So there you have it: you can mix Xeon 5500-based servers in with older servers running previous generations of Xeon CPUs without sacrificing the performance benefits gained from the Xeon 5500.

[1]: {{< relref "2009-04-07-hps-proliant-g6-servers.md" >}}
[2]: {{< relref "2009-06-10-articles-on-cpu-masking-in-vmware-environments.md" >}}
[3]: {{< relref "2007-03-27-new-vmotion-boundary.md" >}}
