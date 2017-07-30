---
author: slowe
categories: Musing
comments: true
date: 2013-04-03T09:21:01Z
slug: thinking-out-loud-targeting-the-real-problem
tags:
- Networking
- OpenFlow
- SDN
- Virtualization
- ToL
title: 'Thinking Out Loud: Targeting the Real Problem'
url: /2013/04/03/thinking-out-loud-targeting-the-real-problem/
wordpress_id: 3125
---

Two (relatively) independent thought streams collided late last week, and this post is the result.

The first thought stream was about the benefits and drawbacks of OpenFlow. As I've said on many occasions, _every_ technology decision has benefits and drawbacks. OpenFlow is no exception, in my opinion. Clearly there are some real benefits to using OpenFlow in certain use cases (here's [one example](http://packetpushers.net/openflow-1-0-actual-use-case-rtbh-of-ddos-traffic-while-keeping-the-target-online/)), but that doesn't mean OpenFlow---especially hop-by-hop OpenFlow, where OpenFlow is involved at every "hop" of the packet forwarding process throughout the network---is the right solution for all environments.

The collision occurred after I read these two blog posts ([here](http://blog.ioshints.info/2013/03/what-did-you-do-to-get-rid-of-manual.html) and [here](http://blog.ioshints.info/2013/03/where-is-my-vlan-provisioning.html)) by Ivan Pepelnjak on manual VLAN provisioning.

Now, you might be thinking "Scott, what do manual VLAN provisioning and hop-by-hop OpenFlow have to do with each other?" That is a valid question, and it was this phrase by Ivan that connected the two (for me, at least):

>I love listening to the Packet Pushers (the best networking podcast there is) on my way to work, and I know what to expect in every SDN-focused episode: an "I'm sick and tired of using [the] CLI to manually provision VLANs" rant.

Based on what I've seen and read so far, it seems like a lot of folks see software-defined networking (SDN), especially hop-by-hop OpenFlow, as the solution to manual VLAN provisioning. After all, if I can use a controller---there are numerous open source and proprietary controllers out there---to gain programmatic access to controlling individual flows within my data center, why do I need VLANs? I can provide a greater level of automation (via an API provided by the controller) **and** eliminate the need to use VLANs (because traffic can be separated at the flow level). Win-win, right?

As I thought about this more, though, it seemed to me that perhaps this line of thinking is targeting the wrong problem. OpenFlow, as I understand it, is targeted at modifying/controlling how packet forwarding occurs across the network. However, it seems to me that this isn't the real problem. The real problem is the configuration and management of network policy: stuff like QoS, VLANs, ACLs, NAT, VRFs, firewalls, load balancing, etc. If we could automate _that_ stuff---perhaps even using OpenFlow in some capacity---then the packet forwarding would, in turn, be properly shaped/controlled/influenced by that policy.

Or am I missing something? I'd love to hear what you think, so feel free to add your thoughts in the comments below. Please be sure to add vendor affiliations, where applicable. All courteous comments are welcome!
