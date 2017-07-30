---
author: slowe
categories: Explanation
comments: true
date: 2009-08-11T11:30:59Z
slug: why-no-multi-hop-fcoe
tags:
- Cisco
- FCoE
- Networking
- Nexus
- Storage
- VLAN
title: Why No Multi-Hop FCoE?
url: /2009/08/11/why-no-multi-hop-fcoe/
wordpress_id: 1513
---

Much ado has been made---some of it by yours truly---about the current lack of ability to create a multi-hop Fibre Channel over Ethernet (FCoE) fabric. After digging in deeper with Cisco during my recent Unified Computing System (UCS) class, I have some additional information to share about the different forms of multi-hop FCoE and _why_ multi-hop FCoE still isn't available.

Multi-hop FCoE falls into two basic scenarios:

* FCoE initiators and/or FCoE targets separated from an FCF (fibre channel forwarder) by multiple hops through IEEE DCB-compliant Ethernet switches

* Multiple FCFs chained together to connect FCoE initiators and FCoE targets

There are additional scenarios, but for now let's discuss just these two.

In the first scenario, FCoE initiators and FCoE targets might be separated from an FCF by one or more IEEE DCB-compliant Ethernet switches (also known as an "FCoE passthrough"). In this situation, FCoE Initialization Protocol (FIP) would be _required_ in order for the FCoE initiators and FCoE targets to communicate. Now that FIP support is beginning to emerge following the ratification of the FC-BB-5 standard in early June, this sort of scenario becomes more possible.

If you think like me (and if you do, I'm very sorry to hear that!), your next question is, "OK, what is an IEEE DCB-capable switch?" As it turns out, the Nexus 5000 can be an IEEE DCB-capable switch (or an FCoE passthrough). Cisco doesn't advertise that fact because they don't feel that building a solution out of a bunch of Nexus 5000 switches is the best approach. OK, fair enough, so the Nexus 5000 isn't really designed to be used that way. So what other options are there? None of which I'm aware, at this point, so that makes it impossible to build multi-hop FCoE solutions today. When a valid IEEE DCB-capable switch or FCoE forwarder _does_ appear then you'll be able to build these sorts of designs---assuming that you have FIP support in both the FCoE initiators and the targets. (Note that you could mix pre-FIP components in here, but all such components would have to be connected directly to the FCF, and would only be able to communicate with other components connected directly to the FCF.)

In the second scenario, FCoE initiators and targets are connected directly to an FCF---like a Nexus 5000--but you've got multiple FCFs chained together to create a larger fabric. You might consider this analogous to linking multiple MDS 9000 series switches together with inter-switch links (ISLs). In this case, FIP support would still be necessary for initiators to connect to targets on a different FCF, but now there's another wrinkle. You see, Cisco has the concept of a VSAN (think of it like a VLAN for Fibre Channel---this is a simplistic definition but reasonable enough to use). In the MDS world (keeping in mind that NX-OS, the software running on Nexus switches, has its roots in SAN-OS, the software that runs on MDS Fibre Channel switches), there is the concept of _trunking E\_ports_, where multiple VSANs are carried on a single E\_port between two MDS switches. Continuing the VSAN/VLAN analogy, a trunking E\_port is analogous to an 802.1Q VLAN trunk.

Bear with me, there's a reason I'm telling you all this.

When you use FCoE on a Nexus 5000, you end up mapping each VSAN to a VLAN. When you need to move from one FCF to another FCF---i.e., from one Nexus 5000 to another Nexus 5000--how should the VSAN information be presented? Should the VSAN information reside in the 802.1q VLAN tag, so that an 802.1q VLAN trunk is considered a trunking E\_port with regard to VSANs? Or should the VSAN information remain embedded in the FC commands that are encapsulated by Ethernet? This fundamental question has not yet been answered. There are advantages and disadvantages to each approach, and as the T.11 group responsible for FC-BB-5 and other FCoE standards hasn't yet come to agreement yet on how to handle this, then it's currently not possible to create the FCoE equivalent of trunking E\_ports (I believe these will be referred to as VE\_ports). Since you can't create VE_ports, you can't connect multiple FCFs together, and you can't build a multi-hop FCoE fabric composed of multiple FCFs.

As you can see, then, that even with FIP present in all components, neither definition of multi-hop FCoE is possible today. Although a Nexus 5000 _can_ function as an FCoE passthrough, Cisco doesn't recommend that architecture. Without any other IEEE DCB-capable Ethernet switches available to use as an FCoE passthrough, that makes the first scenario impossible to build. Likewise, the inability to create VE_ports and trunk VSANs across multiple Nexus 5000 switches means that it's impossible to build the second scenario today. While multi-hop FCoE is the ultimate goal, it's just not possible right now.

Here's some food for thought while you digest this information: how would a fabric extender change things? That's a topic I'll delve into in a future post, so stay tuned!

Of course, FCoE experts and wizards are encouraged to add your corrections, clarifications, and thoughts in the comments below.
