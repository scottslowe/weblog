---
author: slowe
categories: Explanation
comments: true
date: 2006-09-11T16:11:45Z
slug: esx-firewall-oddity
tags:
- ESX
- Networking
- Security
- Virtualization
- VMware
title: ESX Firewall Oddity
url: /2006/09/11/esx-firewall-oddity/
wordpress_id: 333
---

Since I've been performing quite a bit of Kerberos-based testing recently (and since Kerberos is very sensitive to clock skew), I wanted to make sure that my ESX-based hosts were keeping accurate time. Since ESX ships with NTP already installed, I planned on just enabling the NTP traffic outbound, configuring the daemon, and going on my merry way. No big deal, right?

To make a long story short (I'll spare you all the intricate details), what I discovered was that even though the "NTP Client" service was showing as enabled in VirtualCenter, it was being reported as blocked by `esxcfg-firewall`, the command-line tool for manipulating the ESX Server firewall (this was with the command `esxcfg-firewall -q ntpClient`). To add to the confusion, NTP worked on some hosts but did not work on other hosts even though the firewall configuration and NTP configuration was the same on all the hosts.

A quick search on the Internet turned up [this VMTN Forums thread](http://www.vmware.com/community/click.jspa?searchID=-1&messageID=423937), which suggested to restart the management agent (using the command `service mgmt-vmware restart`). I did this, waited for VirtualCenter to shake itself back out again, and noted that NTP had disappeared from the list of enabled services in the firewall configuration in VirtualCenter. At least I had VirtualCenter and `esxcfg-firewall` in sync now.

I enabled NTP client traffic in VirtualCenter again, but returning to the ESX host I again found that `esxcfg-firewall` was still reporting NTP traffic as blocked. Even after allowing almost ten minutes for the change from VirtualCenter to be applied back to ESX, `esxcfg-firewall` still reported NTP as blocked. On that one host, I restarted the VMware management agent again, and again NTP disappeared from VirtualCenter's list of services allowed through the firewall.

So, to avoid any other problems, I enabled NTP with the following command:

    esxcfg-firewall -e ntpClient

The `esxcfg-firewall -q ntpClient` command now listed the service as enabled, but what did VirtualCenter say? Even after a reboot and a disconnect/reconnect sequence, VirtualCenter _still_ did not report NTP client traffic as enabled.

What do I take from this? Perhaps it's a bit harsh, but _don't trust VirtualCenter_---at least, not with regards to firewall settings. If you want to know what's going on, then drop the service console and find out from the source. Just repeat after me: "The command line is my friend. The command line is my friend. The command line is my friend..."
