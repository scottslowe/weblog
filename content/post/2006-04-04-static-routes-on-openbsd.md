---
author: slowe
categories: Explanation
comments: false
date: 2006-04-04T16:59:18Z
slug: static-routes-on-openbsd
tags:
- UNIX
- BSD
- Networking
title: Static Routes on OpenBSD
url: /2006/04/04/static-routes-on-openbsd/
wordpress_id: 220
---

Once [OpenBSD was running on ESX Server][1], there was a configuration issue that had to be addressed involving static routes. This is probably one of those times where the [OpenBSD](http://www.openbsd.org/) experts are saying, "This is so simple!", but I had to search a bit to find the answer. So, for future reference and for the reference of those of us who are not OpenBSD experts (yet), here's the solution.

There are actually two solutions. The first solution involves the use of the `hostname.if` file. OpenBSD maintains a separate `hostname.if` file for each interface, where the ".if" is replaced by the devicename. In a virtualized instance, this would typically be "le1" for the first NIC, "le2" for the second NIC, etc. Therefore, the first NIC would be configured using `hostname.le1`, and the second NIC would be configured using `hostname.le2`.

To add a static route to an interface, append a route command to the appropriate `hostname.if` file. Here's an example:

    inet 192.168.254.254 255.255.255.0 NONE
    !route add default 192.168.254.1

The key, of course, is the "!route add ..." statement. This is the piece that I needed.

You can also add similar statements to `/etc/rc.local`, which will do the same thing. Note, however, that these statements _cannot_ go into `/etc/rc.conf.local`.

[1]: {{< relref "2006-04-03-openbsd-on-esx-server.md" >}}
