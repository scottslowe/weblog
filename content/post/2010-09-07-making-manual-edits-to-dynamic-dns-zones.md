---
author: slowe
categories: Tutorial
comments: true
date: 2010-09-07T10:00:00Z
slug: making-manual-edits-to-dynamic-dns-zones
tags:
- BSD
- Networking
- UNIX
title: Making Manual Edits to Dynamic DNS Zones
url: /2010/09/07/making-manual-edits-to-dynamic-dns-zones/
wordpress_id: 2091
---

This is one of those posts that is as much for my own benefit as it is for others. For a few weeks now, I've been working on a dynamic DNS setup for my home/home office network involving BIND and the ISC DHCP daemon running on a pair of OpenBSD virtual machines. I finally got it to work (thanks in no small part to [this article](http://www.bsdguides.org/guides/openbsd/networking/dynamic_dns_dhcp.php) and [this how-to post](http://www.ops.ietf.org/dns/dynupd/secure-ddns-howto.html)) and then found that I needed to make some manual edits to the DNS zones.

After a great deal of stumbling and fumbling, I found an obscure reference to a need to use `rndc` when making manual edits. After some testing, I learned that the "correct" way to make manual edits is as follows:

1. Halt changes to the dynamic DNS zone with the command `rndc freeze <zone name>`.

2. Make the manual edits to the zone file, being sure to increment the zone serial number.

3. Use the command `named-checkzone <zone name> <zone file>` to verify the syntax in the zone file.

4. Allow changes to the dynamic DNS zone with the command `rndc thaw <zone name>`.

If you monitor the appropriate log files (on my system I had to monitor `/var/log/daemon`), you'll see zone transfers take place to any secondary name servers, a strong indicator that the change has successfully been accepted and propagated.

A very simple task, I know, but hopefully this post will help me next time I need to do this same task again and hopefully it will help someone else out there in the same situation.
