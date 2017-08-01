---
author: slowe
categories: Education
comments: false
date: 2006-07-07T15:10:45Z
slug: setting-dns-and-wins-server-addresses-remotely
tags:
- Microsoft
- Networking
- Windows
title: Setting DNS and WINS Server Addresses Remotely
url: /2006/07/07/setting-dns-and-wins-server-addresses-remotely/
wordpress_id: 294
---

As a follow-up to yesterday's posting about using [WMIC to set the DNS suffix search order remotely][1], here's additional information on using WMIC to remotely set the DNS server and WINS server addresses. This technique can be particularly useful for servers, where the IP addresses are statically assigned (as opposed to configured via DHCP).

**WARNING:** There are some limitations to this technique, so don't unleash it on your network without first taking the time to understand what it is doing and what the effects will be. Don't come crying to me if you mass configure all your servers and your network breaks---you've been warned.

The trick to this technique is understanding that WMIC has to work on a specific NIC instance in order to make this change. The DNS suffix search order, on the other hand, does not need to be applied to a specific NIC instance.

This command assumes that all IP-enabled, statically assigned interfaces should have their DNS and/or WINS server addresses changed. In this case, we're leveraging the power of WMIC's SQL-like `where` clause to limit our activity to only specific NIC instances.

Here's the command to set the WINS server addresses:

    wmic nicconfig where (IPEnabled=TRUE and DHCPEnabled=FALSE) 
    call SetWINSServer "192.168.254.100","192.168.254.101"

To perform this command remotely, add the `/node:<PC name>` switch (this goes before `nicconfig`). To specify alternate user credentials, add `/user:<Username>` and `/password:<Password>` switches (again, before nicconfig). Note the `where` clause that limits the action to only those NICs that are IP-enabled and not using DHCP.

Here's the command to set the DNS server addresses:

    wmic nicconfig where (IPEnabled=TRUE and DHCPEnabled=FALSE) 
    call SetDNSServerSearchOrder ("192.168.254.101","192.168.254.100")

The syntax is slightly different here for DNS servers than above for WINS server, so don't get tripped up. Same things apply again for working against a remote node or for specifying alternate user credentials.

Refer back to the entry on [setting the DNS suffix search order][1] for techniques on how to provide WMIC with a list of computers to act against (either embedding in a `for /f` command or using the `/node:@filename` trick). Using either of these techniques would let you apply these changes to a number of computers all at once.

**WARNING:** It should go without saying that setting either or both of these configuration values can, if done incorrectly, have a negative impact on the operation of your network. Specifically, it may not work _at all._ Exercise caution and make sure you know exactly which computers are going to be affected by this command. For this reason I would recommend against using the output of a Dsquery command with this WMIC command.

There are some important things to note about this command:

* First, the command works against _all_ IP-enabled, statically assigned NICs on the computer. If the computer has multiple NICs (i.e., is multihomed), this command will affect all of them---at least, all of them that match the criteria. This may not be the desired effect.

* Second, both the primary and secondary WINS servers have to be specified. I haven't figured out how to set only the primary WINS server or only the secondary WINS server yet.

With those limitations in mind, I hope that someone finds this information helpful. I don't think I can describe how many times I've had to go back and reconfigure servers with new DNS IP addresses or new WINS IP addresses, and when the number of servers gets up there that process gets real tedious real fast.

[1]: {{< relref "2006-07-06-remotely-setting-the-dns-suffix-search-order.md" >}}
