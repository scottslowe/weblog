---
author: slowe
categories: Explanation
comments: true
date: 2011-04-20T09:00:00Z
slug: connecting-cisco-catalyst-2960-s-to-nexus-5010
tags:
- Cisco
- Networking
- Nexus
- CLI
title: Connecting Cisco Catalyst 2960-S to Nexus 5010
url: /2011/04/20/connecting-cisco-catalyst-2960-s-to-nexus-5010/
wordpress_id: 2277
---

As part of the all-star team that is currently heads down in preparation for some cool stuff that will be at EMC World 2011 in just a couple of weeks, today I needed to connect a Cisco Catalyst 2960-S to a Nexus 5010 over a 10GbE connection. Simple enough, right? You would think so, but as it turns out there was a bit more involved than I suspected.

And it was simple, too---configure each side as a VLAN trunk, make sure the port on each side is enabled (not administratively down), and plug in a Cisco-branded TwinAx cable. All set! Well...except for the fact that only certain "versions" of the TwinAx SFP+ cables are supported with the 2960-S (see [this page](http://www.cisco.com/en/US/docs/interfaces_modules/transceiver_modules/compatibility/matrix/OL_6974.html)). Fortunately, someone at Cisco was smart enough to include the version number in the part number on the SFPs on the end of the TwinAx cables, so it only took a few minutes to find the right version of the cable. Pull the old cable, put in the new cable.

Wait, it's still not working. As it turns out, the port on the 2960-S was put into an "error-disabled" state because of the unsupported transceiver on the pre-v2 TwinAx cable (I believe the command I used to find this was `show interfaces error-disabled`). Fortunately, that's a quick fix: simply use the `shutdown` command to put the port into an administratively down state, then use the `no shut` command to bring it back up again. It's at that point that the switch checks the transceiver again and realizes that it's supported.

Key takeaways?

* There are different versions of TwinAx cables (and their associated transceivers). Who knew? (OK, no snarky comments out there.)

* Certain switches only support certain versions of these TwinAx cables.

* The only way (or is it?) to get a port out of err-disable state is to use `shutdown` and then `no shutdown`.

Every day is a learning experience. That's what makes life fun!
