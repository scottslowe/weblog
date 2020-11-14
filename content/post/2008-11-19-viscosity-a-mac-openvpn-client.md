---
author: slowe
categories: Information
comments: true
date: 2008-11-19T19:51:48Z
slug: viscosity-a-mac-openvpn-client
tags:
- BSD
- IPSec
- Linux
- macOS
- Networking
- OSS
title: Viscosity, a Mac OpenVPN Client
url: /2008/11/19/viscosity-a-mac-openvpn-client/
wordpress_id: 1041
---

So, I've been searching for a good way to establish connectivity to the lab at my office for a while. My first attempt was to work with one of our CCIEs at the office to establish an IPSec-based VPN against a Cisco router at the edge of the lab network, but despite our best efforts we couldn't get the IPSec VPN client I was using, [IPSecuritas](http://www.lobotomo.com/products/IPSecuritas/), to connect and authenticate. No amount of fiddling would make it work.

We finally gave up on that and instead I went with an [OpenBSD](http://www.openbsd.org/) box to which I could establish an SSH session and then tunnel traffic from there. That worked reasonably well, especially after I discovered the [GNU Screen](http://www.gnu.org/software/screen/) utility. Talk about a handy little tool! Anyway, I continued using the SSH gateway for quite some time and I had resigned myself to living with the limitations.

Then a co-worker from the office casually mentions that he's set up a Linux-based [OpenVPN](http://openvpn.net/) server on another subnet in the lab (we have a range of different subnets for different engineers in the lab). He, too, is a Mac user, but still running Mac OS X 10.4 on an older 13" PowerBook G4 and using the [Tunnelblick](http://code.google.com/p/tunnelblick/) OpenVPN client. I thought to myself, "Hey, this might actually work!"

Alas, some additional research indicated that Tunnelblick had some stability problems under Leopard, which I'm running on my MacBook Pro. Bummer! I continued to research the issue but didn't bother trying to use the OpenVPN server until just a couple of weeks ago when I uncovered [Viscosity](http://www.viscosityvpn.com/index.html).

Viscosity is a shareware, Leopard-only OpenVPN client. It supports [Growl](http://growl.info/) notifications (which I very much like) and operates as a simple menu icon that easily allows you to connect or disconnect individual connections. Owing partially to how OpenVPN works, Viscosity uses (and includes) a TUN/TAP driver for OS X and creates a new TUN/TAP interface for every connection. This makes routing much easier and much more logical, in my opinion.

I'm so pleased with OpenVPN thus far, in fact, that I'm going to be setting up my own OpenVPN server here at the house.

My experience thus far has been quite positive. If you are looking for a good OpenVPN client for your Mac, Viscosity would be an excellent choice. At only $9 for a license, it's well worth it.
