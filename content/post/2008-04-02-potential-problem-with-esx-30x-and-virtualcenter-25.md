---
author: slowe
categories: Information
comments: true
date: 2008-04-02T09:37:46Z
slug: potential-problem-with-esx-30x-and-virtualcenter-25
tags:
- Virtualization
- VMware
title: Potential Problem with ESX 3.0.x and VirtualCenter 2.5
url: /2008/04/02/potential-problem-with-esx-30x-and-virtualcenter-25/
wordpress_id: 675
---

Heads up---I've been alerted to a potential problem when using ESX Server 3.0.x with VirtualCenter 2.5. Quoting from [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1004137):

>ESX Server 3.0.x hosts managed by VirtualCenter 2.5 must have their Host Agent (hostd) specially configured to work with VirtualCenter 2.5. This configuration is done automatically during the initial installation of the VirtualCenter Agent (vpxa). If an ESX Server patch is subsequently applied that includes the esx-hostd RPM package, like ESX Server patch 1002435, the patch replaces the existing Host Agent (hostd) configuration file (`/etc/vmware/hostd/config.xml`) and undoes vpxa's changes. This reverting of the Host Agent configuration file breaks connectivity with VirtualCenter 2.5.

There is a workaround that involves re-initializing the VirtualCenter Agent on the affected ESX servers via a script. I didn't have any ESX servers running ESX Server 3.0.x, but it appears that the referenced script is included with the agent when it is installed on the server.
