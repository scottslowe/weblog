---
author: slowe
categories: Explanation
comments: false
date: 2006-02-19T17:32:25Z
slug: details-on-transparent-rdp-tunneling
tags:
- BSD
- Encryption
- Networking
- Security
- SSL
- Windows
- Interoperability
title: Details on Transparent RDP Tunneling
url: /2006/02/19/details-on-transparent-rdp-tunneling/
wordpress_id: 185
---

Quite a long time ago, I posted two short articles on transparent RDP tunneling (read more [here][1] and [here][2]). To be honest, I had forgotten that I hadn't posted more complete details on how exactly I went about making it work. So, to rectify that problem, here are the full details.

First, some background. I have a number of customers whose servers I manage remotely via Remote Desktop. Remote Desktop (or Terminal Services, if running in Application Server mode), as you may be aware, uses Microsoft's RDP as the protocol. Not comfortable using RDP across the Internet, I always encrypted RDP using SSL (typically via [Stunnel](http://stunnel.mirt.net/index.html)), but this proved unwieldy as the number of servers increased. I needed a way that I could use any ordinary RDP client from within my office and transparently tunnel that RDP traffic inside SSL.

&lt;aside&gt;The reason this became unwieldy is due to the number of client-side definitions I had to create on my [Mac OS X](http://www.apple.com/macosx/) laptop using SSL Enabler. After a while, it become difficult to remember which local port corresponded with each remote server.&lt;/aside&gt;

So, using [OpenBSD](http://www.openbsd.org/) (then version 3.7, now version 3.8), I first added some additional IP addresses to the le1 interface by modifying the `/etc/hostname.le1` file like so:

    inet 192.168.100.1 255.255.0.0
    alias 192.168.100.2
    alias 192.168.100.3

Using `ping`, I verified that the new IP addresses were responding, then proceeded to configure Stunnel to accept unencrypted connections and forward them to another host as encrypted connections. The Stunnel configuration looked something like this:

    client = yes
    [ms-wbt-server]
    accept  = 192.168.100.2:3389
    connect = 172.16.100.100:54321

I also had to add "ms-wbt-server" to the `/etc/services` file with the appropriate port numbers (3389).

On the other end of the tunnel, Stunnel was set up in reverse---it was configured to receive an encrypted connection on port 54321 (for example) and forward that as an unencrypted connection to the standard RDP port (3389). The Stunnel configuration looked something like this:

    CApath = c:\winnt\system32\stunnel
    cert = c:\winnt\system32\stunnel\stunnel.pem
    client = no
    service = SSLTunnel
    [ms-wbt-server-s]
    accept = 54321
    connect = 3389

Again, the "ms-wbt-server-s" (for "secure") had to be added to to the services file (on Windows boxes typically located in `C:\winnt\system32\drivers\etc`). Then I registered Stunnel to run as a service (I believe the command-line was `stunnel -s <config file name>` or similar). Upon starting the service, I verified that we now had a listening port using `netstat -an`.

When all looked good, I configured any firewalls to pass the appropriate traffic and tested the connection. Done! I was now able to connect to one of the internal IP addresses on the OpenBSD server using a standard, unencrypted RDP connection. That connection was then encrypted in SSL and forwarded across the Internet to a waiting Stunnel instance, where it was decrypted and handed off to the RDP listener.

With a few modifications, this approach could easily be used to create [application-specific VPNs][3] between multiple locations within the same organization, or between different organizations.

[1]: {{< relref "2005-06-29-transparent-rdp-tunneling.md" >}}
[2]: {{< relref "2005-07-01-transparent-rdp-tunneling-part-2.md" >}}
[3]: {{< relref "2005-12-13-application-specific-vpns.md" >}}
