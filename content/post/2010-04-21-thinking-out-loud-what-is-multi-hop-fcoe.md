---
author: slowe
categories: Musing
comments: true
date: 2010-04-21T09:09:43Z
slug: thinking-out-loud-what-is-multi-hop-fcoe
tags:
- Cisco
- FCoE
- Networking
- Storage
- Nexus
- ToL
title: 'Thinking Out Loud: What is multi-hop FCoE?'
url: /2010/04/21/thinking-out-loud-what-is-multi-hop-fcoe/
wordpress_id: 1888
---

It's been a while since I last posted a "Thinking Out Loud" post, but a Twitter discussion this morning prompted me to post this one. Last night, I posted a question to Brad Hedlund ([@bradhedlund](http://twitter.com/bradhedlund) on Twitter and a great data center networking resource) regarding the Nexus 2232PP fabric extender. My [tweet to Brad](http://twitter.com/scott_lowe/statuses/12555532959) was this: does the Nexus 2232PP make "multi-hop FCoE" possible via NIV?

(If you haven't already read my post on the [relationship between FCoE and NIV][1], go read it now as it provides a good background and context for this discussion.)

Brad responded, confirming my assessment and stating that FIP snooping wasn't necessary. I wasn't sure about the FIP snooping part but after seeing the response I now understand. In any event, a number of others starting questioning my use of "multi-hop FCoE" in conjunction with the 2232PP fabric extender, stating that it wasn't a hop because ti does not actually switch any traffic.

Strictly speaking, all of these individuals are absolutely correct; for more information, go read [this post on NIV][2]. In this case, the Nexus 5000 is the interface virtualization (IV)-capable bridge and the Nexus 2232PP is the interface virtualizer (IV). The IV doesn't switch any traffic; all switching occurs on the IV-capable bridge. Therefore, from a switching perspective, a Nexus 5000 and any or all associated fabric extenders are a single hop.

My response is this: what _is_ multi-hop, anyway? Is it the presence of multiple switches in a data path between an initiator and a target? Or is it the presence of multiple physical devices, chained together, between an initiator and a target? In the first definition, using a fabric extender isn't multi-hop; in the second definition, it is. More to the point, should a customer really care which definition is correct? Why is multi-hop FCoE important, anyway? I would say the answer to that question is scalability. Customers want to be able to scale the FCoE fabrics larger than they can today. Multi-hop FCoE---however you want to define it---makes that possible. So does it really matter how we get there? Besides which point, using the fabric extender approach means only a single point of management instead of multiple points of management. Isn't that better?

What do you think? Do your own "thinking out loud" in the comments below.

[1]: {{< relref "2010-03-21-how-niv-and-fcoe-play-together.md" >}}
[2]: {{< relref "2010-03-16-understanding-network-interface-virtualization.md" >}}
