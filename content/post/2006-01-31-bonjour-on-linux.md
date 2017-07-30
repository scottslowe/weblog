---
author: slowe
categories: Information
comments: false
date: 2006-01-31T19:05:40Z
slug: bonjour-on-linux
tags:
- Bonjour
- CentOS
- Linux
- Macintosh
- Networking
- OSS
- RedHat
- SSH
title: Bonjour on Linux
url: /2006/01/31/bonjour-on-linux/
wordpress_id: 169
---

A while back, I experimented with a multicast DNS (mDNS) responder for Linux. (For those not already "in the know," so to speak, multicast DNS is one of the key components of [Bonjour](http://www.apple.com/macosx/features/bonjour/), Apple's automatic service discovery functionality---formerly known as Rendezvous). For some strange reason, I had an urge to try it again today. Here's what I found.

First, I started looking for an official RPM package for a [CentOS](http://www.centos.org/) 4.2-based server that I manage. Despite numerous Google hits that implied an official RPM existed, I could not find one. (Pointers and/or URLs are welcome.) I finally found a few RPMs on one of the CentOS mirrors, and installed it without any major issues. The problem was, there was no documentation. It installed an executable file called `mdnsd`, along with a directory in `/usr/share/doc` and a matching init script. But how to configure it? How to tell it what services to advertise via mDNS?

Having no luck whatsoever finding any additional documentation, I turned to a POSIX-compliant mDNS responder I had downloaded from Apple's developer site and compiled on Red Hat Linux 9.0 some time ago. I also had a simple init script for it, which (if I recall correctly) had been created by Rui Carmo of [Tao of Mac](http://the.taoofmac.com/space) (great site, by the way---I recommend it). Fortunately for me, all I had to do was just copy the files over to the CentOS-based server and place the files in the right place, and it worked flawlessly.

Sure enough, I could now see this Linux-based server in Terminal.app's "Connect to Server" dialog box. I could not, however, see the server as an SFTP server in [Cyberduck](http://cyberduck.ch/). I briefly searched to see what kind of advertisements Cyberduck was expecting to see, but couldn't find any information. (Note, strangely enough, that Terminal.app could see the server as an SFTP server, but Cyberduck couldn't.)

Now don't ask me _why_ exactly I was driven to tinker with this today, because I couldn't tell you.

More information on multicast DNS, DNS Service Discovery, and related technologies can be found at the sites linked below:

DNS Service Discovery (DNS-SD) - [http://www.dns-sd.org/](http://www.dns-sd.org/)  
Multicast DNS - [http://www.multicastdns.org/](http://www.multicastdns.org/)
