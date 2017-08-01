---
author: slowe
categories: Information
comments: true
date: 2009-05-18T10:18:23Z
slug: summary-of-cdp-articles
tags:
- Cisco
- CLI
- IOS
- Networking
- Virtualization
- VMware
- vSphere
title: Summary of CDP Articles
url: /2009/05/18/summary-of-cdp-articles/
wordpress_id: 1366
---

For all you data storage guys and gals out there, CDP here means "Cisco Discovery Protocol", not "Continuous Data Protection." Sorry.

It seems that interest in VMware ESX's support for CDP is gaining interest and attention; fellow blogger Rich Brambley of VM /ETC recently [covered the topic](http://vmetc.com/2009/05/14/identify-esx-server-switch-ports-without-tracing-cables/) as well.

To help summarize some of the content that I've generated around VMware ESX and CDP, here is a list of the articles available on this site:

* [Identifying ESX Server NICs in Blades][1] - Although written for the blade server environment, the information presented in this article on how to use CDP to identify NICs works just as well for rack mount servers.

* [Next-Gen Stuff: Enabling CDP in ESX/ESXi][2] - This article describes how to enable CDP on a standard vSwitch (on both VMware ESX and VMware ESXi) as well as on a vNetwork Distributed Switch (officially abbreviated as vDS but I like to call them a dvSwitch).

* [Viewing CDP Data on VMware ESX][3] - Using the `esxcfg-info` command will actually show you the CDP data that's been collected by VMware ESX. This command is different from the command that Rich showed; I think I like Rich's command (also shared by wharlie in the comments to my article) better.

Enjoy!

[1]: {{< relref "2008-03-11-identifying-esx-server-nics-in-blades.md" >}}
[2]: {{< relref "2009-03-13-next-gen-stuff-enabling-cdp-in-esxesxi.md" >}}
[3]: {{< relref "2009-03-24-viewing-cdp-data-on-vmware-esx.md" >}}
