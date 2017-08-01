---
author: slowe
categories: Explanation
comments: false
date: 2006-04-04T17:20:13Z
slug: modifying-dhcp-client-operation-on-openbsd
tags:
- UNIX
- BSD
- Networking
title: Modifying DHCP Client Operation on OpenBSD
url: /2006/04/04/modifying-dhcp-client-operation-on-openbsd/
wordpress_id: 221
---

One more quick lesson learned from the recent experiment getting [OpenBSD running on VMware ESX Server][1] involved modifying the default operation of OpenBSD's DHCP client, `dhclient`.

In this case, [OpenBSD](http://www.openbsd.org/) only needed to obtain the IP address and subnet mask from the DHCP server. Specifically, OpenBSD should not obtain the default gateway, as another NIC on a different subnet would be directing traffic to a separate Internet connection. I found that by editing the `/etc/dhclient.conf` file, it is possible to control the behavior of the DHCP client so that it only "listens" to certain configuration parameters passed down by the DHCP server.

For example, to have the DHCP client only pick up IP address and subnet mask, change the "request" line in `dhclient.conf` to look something like this:

    request subnet-mask, broadcast address;

The standard `dhclient.conf` "request" line looks something like this:

    request subnet-mask, broadcast-address, time-offset, routers, domain-name, domain-name-servers, host-name, lpr-servers, ntp-servers;

Obviously, this list can be trimmed to pick up only the items that are needed by the server. Another neat trick is using the "prepend" statement; this allows the local client to use a value configured locally, then use the values passed down by the DHCP server. Check the man page for `dhclient.conf` for more detailed information.

[1]: {{< relref "2006-04-03-openbsd-on-esx-server.md" >}}
