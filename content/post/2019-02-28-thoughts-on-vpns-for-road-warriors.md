---
author: slowe
categories: Information
comments: true
date: 2019-02-28T12:00:00Z
tags:
- VPN
- Networking
- Travel
- Security
title: Thoughts on VPNs for Road Warriors
url: /2019/02/28/thoughts-on-vpns-for-road-warriors/
---

A few days ago I was talking with a few folks on [Twitter][link-1] and the topic of using VPNs while traveling came up. For those that travel regularly, using a VPN to bypass traffic restrictions is not uncommon. Prompted by my former manager [Martin Casado][link-4], I thought I might share a few thoughts on VPN options for road warriors. This is by no means a comprehensive list, but hopefully something I share here will be helpful.<!--more-->

There were a few things I wanted to share with readers:

* I found commercial VPN services too unreliable (it's not uncommon for commercial VPN services to get blocked and thus defeat the purpose of using a VPN).
* Instead, I've found more success using something like [AutoVPN][link-2]. AutoVPN helps you stand up an on-demand OpenVPN endpoint on AWS. I used this successfully in Beijing, setting up endpoints in Seoul, Singapore, Tokyo, and sometimes Sydney. Because these IP addresses are "ephemeral," they're far less likely to be blocked. (Here's [another example][link-3] of using AWS as a personal VPN service.)
* I also had success using AutoVPN not to get around traffic restrictions, but to change my source IP. My wife needed to access some real estate-related sites while she was traveling in Europe, but because her IP address was outside the US the login process was failing. I had her use AutoVPN with an instance in us-east-1 from her Mac. It worked very well!
* If you have a static IP address, you can host your own VPN endpoint (Martin Casado shared that this was something he did for years when he had a static IP address).
* If you don't have a static IP address but still want to host your own VPN endpoints, one potential workaround is to use a dynamic DNS service like DynDNS. I did this for several years, and it ran flawlessly.
* _Be sure to appropriately secure your VPN server!_ Use two-factor authentication, use a secure OS (I built mine on [OpenBSD][link-7]), keep it patched/updated, etc. The last thing you want is folks routing traffic through your VPN endpoint without your knowledge/approval.
* OpenVPN seems to be the VPN of choice, probably due to flexibility of encapsulation (UDP or TCP). It's also well-supported on the client side, with your choice of clients on macOS ([Tunnelblick][link-5] and [Viscosity][link-6] spring to mind) and Linux (both command-line and GUI options are available).

Hopefully something I've shared here will prove useful to readers. If anyone has any other suggestions they'd like to mention, [hit me on Twitter][link-99]. Thanks for reading!

[link-1]: https://twitter.com/
[link-2]: https://github.com/ttlequals0/autovpn
[link-3]: https://zugdud.io/index.php/2017/10/08/using-aws-as-a-personal-vpn/
[link-4]: https://twitter.com/martin_casado/
[link-5]: https://tunnelblick.net/
[link-6]: https://www.sparklabs.com/viscosity/
[link-7]: https://www.openbsd.org/
[link-99]: https://twitter.com/scott_lowe/
