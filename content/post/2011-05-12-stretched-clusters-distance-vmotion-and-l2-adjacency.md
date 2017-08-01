---
author: slowe
categories: Explanation
comments: true
date: 2011-05-12T12:25:24Z
slug: stretched-clusters-distance-vmotion-and-l2-adjacency
tags:
- Networking
- EMC
- Storage
- Virtualization
- VMotion
- VMware
- VPLEX
title: Stretched Clusters, Distance vMotion and L2 Adjacency
url: /2011/05/12/stretched-clusters-distance-vmotion-and-l2-adjacency/
wordpress_id: 2295
---

Beth Pariseau recently published an article discussing [the practical value of long-distance vMotion](http://itknowledgeexchange.techtarget.com/server-virtualization/long-distance-vmotion-inches-toward-realitywho-will-use-it/), partially in response to EMC's announcement of VPLEX Geo at EMC World 2011. In that article, Beth quotes some text from a tweet I posted as well as some text from Chad Sakac's recent [post on VPLEX Geo](http://virtualgeek.typepad.com/virtual_geek/2011/05/vplex-geo-awesomesauce.html). However, there are a couple inaccuracies from Beth's article that I really feel need to be cleared up:

1. Long-distance vMotion and stretched clusters are not the same thing.

2. L2 adjacency for virtual machines is not the same as L2 adjacency for the vMotion interfaces.

Regarding point #1, in her article, Beth implies that Chad's statement "Stretched vSphere clusters over [long] distances are, as of right now, still not supported" is a statement that long-distance vMotion is not supported. Long-distance vMotion, over distances with latencies of less than 5 ms round trip time (RTT), is fully supported. What's not supported is a stretched cluster, which is not a prerequisite for long-distance vMotion (as I pointed out in [the stretched clusters presentation][1] Beth also referenced). If you want to do long-distance vMotion, you don't need to set up a stretched cluster, so statements of support for stretched clusters cannot be applied as statements of support for long-distance vMotion. Let's not confuse the two, as they are separate and distinct.

Regarding point #2, the L2 adjacency for virtual machines (VMs) is **absolutely** necessary for distance vMotion. As I explained [here][2], it is possible to use a Layer 3 protocol to handle the actual VMkernel (vMotion) traffic, but the VMs themselves still require Layer 2 adjacency. If you don't maintain a single Layer 2 domain for the VMs, then VMs would have to change their IP addresses on a live migration. That's REALLY BAD and it completely breaks live migration. Once again, there is a very separate and distinct behavior that you're trying to modify with large L2 domains.

Am I off? Speak your mind in the comments below.

[1]: {{< relref "2011-05-10-stretched-clustering-presentation.md" >}}
[2]: {{< relref "2010-08-19-vmotion-layer-2-adjacency-requirement.md" >}}
