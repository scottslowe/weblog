---
author: slowe
categories: Rant
comments: true
date: 2009-07-29T12:34:39Z
slug: no-such-thing-as-an-end-to-end-fcoe-solution
tags:
- Cisco
- FCoE
- FibreChannel
- Storage
- UCS
title: No Such Thing as an End-to-End FCoE Solution
url: /2009/07/29/no-such-thing-as-an-end-to-end-fcoe-solution/
wordpress_id: 1495
---

**Update:** See [this follow-up post][1] for more information.

I [mentioned yesterday](http://twitter.com/scott_lowe/status/2897123000) on Twitter that I'd had something of a revelation with regard to Fibre Channel over Ethernet (FCoE). This is probably nothing new to the experienced storage intelligentsia, but I'm just a simple guy so this was a big deal. After a spirited discussion in the Cisco UCS class about how to best leverage "FCoE-capable" storage, I have come to this realization: _there is no such thing as an end-to-end FCoE solution._

If you're impatient and want the short story, here it is: Even if you have an FCoE-capable storage array and you have FCoE converged network adapters (CNAs), you still can't build an end-to-end FCoE solution. Why? Because you _must_ put a standard Fibre Channel switch into the mix in order to provide fabric services like zoning, etc., because equipment like the UCS 6100 fabric interconnects and the Nexus 5000 don't provide those services.

Here's the longer version. We were having a discussion in the Cisco UCS training class revisiting the northbound FCoE connectivity issue that I discussed [here][2]. It turns out that the UCS 6100 fabric interconnect runs in NPV (or end-host) mode, so you can't hook up any sort of storage target, FC or FCoE, directly to the UCS 6100 fabric interconnect. Even if you were to enable the UCS 6100 fabric interconnect to run in switch mode---something that's not possible today---you _still_ can't hook a storage target, FC or FCoE, to the fabric interconnect because the fabric interconnect doesn't provide any fabric services. Further, even if you were to leave the UCS 6100 fabric interconnect in NPV mode and add a Nexus 5000 switch to the mix, you can't hook the the UCS 6100 and the Nexus 5000 together because FCoE isn't multi-hop capable (yet). If I understand correctly, the FC-BB-5 standard includes FIP, which will address this limitation. However, according to the information I'm getting here---and I'm fully open to more information from others who are "in the know"--even that won't fully address the problem because neither the UCS 6100 nor the Nexus 5000 will offer fabric services. So, you will _still_ need a traditional Fibre Channel switch, like a Cisco MDS 9000 series, to provide fabric services.

The end result is that, today, it's impossible to build an end-to-end FCoE solution. You will still need a traditional Fibre Channel switch somewhere in the mix, either to connect the FCoE equipment together (for example, to link a UCS 6100 fabric interconnect to a Nexus 5000) and/or to provide fabric services.

&lt;aside&gt;Now, there seems to be some confusion within Cisco, as the UCS resources to which I've been speaking are confirming my conclusions, but others (consider [this tweet](http://twitter.com/bradhedlund/status/2911928501) by Brad Hedlund) are saying it's not true. I don't know who's correct---I can only go on what I'm being given.&lt;/aside&gt;

As a result, it seems completely futile and useless for storage vendors to offer FCoE support on their storage arrays until these issues are addressed. In my mind, this further cements FCoE as an "edge-only" solution. Adding fabric services to the Nexus 5000 and/or UCS 6100 fabric interconnects would address this problem, and perhaps that's something that is now enabled and made possible via the FC-BB-5 standard and FIP. If so, I have yet to hear a timeline in which these limitations will be addressed.

Either way, if you're thinking of deploying FCoE today, be sure to keep this in mind or you could find yourself in for a surprise.

Courteous comments and clarifications are welcome!

[1]: {{< relref "2009-07-30-correction-there-might-be-an-end-to-end-fcoe-solution.md" >}}
[2]: {{< relref "2009-07-27-potential-ucs-issue-northbound-fcoe-connectivity.md" >}}
