---
author: slowe
categories: Rant
comments: true
date: 2009-07-27T15:52:23Z
slug: potential-ucs-issue-northbound-fcoe-connectivity
tags:
- Virtualization
- Cisco
- FCoE
- FibreChannel
- Hardware
- Networking
- UCS
title: 'Potential UCS Issue: Northbound FCoE Connectivity'
url: /2009/07/27/potential-ucs-issue-northbound-fcoe-connectivity/
wordpress_id: 1483
---

I'm about halfway through the first day of Unified Computing System (UCS) training in San Jose, CA, and I've learned of what I think is a fairly significant limitation. The issue centers around what Cisco refers to as "northbound" traffic and how Fibre Channel over Ethernet (FCoE) is handled with northbound traffic.

Recall that a central part of UCS is the UCS 6100 series fabric interconnect. The 6100 series fabric interconnect has connectivity in two directions:

* _Southbound_ connectivity is connectivity aimed back at the fabric extenders in the blade chassis themselves.

* _Northbound_ connectivity is connectivity headed outside the UCS to other systems and networks.

All southbound traffic is 10Gbps Ethernet with FCoE. Northbound traffic can be 10Gbps Ethernet or Fibre Channel, _but not FCoE._ Based on the information I've been given (and if I'm incorrect please let me know in the comments), you **cannot directly connect an FCoE-enabled storage array to a UCS.** Even if your storage array has native FCoE interfaces, you can't plug them into the UCS 6100 series fabric interconnects because that's considered northbound traffic and you can't use FCoE with northbound traffic.

I have a feeling customers who have purchased storage arrays with FCoE interfaces with the intention of hooking the arrays up directly to a UCS are going to be a bit upset when this information becomes more widely known.

If I'm working from incorrect or incomplete information, please feel free to speak up in the comments.
