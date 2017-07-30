---
author: slowe
categories: Musing
comments: true
date: 2012-10-18T09:00:35Z
slug: thinking-out-loud-ovs-and-the-enterprise
tags:
- Linux
- Networking
- OSS
- OVS
- RedHat
- ToL
title: 'Thinking Out Loud: OVS and the Enterprise'
url: /2012/10/18/thinking-out-loud-ovs-and-the-enterprise/
wordpress_id: 2889
---

Over the last few days, I've been unsuccessfully trying to get Open vSwitch (OVS) running on Red Hat Enterprise Linux (RHEL) 6.3 (as well as CentOS 6.3). The rationale behind this is that RHEL/CentOS are the sort of battle-tested, well-supported Linux distributions that enterprises will (and do) deploy in their data centers. OVS, of course, is central to the strategy and functionality of several network virtualization players and projects:

* Nicira

* Midokura (more information [here](http://searchnetworking.techtarget.com/news/2240166952/Midokura-network-virtualization-Layer-2-7-services-OpenStack))

* OpenStack Quantum

If you [follow me on Twitter](http://twitter.com/scott_lowe), you've probably noticed me talking about trying to rebuild source RPMs, running into all sorts of dependency issues, encountering errors when compiling, patching files, etc. I still haven't achieved success (yet), but I'm going to keep trying---for a little while longer, at least.

And this is what brings me to the central thought in this Thinking Out Loud post. I'm no expert---especially in this area, where I'm still learning more and more every day---but shouldn't projects like OVS _just work_ with enterprise-class Linux distributions? I mean, if you're going to build network virtualization products that attempt to bring cloud computing-style functionality to enterprise networks, don't you kind of need a core part of your solution to work on the Linux distributions that enterprises will actually deploy? Yes, it's great that OVS ships with Fedora 17, and that it works (so I've been told, I haven't actually tried it personally). But what organization is going to deploy Fedora 17 to help support an important function like network virtualization? **They won't.** Doesn't it make sense for the developers behind projects like OVS to really pay attention to the distributions that are most likely to be used by their current (and potential) customers?

It really boils down to this: if OVS is going to see adoption in the enterprise, wouldn't it make sense to improve its support on enterprise-focused Linux distributions?

I'm new to a lot of this stuff, so I freely admit I that I could be just missing some key piece of information here. So help me out---what I am missing? What am I overlooking? Speak up below---all courteous comments are welcome.
