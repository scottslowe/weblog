---
author: slowe
categories: Information
comments: false
date: 2005-11-02T19:09:00Z
slug: small-openbsd-38-speed-bump
tags:
- BSD
- UNIX
- Networking
title: Small OpenBSD 3.8 Speed Bump
url: /2005/11/02/small-openbsd-38-speed-bump/
wordpress_id: 110
---

My attempts to deploy the latest version of [OpenBSD](http://www.openbsd.org/), version 3.8 (released yesterday), have run into what I hope is only a small speed bump.

In previous versions of OpenBSD (I first started using OpenBSD only a couple of versions ago, with the release of OpenBSD 3.6), the AMD PCnet adapter that [VMware](http://www.vmware.com/) presents to virtual machines was detected using the `le` driver. So, the virtual network adapter in a VM running OpenBSD 3.6/3.7 (and earlier versions, presumably) would be `le1`. With the release of OpenBSD 3.8, the driver has changed to `pcn` (making the virtual network adapter `pcn0`), and this doesn't seem to work---at all. Several attempts last night failed miserably.

Fortunately, there is hope on the horizon. A [Google](http://www.google.com/) search turned up an [archived Neohapsis discussion](http://archives.neohapsis.com/archives/openbsd/2005-08/1314.html) that raises the possibility of disabling the `pcn` driver and using `le` instead. I hope to try that in the next couple of days to see how it works, and I'll post the results here.
