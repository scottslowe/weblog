---
author: slowe
categories: News
comments: true
date: 2012-02-06T11:47:18Z
slug: recent-cisco-product-launch
tags:
- Cisco
- Networking
- Nexus
title: Recent Cisco Product Launch
url: /2012/02/06/recent-cisco-product-launch/
wordpress_id: 2530
---

Cisco recently launched a number of new products and new versions of products aimed at showing Cisco's dedication and innovation in network switching. You can get Cisco's summary of the products [here](http://www.cisco.com/go/switchinginnovations). I'll start with the hardware-focused announcements.

## Hardware-Focused Announcements

Cisco's announcements on the hardware side primarily centered around new switching capabilities in the form of 40/100 Gigabit Ethernet (GE) cards for both the Nexus 7000 series platform as well as a 40 GE card for the Catalyst 6500 series. (The 40 GE card for the Catalyst 6500 series does require the newer Supervisor Engine 2T, however.)

&lt;aside&gt;Did you know that the 40/100 GE specification, [802.3ba](http://grouper.ieee.org/groups/802/3/ba/index.html), was ratified in June 2010? I didn't realize it has been ratified that long.&lt;/aside&gt;

For the Nexus 7000 series, Cisco unveiled two cards:

* The M2-Series 6-port 40 Gigabit Ethernet Module with XL Option (how's that for a mouthful?) sports, as the name suggests, 6 non-blocking 40 GE ports. With 16 of these blades in the 18-slot Nexus 7018, that provides up to 96 ports of 40 GE connectivity. The "XL Option" in the name of the card enables the card---in conjunction with the Scalable Feature License---to support more IPv4 routes (up to 1 million, depending on several factors), more IPv6 routes (up to 350,000, again depending on several factors), and more access control list (ACL) entries. These increased route and ACL limits could be useful in environments with multiple Virtual Routing and Forwarding (VRF) or multiple Virtual Device Context (VDC) instances. Based on the [Cisco data sheet](http://www.cisco.com/en/US/prod/collateral/switches/ps9441/ps9402/data_sheet_c78-695858.html), it looks like this card can work with either Fabric-1 or Fabric-2 modules, although you'll need Fabric-2 modules for the most throughput.

* The M2-Series 2-port 100 Gigabit Ethernet Module with XL Option provides two non-blocking 100 GE ports. The "XL Option" functions in the same way as with the 6-port 40 GE card, and it does work with both Fabric-1 and Fabric-2 modules. Here's the official [Cisco data sheet](http://www.cisco.com/en/US/prod/collateral/switches/ps9441/ps9402/data_sheet_c78-695859.html).

Based on the data sheet, it looks like both of these cards will require version 6.1 of NX-OS, which---to my knowledge---is a brand-new release. (Version 6.0 of NX-OS for the Nexus 7000 series was released in late December.)

One interesting note about the 40 GE card is that it supports the use of a breakout cable that allows a single 40 GE port to support four 10 GE connections. This is true for both the Nexus 7000 and Catalyst 6500 cards, as far as I can tell. (Cisco references a FourX connector for the Catalyst 6500 card, but does not reference the same connector for the Nexus blade, instead simply mentioning a "breakout cable".)

Cisco also introduced the Catalyst 4500-X, a fixed configuration 10 GE aggregation switch. They mentioned "40 GE readiness," but it's not clear when 40 GE uplinks will make their way to this particular platform.

Rounding out the hardware announcements was the Nexus 3064-X, a follow-up the low-latency Nexus 3000 series of switches introduced some time ago. This version offers lower power consumption and additional reductions in switching latency. The specific switching latency reductions were not specified anywhere that I found.

## Software-Focused Announcements

The software-focused announcements were primarily centered around the Nexus 1000V/1010 and a new feature called Easy Virtual Network (EVN).

* A new version of the Nexus 1000V was announced that supports VXLAN (Virtual Extensible LAN). I've discussed VXLAN extensively (see [here][1]), as have others in the industry, so I won't rehash that again.

* Also of note is that the Cisco Virtual Security Gateway (VSG) will offer zone-based firewalling services to VMs on VXLAN segments.

* The Nexus 1010-X is a "beefed up" version of the Nexus 1010 (yes, I know this is _technically_ a hardware product), intended to support more virtual networks. See [here for more information](http://www.cisco.com/en/US/prod/collateral/switches/ps9441/ps9902/ps10785/qa_c67-578700.html) on the differences between the Nexus 1010 and the Nexus 1010-X.

* Easy Virtual Network (EVN) is the most interesting of the software-based announcements (to me, at least). Cisco touts EVN as "fully compatible with established standards, including Multiprotocol Labl Switching (MPLS), MPLS VPN over IP (multipoint generic routing encapsulation, also known as mGRE), Multi-Virtual Route Forwarding (also known as VRF-Lite), and others." (More information available [here](http://www.cisco.com/en/US/prod/collateral/iosswrel/ps6537/ps6557/ps6604/whitepaper_c11-638769.html).) However, it appears that EVN doesn't actually use any of these mechanisms. In fact, it's unclear to me exactly what mechanism EVN _does_ use. The data sheet (linked above) mentions the use of a VNET tag, and indicates that the VNET tag is stored in the 802.1q VLAN ID field. This looks like Cisco is creating _yet another_ Layer 3 VPN solution, instead of leveraging existing solutions. Why not add VXLAN support to the hardware switches instead of creating EVN? Maybe I'm completely missing the mark herefeel free to correct me (courteously, of course!).

## Why This Matters to You

All these product announcements and new product versions are pretty cool, but you might be wondering, "Why does this matter to me?" Good question. Here are my thoughts:

* The 40 GE and 100 GE cards are important in data centers where we are seeing increased deployment of 10 GE to the servers. Once motherboard manufacturers start using 10 GE LoM (LAN on Motherboard) ports, the deployment of 10 GE to servers in data centers will naturally increase. Deploying 40 GE and 100 GE uplinks in 10 GE-heavy environments makes sense (depending on the details, naturally).

* You already knew the VXLAN-capable 1000V was on the way (this was alluded to in the original VXLAN announcement at VMworld 2011), so no real surprises there.

* The Nexus 1010-X simply allows you to run more VSBs (virtual service blades), such as the Nexus 1000V VSM (virtual supervisor module). If you're a Nexus 1000V customer, you might have already started investigating the use of the Nexus 1010 to host the VSMs. Large customers (service providers, perhaps?) had a need for more VSBs on a single Nexus 1010, hence the Nexus 1010-X.

* EVNwell, I'm stuck on EVN. I certainly see the need for simpler separation of traffic, but again I must ask why Cisco appears to be creating something new instead of re-using protocols that are perfectly suited for this purpose? Maybe I need a networking expert to explain it to me. (When I understand it, I'll post an explanation that everyone else can understand. Fair?)

As always, feel free to post any clarifications or corrections in the comments below. I'd love to hear from any networking gurus on any of the points that I raised in this article.

[1]: {{< relref "2011-12-02-examining-vxlan.md" >}}
