---
author: slowe
categories: News
comments: true
date: 2009-08-24T10:02:53Z
slug: hytrust-appliance-15
tags:
- Networking
- Security
- Virtualization
- VMware
title: HyTrust Appliance 1.5
url: /2009/08/24/hytrust-appliance-15/
wordpress_id: 1554
---

Numerous other sites and numerous other bloggers have already covered the fact that HyTrust released version 1.5 of the HyTrust Appliance a couple of weeks ago. If you're attending VMworld 2009 in San Francisco, I believe that HyTrust will be demonstrating the new version and some of its new features at the show, so be sure to stop by.

I actually had the opportunity to sit down with Eric Chiu, President and CEO of HyTrust, when I was in San Jose a few weeks ago. We talked extensively about the features that were coming in version 1.5 of the HyTrust appliance. He's really excited about the features that have been added and the future plans that HyTrust has in place for the product.

Some of the new features included in version 1.5 include:

* Full support for VMware vSphere (both ESX and ESXi)
* Full support for VMware vCenter Server 2.5 and 4.0
* Support for two-factor authentication using RSA SecurID
* Label-based policy (akin to Web 2.0-style tagging)
* VM-to-host control
* VM-to-network segment control

Those last three features are pretty cool. The label-based policy engine is a new way for virtualization administrators to apply policy to VMs, hosts, and network segments that breaks out of the old tree or container styles of applying policy. For example, you could label (or tag) a VM as "PCI", and then specify that VMs labeled "PCI" can only be started on ESX/ESXi hosts also labeled as "PCI", or attached to network segments also labeled "PCI". This latter functionality---the ability to control network segment attachment based on HyTrust's labels---was functionality that HyTrust developed in close coordination with Cisco's Nexus 1000V development team. Further integration between HyTrust and the Nexus 1000V includes the ability to apply policy based on VNtag information.

Taken together, you can see that this new functionality is quite powerful and gives administrators a very flexible yet extensive ability to apply policy throughout the environment in a consistent fashion.

For more information, please visit the HyTrust site directly, or stop by and see them at VMworld 2009 in San Francisco next week.
