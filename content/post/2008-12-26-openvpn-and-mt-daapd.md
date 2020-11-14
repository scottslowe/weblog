---
author: slowe
categories: Explanation
comments: true
date: 2008-12-26T18:00:48Z
slug: openvpn-and-mt-daapd
tags:
- Linux
- macOS
- Music
- OSS
title: OpenVPN and mt-daapd
url: /2008/12/26/openvpn-and-mt-daapd/
wordpress_id: 1112
---

I have a system at home with an "older" Linux distribution for mundane tasks like DHCP and content filtering on my broadband connection (no need for the kids to see something they shouldn't be seeing, if you know what I mean). I've thought frequently about rebuilding it with a newer distribution, perhaps Ubuntu, but---to be perfectly honest---I'm just too lazy. It generally just works, and overall doesn't require a great deal of care and feeding.

One of the various things this server does is run mt-daapd (now called [Firefly Media Server](http://www.fireflymediaserver.org/), I believe)---basically it's an iTunes server. I dump copies of the MP3's generated when I rip a CD onto a mount point on this server, and anyone in the house with iTunes can connect and listen to them. They can't copy them or sync them to their iPod, but they can listen to them. Since the kids and I share some similar tastes in Christian contemporary music, it works out well.

After being rather [impressed with the Viscosity OpenVPN client][1] and OpenVPN in general, I also setup OpenVPN on this Linux home server for those instances when I need to connect to my home network for some reason. I've only needed to use it a couple of times, but it's worked great thus far.

While setting up some older laptops for the kids (one of their Christmas presents this year), I ran into an instance where iTunes for Windows wouldn't connect to the shared music library on my Linux server. The problem seemed sporadic, and seemed to be somewhat limited to the Windows laptops I was setting up; I was still able to connect from my MacBook Pro. About the same time, one of my younger kids came up and told me that the Mac mini downstairs wouldn't connect to the shared music library, either. Hmmm, something was going on.

Restarting the mt-daapd daemon didn't change anything, nor did disabling the Windows Firewall on the laptops. Turning off the firewall on the Linux server didn't change anything, either. I started to dig in a bit deeper then, and after a short while realized that Bonjour---which is used by iTunes to discover shared music libraries on other systems---was somehow picking up the wrong IP address. But where was this address coming from?

It didn't take long after that to figure out that mDNSResponder on the Linux server was broadcasting the IP address of the server's `tun0` interface, which is used by OpenVPN. Because of various routing issues and limitations, this range of addresses isn't reachable by the home LAN; hence, failures to connect to the mt-daapd server.

The fix, in my case at least, was to modify the `/etc/init.d/mDNSResponder` script to add the "-i eth0" parameter to the command that started mDNSResponder. This forced mDNSResponder to broadcast only the IP address of eth0, the server's primary Ethernet interface. Two changes needed to be made to the file:

1. First, the "-i eth0" needs to be added to the line that defines the variable $OTHER_MDNSRD_OPTS.

2. Second, double quotes have to be added around the command that actually launches mDNSResponder using the `runuser` command. Otherwise, the parameter to mDNSResponder is interpreted as a parameter to `runuser` and causes an error.

Once I made these changes and restarted both mDNSResponder and mt-daapd, all the systems were able to connect to the shared music library without any further issues. Problem solved!

[1]: {{< relref "2008-11-19-viscosity-a-mac-openvpn-client.md" >}}
