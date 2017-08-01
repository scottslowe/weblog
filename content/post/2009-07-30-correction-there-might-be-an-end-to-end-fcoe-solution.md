---
author: slowe
categories: Explanation
comments: true
date: 2009-07-30T19:10:03Z
slug: correction-there-might-be-an-end-to-end-fcoe-solution
tags:
- Cisco
- FCoE
- FibreChannel
- Hardware
- Networking
- Storage
title: 'Correction: There Might be an End-to-End FCoE Solution'
url: /2009/07/30/correction-there-might-be-an-end-to-end-fcoe-solution/
wordpress_id: 1501
---

This post is a follow-up from my post yesterday titled ["No Such Thing as an End-to-End FCoE Solution"][1].

After publishing that post, I managed to get in touch with some very smart people who were willing to spend some time with me and educate me on the various intricacies involved here. In order to help you, my readers, understand the various pieces and parts, I'll need to first provide some definitions.

_Fibre Channel Forwarder (FCF):_ In its simplest form, this is another form for an FCoE switch. A Nexus 5000 would be an example of an FCF.

_Multi-hop FCoE:_ There are a couple of different definitions here. One definition would be having multiple FCFs connected together (i.e., a Nexus 5000 connected to another Nexus 5000). A second definition would be having multiple Layer 2 hops between an FCoE initiator or target and an FCF. Note that the switches handling those hops must be IEEE DCB capable.

_FCoE Initialization Protocol:_ FIP, as its more commonly known, was included in the FC-BB-5 FCoE standard that was finalized in early June.

OK, now that I have some definitions in place, I can discuss how it might be possible (eventually) to build an end-to-end FCoE solution.

**Disclaimer:** I don't claim to be an FCoE expert, I'm just trying to understand it better myself and help others understand it better. If I'm misrepresenting something, let me know---courteously and professionally---in the comments, or drop me an e-mail.

The question "Can I build an end-to-end FCoE solution?" has multiple answers:

* If you have only a single FCF and everything is plugged into that FCF, then you can build a pure FCoE solution today. The Nexus 5000 can function as the FCF, and both the CNAs and targets that are available will work. Obviously, this is not a very scalable solution.

* If you have multiple FCFs, or if you have multiple Layer 2 hops between initiators or targets and the FCFs, then you might or might not be able to build an end-to-end FCoE solution. In this scenario, FIP-enabled initiators and targets would be able to find and communicate with each other, but non-FIP-enabled initiators and targets would not (unless they were plugged into the same FCF). At this point I am unclear about connectivity between pre-FIP initiators or targets on the same IEEE DCB-capable Layer 2 switch (not an FCF); I suspect they would not be able to communicate.

All of these statements are applicable _until you bring UCS into the mix._ For UCS, my earlier statement stands: with UCS, you cannot have an end-to-end FCoE solution today. That will change at some point in the future, but no one has shared any information with me regarding just how far in the future that might be.

If you have a pure Nexus 5000 environment, with FCoE-capable storage and servers with CNAs, you'd probably be able to make it work. With FIP support in that environment, you'd definitely be able to make it work. When you add UCS, though, it becomes very different. I hope to be able to discuss that in greater detail in the near future.

So, my earlier statement wasn't entirely true; it is possible to build an end-to-end FCoE solution. Today, that solution would be very limited in size; once FIP support is baked into the initiators, the targets, and the FCFs, then the solution size will be able to scale.

As always, comments and clarifications are welcome!

[1]: {{< relref "2009-07-29-no-such-thing-as-an-end-to-end-fcoe-solution.md" >}}
