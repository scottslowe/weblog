---
author: slowe
categories: Information
comments: true
date: 2011-07-25T09:00:00Z
slug: some-useful-unix-commands-on-your-mac
tags:
- CLI
- macOS
- Networking
- UNIX
title: Some Useful UNIX Commands on your Mac
url: /2011/07/25/some-useful-unix-commands-on-your-mac/
wordpress_id: 2358
---

Over the last day or so I've been messing around at the UNIX command line on my Mac, trying to find a workaround for a VPN policy that doesn't allow split tunneling. (Just as a stupid side question, what is the security issue with split tunneling, anyway?) Along the way, I uncovered some handy commands for gathering information about the networking configuration of your Mac.

I can't take credit for all of these; most of them were shared with me by Matt Cowger, fellow VCDX and vSpecialist.

If anyone has any additional commands they'd like to share, I encourage you to add them to the comments on this post. Enjoy!

To find the IP address of the default gateway:

	netstat -nr -f inet | grep default | grep en | awk '{print $2}'

To find the interface name of the default route:

	netstat -nr -f inet | grep default | grep en | awk '{print $6}'

To find the IP address assigned to the interface for the default gateway:

	ORGGWIF=`netstat -nr -f inet | grep default | grep en | awk '{print $6}'\`  
	ifconfig $ORGGWIF | grep "inet " | awk '{print $2}'

To find the default gateway network:

	ORGGWIF=`netstat -nr -f inet | grep default | grep en | awk '{print $6}'\`  
	netstat -I $ORGGWIF -n | grep -v : | grep $ORGGWIF | awk '{print $3}'

To find the subnet mask for the default gateway network:

	ORGGWIF=`netstat -nr -f inet | grep default | grep en | awk '{print $6}'\`  
	system_profiler SPNetworkDataType | grep -A 15 $ORGGWIF | grep "Subnet Masks" | awk '{print $3}'

To convert the subnet mask into CIDR format:

	ORGGWIF=`netstat -nr -f inet | grep default | grep en | awk '{print $6}'\`  
	ORGGWMASK=`system_profiler SPNetworkDataType | grep -A 15 $ORGGWIF | grep "Subnet Masks" | awk '{print $3}'`  
	echo obase=2.$ORGGWMASK | tr . \; | bc | tr -d 0\\n | wc -c | awk '{print $1}'

To determine the wireless SSID to which your Mac is currently associated:

	/System/Library/PrivateFrameworks/Apple80211.framework/Versions/A/Resources/airport -I | grep SSID | tail -n 1 | awk '{print $2}'

CLI gurus and wizards are encouraged to share other useful commands in the comments below. Thanks!
