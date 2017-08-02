---
author: slowe
categories: Explanation
comments: true
date: 2011-12-22T09:00:00Z
slug: otv-and-vxlan-layer-3-connectivity-compared
tags:
- Networking
- OTV
- Virtualization
- VXLAN
title: OTV and VXLAN Layer 3 Connectivity Compared
url: /2011/12/22/otv-and-vxlan-layer-3-connectivity-compared/
wordpress_id: 2506
---

Building large-scale L2 networks, including stretched L2 networks, seems to be all the rage these days, driven in part by virtual machine mobility (aka vMotion in VMware vSphere environments or XenMotion in Citrix XenServer environments). While this isn't always a good idea---some might say it's [never a good idea](http://blog.ioshints.info/2011/12/large-scale-l2-dci-true-story.html)--it is still something that many organizations are evaluating.

With the announcement of VXLAN at VMworld 2011, a new question seems to have arisen: can I use VXLAN instead of (insert some other protocol here) to create my stretched L2 networks? In this post, I'd like to compare the use of VXLAN with OTV (Overlay Transport Virtualization) for that very purpose. Of course, since VXLAN hasn't actually been released, the discussion is partially theoretical.

My primary focus in this post will be how each of these protocols handles traffic patterns in the course of addressing the need for L2 connectivity over routed L3 networks.

First, let's look at VXLAN. The figure below is taken from my [revised L3 connectivity with VXLAN post][1], which I encourage you to read for more details.

![](/public/img/fixed-l3-postmig.png)

As you can see, once a VM inside a VXLAN segment is migrated to a new network, the traffic "trombones" back and forth across the VXLAN segment because all traffic has to pass through a single vShield Edge (VSE) instance. This brings up a key limitation of VXLAN that I think is important to point out: VXLAN has an innate dependency on VSE, and **VSE cannot be made redundant.** That's right---you can't have VSE-specific failover functionality; instead, you have to rely on vSphere HA, VM Monitoring, and other features. That means failover times in the minutes, not seconds. What do you think that will do to network connections?

Now, let's compare VXLAN's L3 connectivity with OTV. First, here's a diagram to show connectivity with OTV before a VM is migrated to the second site:

![](/public/img/otv-l3-premig.png)

No real surprises here. I'll just point out here that a typical OTV deployment following "recommended practices" will use redundant Nexus 7000 switches, as shown here. That's a key advantage that OTV has over VXLAN---the ability to provide redundancy is there and redundancy is easily built into the solution, with failover times in the seconds (or better).

Now, take a look at the post-migration traffic flows with OTV:

![](/public/img/otv-l3-postmig.png)

In case you didn't notice it, let me point out the obvious: **note the lack of traffic tromboning here.** Here's how it's accomplished (and documented in [this blog post](http://ccie5851.blogspot.com/2011/03/otv-deep-dive-part-3.html) by Ron Fuller, aka [@ccie5851](http://twitter.com/ccie5851) or VDCBadger to his friends):

* Each Nexus 7000 pair runs HSRP.

* The HSRP hello packets are filtered (blocked) from the OTV interfaces. This keeps the HSRP pairs in each data center from knowing about the pair in the other data center.

* Each HSRP pair runs the same virtual IP (the default gateway for the 10.1.1.0/24 subnet).

In this configuration, once the VM migrates to the second site the HSRP pair at the second site won't need to send traffic across the OTV link to reach the migrated VM. This appears to be a significant advantage to OTV---a greater knowledge of the routing topology allows OTV to be more intelligent about how traffic should be directed across/around the network.

&lt;aside&gt;Of course, this doesn't address L3 routing concerns from subnets not directly attached to the Nexus 7000 pairs. For that, we'd need something like LISP.&lt;/aside&gt;

As I see it---and networking experts are welcome to jump in if I'm mistaken---this gives OTV two key advantages over VXLAN:

1. OTV, because it is running on physical networking equipment, is more intelligent than VXLAN about how traffic is directed/routed in/around/across a network. This can result in more efficient utilization of a data center interconnect as a result of reduced "traffic tromboning."

2. OTV, because it is running on physical networking equipment, can provide better redundancy and faster failover than VXLAN (which relies on single instances of VSE).

It's entirely possible that if VXLAN ever makes it into physical network equipment that these advantages of OTV will be nullified.

It's also important to point out that while OTV and VXLAN have some overlap in functionality they are partially targeted at solving different problems. While both protocols address L2 connectivity across L3 networks, VXLAN also addresses the exhaustion of the VLAN address space in larger networks (especially service provider networks). This is an issue that OTV does not try to address. However, it seems to me that OTV would co-exist better with a solution like Q-in-Q, which could (as far as I can tell) address the VLAN ID exhaustion issue.

Once again, I encourage network experts to chime in and share their views. If I've misstated something, please let me know. Questions, thoughts, and comments are always welcome.

[1]: {{< relref "2011-12-07-revisiting-vxlan-and-layer-3-connectivity.md" >}}
