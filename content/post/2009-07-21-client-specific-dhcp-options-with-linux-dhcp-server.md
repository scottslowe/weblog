---
author: slowe
categories: Explanation
comments: true
date: 2009-07-21T11:10:31Z
slug: client-specific-dhcp-options-with-linux-dhcp-server
tags:
- Linux
- Networking
title: Client-Specific DHCP Options with Linux DHCP Server
url: /2009/07/21/client-specific-dhcp-options-with-linux-dhcp-server/
wordpress_id: 1471
---

Last week, I had a need to present a different set of DHCP options to one specific DHCP client (my iPhone) on my home network. Being the geek that I am, I have a small server set up here at the house running Ubuntu Linux. (You can read about the latest evolution of my home network in [this article][1].) Now, I knew that this was possible using the Windows DHCP server, but I'd never done it with the Linux DHCP server. So, in case you find yourself in a similar situation, here's how it works.

The Linux DHCP server configuration file (typically `dhcpd.conf`) is broken into different blocks. For example, the "main" portion of the configuration file might look something like this:

	subnet 192.168.128.0 netmask 255.255.255.0 {  
	option routers 192.168.128.1;  
	range dynamic-bootp 192.168.128.50 192.168.128.150;:  
	option domain-name-servers 208.67.222.222, 208.67.220.220; }

If you want to set up a reservation---so that a particular DHCP client always gets the same IP address---you set up additional blocks, like this:

	host <hostname> {  
		hardware ethernet 00:11:22:33:44:55;  
		fixed-address 192.168.128.200; }

As it turns out, if you want to specify a different set of DHCP options to a client with a reservation (for example, in my situation I wanted to specify a different set of DNS servers), you just add a declaration to the client-specific section:

	host <hostname> {  
		hardware ethernet 00:11:22:33:44:55;  
		fixed-address 192.168.128.200;  
		option domain-name-servers 192.168.128.10; }

Of course, now that I know this it seems incredibly obvious. At the time that I was trying to figure this out, though, I wasn't sure exactly what the syntax would look like. So, next time you find yourself needing to change the options on a DHCP reservation on Linux, you'll know what to do!

[1]: {{< relref "2009-01-02-ubuntu-and-mac-os-x-integration.md" >}}
