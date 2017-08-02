---
author: slowe
categories: Information
comments: true
date: 2009-03-16T15:47:34Z
slug: more-on-cisco-ucs
tags:
- Cisco
- FCoE
- Hardware
- Networking
- Virtualization
- UCS
title: More on Cisco UCS
url: /2009/03/16/more-on-cisco-ucs/
wordpress_id: 1239
---

The entire IT world is abuzz with talk of Cisco's Unified Computing System (UCS). I pointed out a few of the [various UCS announcements][1] in this earlier post, and now I'd like to just expand a little bit upon the solution.

UCS essentially consists of 4 major components:

* The UCS 6100 Series Fabric Interconnect devices, running Cisco UCS Manager

* The UCS 2100 Series Fabric Extender, with up to 2 of them running in each chassis

* The UCS B-Series blades, either half-width (8 blades per chassis) or full-width (4 blades per chassis), and up to 40 chassis per system

* UCS network adapters supporting DCE/CEE/DCB and FCoE, apparently coming in three different flavors (efficiency/performance, compatibility, and virtualization)

This diagram shows an overview of UCS:

![cisco-ucs-components-overview.gif](/public/img/cisco-ucs-components-overview.gif)

With the exception of the UCS Manager software and the Converged Network Adapters (CNAs), everything else is pretty standard stuff:

* The UCS 6100 is essentially a Nexus 5000, but with the ability to run the UCS Manager software.

* The UCS 2100 is essentially the same as the Nexus 2000 Fabric Extender (FEX), but in a form factor that is intended to plug into the UCS chassis.

* The B-Series blades are industry standard blades running Intel Nehalem CPUs, standard hot-plug hard drives, and 10Gb CNAs.

The CNAs appear to be one area in which there may be some innovation. In particular, the virtualization-optimized CNA appears to extend some new functionality into the virtualization layer itself, although it's currently unclear exactly how, or how the virtualization layer will leverage that functionality. It sounds like SR-IOV to me, but others are indicating that it's an offshoot of Intel's VT-d technology. Speaking specifically with regard to VMware ESX/ESXi, I would guess that this will need to be combined with VMDirectPath, as it appears to replace the need for the vSwitch within the ESX/ESXi host. Personally, I'd rather not replace the vSwitch and instead allow the UCS 6100 and/or UCS Manager to manage Nexus 1000V VEMs on the ESX/ESXi hosts instead. This will truly bring unification without adding complexity.

The real wildcard here is UCS Manager. Although the Cisco webcast spoke frequently of the "open APIs" and "XML APIs" that other partners can leverage, but nothing substantial or significant was released regarding UCS Manager. Lots of questions have yet to be answered, but the one that really jumps out at me is this one:

_How will an organization need to organize their storage in order to take advantage of UCS?_

I'm guessing here that organizations will need to do boot-from-SAN in order to gain the true flexiblity and agility that UCS is supposed to provide. In that case, what Cisco is supplying is not that dramatically different from a multi-vendor solution that utilizes something like Scalent to provide automation. Of course, Cisco's solution is from a single vendor and is supposedly more integrated.

So, there are my initial thoughts. What about you?

[1]: {{< relref "2009-03-16-cisco-ucs-announcements.md" >}}
