---
author: slowe
categories: Explanation
comments: false
date: 2005-11-28T16:11:38Z
slug: ipfw-rules-for-bonjour
tags:
- macOS
- Networking
- Security
title: ipfw Rules for Bonjour
url: /2005/11/28/ipfw-rules-for-bonjour/
wordpress_id: 124
---

I originally published this information on my business website, but thought I'd reproduce it here in the event someone needs it.

Apple's [Bonjour](http://www.apple.com/macosx/features/bonjour/) (formerly Rendezvous) auto-discovery service is used in a number of Apple-branded products as well as a growing number of third-party applications (for example, my HP Color LaserJet 2550N supports Bonjour). It's used to advertise and discover [iTunes](http://www.apple.com/itunes/) music libraries, and Apple's Airport Admin Utility also uses Bonjour to discover [Airport Extreme](http://www.apple.com/airportextreme/) and [Airport Express](http://www.apple.com/airportexpress/) wireless base stations for configuration.

Bonjour uses [multicast DNS](http://www.multicastdns.org/) (known as mDNS), part of the IETF standard known as [Zeroconf](http://www.zeroconf.org/).

All that is well and good, but if you are using the built-in firewall you most likely are preventing Bonjour from working correctly. I grew frustrated trying to utilize Bonjour-related services while also keeping my [PowerBook](http://www.apple.com/powerbook/) protected against unwanted network traffic. Using a packet sniffer and Brian Hill's [Brickhouse](http://personalpages.tds.net/~brian_hill/brickhouse.html) application, I observed Bonjour traffic and created a set of ipfw rules that can be used to allow this traffic.

Multicast DNS operates over UDP (IP protocol 17) with a destination port 5353 and a source port above 1024.  The destination address is, of course, a multicast address (in the 224.0.0.0 range). Responses to mDNS multicasts originate from UDP port 5353, but are bound for a random high port above 1024.  Simply defining a rule that allows traffic to and from UDP port 5353 won't work, because while outbound traffic will be correctly matched the responses to those outbound requests won't be matched and will be dropped (assuming the default action is to deny traffic). So, a sample rule to be added to ipfw might look something like this:

    add 2008 allow udp from 10.1.1.0/24 5353 to any 1024-65535 in via en0

(The line above should be entered all on a single line.)

This rule allows traffic from the source network 10.1.1.0 (the "/24" indicating a 24-bit mask, i.e., a subnet mask of 255.255.255.0) from UDP source port 5353 to any port above 1024 on any destination address inbound via the `en0` interface. The `en0` interface is typically the built-in Ethernet interface on most [Mac OS X](http://www.apple.com/macosx/)-based systems. (Likewise, `en1` is typically the built-in AirPort/AirPort Extreme wireless interface.)

If you are tightly restricting outbound traffic as well, you'll need a matching outbound rule (of course).

My own experience has shown that adding this rule to my ipfw ruleset has allowed Bonjour to work as expected without sacrificing the security of my system.
