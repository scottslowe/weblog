---
author: slowe
categories: Tutorial
comments: true
date: 2019-03-12T12:00:00Z
tags:
- Linux
- VPN
- Networking
- IPSec
title: Split Tunneling with vpnc
url: /2019/03/12/split-tunneling-with-vpnc/
---

`vpnc` is a fairly well-known VPN connectivity package available for most Linux distributions. Although [the `vpnc` web site][link-1] describes it as a client for the Cisco VPN Concentrator, it works with a wide variety of IPSec VPN solutions. I'm using it to connect to a Palo Alto Networks-based solution, for example. In this post, I'd like to share how to set up split tunneling for `vpnc`.<!--more-->

Split tunneling, as explained in [this Wikipedia article][link-2], allows remote users to access corporate resources over the VPN while still accessing non-corporate resources directly (as opposed to having _all_ traffic routed across the VPN connection). Among other things, split tunneling allows users to access things on their home LAN---like printers---while still having access to corporate resources. For users who work 100% remotely, this can make daily operations much easier.

`vpnc` does support split tunneling, but setting it up doesn't seem to be very well documented. I'm publishing this post in an effort to help spread infomation on how it can be done.

First, go ahead and create a configuration file for `vpnc`. For example, here's a fictional configuration file:

```
IPSec gateway vpn.company.com
IPSec ID VPNGroup
IPSec secret donttellanyone
Xauth username bobsmith
```

All this information, naturally, has to reflect the correct configuration for your particular VPN setup. This is all reasonably well-documented on various `vpnc` tutorials. If you stop here, you'll have a "regular" `vpnc` connection that will route all traffic across the VPN.

To do split tunneling, add this line at the end of your configuration file:

```
Script /etc/vpnc/custom-script
```

You can use whatever filename you want there (and put it wherever you want in the file system, although I prefer keeping it in `/etc/vpnc`). In the file you specified, add these contents:

``` bash
#!/usr/bin/env bash

# Set up split tunneling
CISCO_SPLIT_INC=1
CISCO_SPLIT_INC_0_ADDR=10.0.0.0
CISCO_SPLIT_INC_0_MASK=255.0.0.0
CISCO_SPLIT_INC_0_MASKLEN=8
CISCO_SPLIT_INC_0_PROTOCOL=0
CISCO_SPLIT_INC_0_SPORT=0
CISCO_SPLIT_INC_0_DPORT=0

# Call regular vpnc-script
. /etc/vpnc/vpnc-script
```

The `CISCO_SPLIT_INC` value specifies how many networks are going to be configured to route across the VPN. In this example, there is only a single network being routed across the VPN. That network is provided by the `CISCO_SPLIT_INC_0_ADDR`, `CISCO_SPLIT_INC_0_MASK`, and `CISCO_SPLIT_INC_0_MASKLEN` entries, and in this case equates to 10.0.0.0/8.

If you have multiple/non-contiguous networks, then specify how many networks on the `CISCO_SPLIT_INC` line, and then repeat the lines above for each network, incrementing the number for each section. For two non-contiguous networks, you'd have a series of `CISCO_SPLIT_INC_0_*` lines (for the first network) followed by a set of `CISCO_SPLIT_INC_1_*` lines (for the second network).

The last line is important---this ties back to the script that comes packaged with `vpnc` to set up all the routing and such, as modified/directed by the values specified in your custom script. This allows you to customize the behavior of split tunneling on a per-connection basis.

Once you have your custom script in place, you can connect using `sudo vpnc /etc/vpnc/config.conf` (as normal). Once the connection is up, you can use `ip route list` to see that only the specified networks are being routed across the VPN. All other traffic still uses your local gateway.

Note that this solution does _not_ address custom DNS resolver configurations. If you need to be able to resolve corporate hostnames _and_ a DNS domain on your home LAN, additional steps are needed. I'll try to document those soon (once I've had a chance to do some additional testing).

[Find me on Twitter][link-99] if you have questions, comments, suggestions, or corrections. Thanks!

**Update 4 Feb 2021:** For systems running `resolvectl` or the equivalent, I've found that adding `CISCO_SPLIT_DNS=domain1.com,domain2.com,domain3.com` to the custom script will configure the DNS search domains for that connection, which may help address situations where you need to resolve both local hostnames on your LAN as well as corporate hostnames.

[link-1]: https://www.unix-ag.uni-kl.de/%7Emassar/vpnc/
[link-2]: https://en.wikipedia.org/wiki/Split_tunneling
[link-99]: https://twitter.com/scott_lowe
