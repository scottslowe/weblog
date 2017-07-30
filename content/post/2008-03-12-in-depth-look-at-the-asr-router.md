---
author: slowe
categories: Information
comments: true
date: 2008-03-12T19:36:27Z
slug: in-depth-look-at-the-asr-router
tags:
- Cisco
- IOS
- Linux
- Networking
- Virtualization
title: In-Depth Look at the ASR Router
url: /2008/03/12/in-depth-look-at-the-asr-router/
wordpress_id: 658
---

Back on March 5 blogger extraordinaire Alessandro Perilli of virtualization.info revealed that [Cisco had chosen KVM](http://www.virtualization.info/2008/03/cisco-puts-kvm-in-its-ios.html) as the virtualization platform for IOS-XE, the new Linux-based version of IOS that runs on the [recently introduced ASR series of routers](http://newsroom.cisco.com/dlls/2008/prod_030408.html).

If you were like me, you may have been wondering exactly _how_ Cisco was putting KVM to use. No need to wonder any longer! Colleague and fellow blogger Colin McNamara has written up a [detailed and in-depth discussion of the ASR1000](http://www.colinmcnamara.com/2008/03/10/cisco-is-using-linux-virtualization-and-40-core-cpus-for-its-next-generation-routers) and how it uses KVM to provide virtualized instances of IOS-XE. Colin also discusses the role of the QuantumFlow processor and, believe it or not, the role of Popeye and Spinach. (Go read the article. It will make sense when you're done.) Nice work, Colin!
