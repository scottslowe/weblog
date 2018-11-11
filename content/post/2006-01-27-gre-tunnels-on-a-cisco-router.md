---
author: slowe
categories: Explanation
comments: true
date: 2006-01-27T10:35:33Z
slug: gre-tunnels-on-a-cisco-router
tags:
- Cisco
- Encryption
- IOS
- Networking
- VPN
title: GRE Tunnels on a Cisco Router
url: /2006/01/27/gre-tunnels-on-a-cisco-router/
wordpress_id: 167
---

One of my projects involved the configuration of GRE (Generic Routing Encapsulation) tunnels, encrypted by IPSec, between two locations. I was having some problems getting the tunnels to work properly, but now I've managed to resolve that problem, and the configuration is working well. Here's some additional information on the problem and how it was finally corrected.

This was my first project using GRE tunnels. I'd used IPSec tunnels many times, and on many different platforms, but this time around we needed an interface that could be tracked for HSRP (Hot Standby Router Protocol) purposes, and until recently Cisco didn't offer IPSec tunnel interfaces. (I just came across some documentation last night that indicated very recent releases of IOS offer this functionality.) So, the idea was to use GRE tunnels, track the GRE tunnels using HSRP for failover with another router, and encrypt the traffic using IPSec in transport mode.

The GRE tunnel configuration (scrubbed for sensitive data) looked something like this originally:

```text
interface Tunnel0
description GRE tunnel to other location
ip address 192.168.254.1 255.255.255.252
tunnel source FastEthernet0/0
tunnel destination 172.31.254.1
crypto map tunnel-ipsec-map
```

Of course, there was an appropriately configured interface at the other end of the tunnel as well. The tunnels came up, and appeared to work just fine, until we added the `keepalive` statement. (The `keepalive` statement is required for the tunnel to report an actual up/down status, necessary for HSRP interface tracking.) Then they went down and stayed down.

A `debug tunnel` statement showed that the keepalives were being sent, but none were being received. Thinking perhaps the IPSec configuration was incorrect, I removed the `crypto map` statement from the tunnel interface. It still didn't work.

After reviewing the configuration again, I began to suspect an MTU issue---the `show int tun0` output listed an MTU of 1514. I consulted with a Cisco expert (recently obtained his CCIE), and he confirmed that it was most likely an MTU issue. So I modified the configuration to look like this:

```text
interface Tunnel0
description GRE tunnel to other location
ip address 192.168.254.1 255.255.255.252
ip mtu 1400
keepalive
tunnel source FastEthernet0/0
tunnel destination 172.31.254.1
```

At that point, the tunnel finally came up and I was able to pass traffic through the tunnel. I re-added the `crypto map` statement to enforce encryption, and the tunnels promptly went back down again.

Once again, debug output saved the day. The output from a `debug crypto` statement was constantly reporting "packet too small". A search of the Cisco web site turned up a result (I can't find it now) that indicated a bug within IOS and suggested the addition of a `tunnel key` statement. So, I modified the configuration again:

```text
interface Tunnel0
description GRE tunnel to other location
ip address 192.168.254.1 255.255.255.252
ip mtu 1400
keepalive
tunnel source FastEthernet0/0
tunnel destination 172.31.254.1
tunnel key 12345
crypto map tunnel-ipsec-map
```

With this configuration, the IPSec/ISAKMP SAs were established and the tunnels came up, passing traffic as expected. The debug output showed no crypto errors, and keepalives were being sent and received. Success!
