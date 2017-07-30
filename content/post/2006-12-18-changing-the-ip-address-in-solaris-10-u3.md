---
author: slowe
categories: Explanation
comments: true
date: 2006-12-18T15:32:16Z
slug: changing-the-ip-address-in-solaris-10-u3
tags:
- Networking
- Solaris
- UNIX
title: Changing the IP Address in Solaris 10 U3
url: /2006/12/18/changing-the-ip-address-in-solaris-10-u3/
wordpress_id: 388
---

Changing the IP address of a system running Solaris (Solaris 10, specifically) is different than a lot of other operating systems out there. Really, all you have to do is just edit a few files and then take the interface down and back up again. However, there seems to be a "gotcha" with Solaris 10. (I don't know how far back this procedure goes---it is unclear to me if this is new to Solaris 10, or if it extends back to Solaris 8 or 9.)

Most of the sites out there I found indicated that you only needed to edit the `/etc/hosts` file (which is actually just a symlink to `/etc/inet/hosts`) and place the new IP address of the server in that file. Since I wasn't changing the hostname or default gateway, there was no need to edit `/etc/hostname.pcn0` (the hostname file for the only interface in the system), `/etc/nodename`, or `/etc/defaultrouter`. So I edited the `/etc/inet/hosts` file, rebooted the server, and expected to see the new IP address show up on the network.

It didn't work. A bit more research indicates that in Solaris 10, the operating system uses `/etc/inet/ipnodes` over `/etc/inet/hosts`. This is a bit odd since ipnodes is only supposed to be used for IPv6, and I know that I specifically disabled IPv6 in this installation. Some additional targeted searches I performed, however, showed that this was indeed the case _even if IPv6 is disabled._

Upon editing `/etc/inet/ipnodes` and rebooting the server, the IP address change took effect.

So, if you need to change the IP address of a server running Solaris 10, change the following files:

    /etc/inet/hosts
    /etc/inet/ipnodes

Upon a reboot, the server will now have the new IP address.

(By the way, Solaris 10 U3 runs perfectly under ESX Server.)
