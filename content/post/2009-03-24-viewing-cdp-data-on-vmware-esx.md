---
author: slowe
categories: Tutorial
comments: true
date: 2009-03-24T15:47:55Z
slug: viewing-cdp-data-on-vmware-esx
tags:
- Cisco
- CLI
- IOS
- Networking
- Virtualization
- VMware
title: Viewing CDP Data on VMware ESX
url: /2009/03/24/viewing-cdp-data-on-vmware-esx/
wordpress_id: 1251
---

A couple of weeks ago I wrote about [enabling Cisco Discovery Protocol (CDP) on next-gen ESX/ESXi][1] and made the comment that I hadn't yet found a way to view CDP data from the ESX side. One of the great things about writing a blog is that insightful and knowledgeable readers share some great information with you, and you learn a lot! That's the situation here.

Viewing CDP data from the Cisco switch is easy. From the switch's command prompt, use this command:

	show cdp neighbors

This will display the CDP information that the switch has gathered. When CDP is enabled on ESX/ESXi, that will include information on which VMnics are attached to which switch ports.

From the ESX side, you can use this command:

	esxcfg-info | more +/CDP\ Summary

This searches for the string "CDP Summary" in the output of the `esxcfg-info` command. The output from that command will include information about the switch to which the ESX host is connected, the ports to which the NICs are connected, and associated VLANs. The screenshot below shows some of the output from this command.

![CDP-related output of esxcfg-info](/public/img/esxcfg-info-cdp.png)

Thanks go to reader Larry for the information on this command. Other readers, feel free to continue to share information here. It is helpful!

[1]: {{< relref "2009-03-13-next-gen-stuff-enabling-cdp-in-esxesxi.md" >}}
