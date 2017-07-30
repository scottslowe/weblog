---
author: slowe
categories: News
comments: true
date: 2007-04-06T09:56:28Z
slug: esx-security-vulnerabilities
tags:
- ESX
- Security
- Virtualization
- VMware
title: ESX Security Vulnerabilities
url: /2007/04/06/esx-security-vulnerabilities/
wordpress_id: 437
---

Within the last few days, [VMware](http://www.vmware.com/) has acknowledged the presence of a number of security vulnerabilities within the flagship [ESX Server](http://www.vmware.com/products/vi/esx/) product. At least two of these vulnerabilities were discovered during an internal security audit of the code, but it's unclear how many of the rest were internally discovered or externally discovered and reported to VMware. There has been no indication that there are any publicly-available exploits for any of these vulnerabilities.

In fact, in two of the vulnerabilities disclosed by VMware, the attack vectors are currently listed as "unknown."

For more information, refer to one of the following links:

[Secunia - VMware ESX Server Multiple Vulnerabilities](http://secunia.com/advisories/24788/)

[SecurityTracker - VMware ESX Server Double Free Error May Let Remote Users Execute Arbitrary Code](http://www.securitytracker.com/alerts/2007/Apr/1017875.html)

[VMware - ESX Server 3.0.1, Patch Bundle ESX-6431040: Security Fix For Buffer Overflow Issue](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&externalId=6431040)

Installing the patches does require that all VMs on that host be powered off or moved to another host using VMotion, but a reboot of the physical host is _not_ required.
