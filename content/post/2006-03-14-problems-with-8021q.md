---
author: slowe
categories: Information
comments: false
date: 2006-03-14T09:24:16Z
slug: problems-with-8021q
tags:
- Apple
- Macintosh
- Networking
- Standards
- VLAN
title: Problems with 802.1Q
url: /2006/03/14/problems-with-8021q/
wordpress_id: 201
---

I stumbled across a [forum on Apple's web site](http://discussions.apple.com/thread.jspa?threadID=378673) this morning (referred to me in turn by [Derrick Story's request for MacBook Pro feedback](http://www.oreillynet.com/mac/blog/2006/03/what_are_your_macbook_pro_impr.html)) that makes me glad I'm not yet able to afford a [MacBook Pro](http://www.apple.com/macbookpro/).

According to the forum, there is a bug in the Ethernet driver in the Intel version of Mac OS X 10.4 that causes excessive packet loss on networks that employ 802.1Q. [802.1Q](http://en.wikipedia.org/wiki/802.1Q), in case you aren't already familiar with it, is an Ethernet networking standard used in networks that employ multiple VLANs. Apparently, the Ethernet driver on the Intel-based Macs is incorrectly processing packets as 802.1Q packets when they really aren't. This causes excessive packet loss and network connections that are pretty much unusable.

I guess I'll have to wait for the second generation of Intel-based Mac laptops. That's OK, as it gives me plenty of time to get my money's worth from my current [PowerBook G4](http://www.apple.com/powerbook/).
