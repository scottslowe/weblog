---
author: slowe
categories: Rant
comments: true
date: 2011-05-16T23:05:20Z
slug: distance-vmotion-stretched-cluster
tags:
- Networking
- Storage
- Virtualization
- VMware
- vSphere
title: Distance vMotion = Stretched Cluster?
url: /2011/05/16/distance-vmotion-stretched-cluster/
wordpress_id: 2305
---

Apparently, some people within the virtualization and storage community took umbrage with one of my statements from [my article on stretched clusters and distance vMotion][1]. Specifically, this statement:

>Long-distance vMotion and stretched clusters are not the same thing.

I'll be the first to admit that I don't know everything, and I try (not always successfully) to be open to admitting where I'm wrong. So feel free to (constructively) tell me where I'm mistaken here. How can long-distance vMotion and stretched clusters be considered the same?

Yes, both of them require Layer 2 adjacency for VMs. Yes, both of them need an appropriately configured storage layer underneath them (the architecture of which is an entirely separate discussion, in my opinion---or is it?) so that storage is accessible from both sites. But the fact that they share common requirements doesn't make them the same. Related? Yes. The same? No.

We know that a cluster is not a vMotion boundary. In other words, you can use vMotion to move VMs between clusters. That means I can have two clusters---one in each site---and I can use distance vMotion to move VMs between them. Ergo, distance vMotion but **_not_** a stretched cluster.

Similarly, I could have a stretched cluster (a cluster of ESX/ESXi hosts with some hosts in one site and some hosts in another site) and not use distance vMotion, since a cluster does not automatically mean vMotion. (Thnik about a cluster with DRS disabled.) Ergo, a stretched cluster but **_not_** distance vMotion.

So what am I missing? Or am I just being too technical with the definitions? Constructive comments and feedback are welcome.

[1]: {{< relref "2011-05-12-stretched-clusters-distance-vmotion-and-l2-adjacency.md" >}}
