---
author: slowe
categories: Rant
comments: false
date: 2005-08-03T17:45:28Z
slug: squid-pix-integration
tags:
- Interoperability
- Networking
- Cisco
title: Squid-PIX Integration
url: /2005/08/03/squid-pix-integration/
wordpress_id: 63
---

I have been searching for the last few days on some techniques to integrate a [Squid web cache](http://www.squid-cache.org/) with a PIX firewall in a transparent fashion. Most of the information I am finding involves using the Squid web cache as the default gateway along with an iptables firewall that transparently redirects outbound TCP port 80 traffic to port 3128 (the Squid web cache port). The web cache then talks to the PIX, which takes it from there. Certainly, this works, but it is not what I was hoping to find. I'd really like a way to have the PIX redirect the traffic, but it appears that the PIX OS does not support that functionality. How can this be? The pf firewall in [OpenBSD](http://www.openbsd.org) supports redirection, if I'm not mistaken. The iptables firewall in Linux supports redirection. But not Cisco's PIX OS? Is it just me, or does anyone else see a problem with this?
