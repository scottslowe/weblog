---
author: slowe
categories: News
comments: true
date: 2008-04-18T16:28:32Z
slug: live-migration-and-vm-failover-for-oracle-vm
tags:
- Virtualization
- Xen
title: Live Migration and VM Failover for Oracle VM?
url: /2008/04/18/live-migration-and-vm-failover-for-oracle-vm/
wordpress_id: 686
---

If the press release I received via e-mail today is accurate, then live migration (known as VMotion in the VMware world) and VM failover (known as VMware HA in the VMware world) are coming to Oracle VM:

>Saratoga, California  April 17, 2008  Thinsy Corporation today announced the immediate availability of a new version of their Peer to Peer SAN Replacement technology LiveSync, for Oracle VM Server 2.1.1. Once the LiveSync rpm is installed on the Direct Attached Storage equipped Oracle VM Servers, High Availability Failover and LiveMigration are enabled. Clone VM support, a new feature which enables the cloning of VMs in less than fifteen seconds was also unveiled by Thinsy Corporation.

(By the way, I say "if the press release is accurate" not because I think that Thinsy Corporation isn't being truthful; it's just that we all know how vendors can spin their products to make them sound good. When I see it work, then I'll believe it.)

One particularly interesting note about this product announcement is that it seems to bring this functionality to Oracle VM without the need for shared SAN storage ("...installed on the Direct Attached Storage equipped Oracle VM Servers..."). If so, this would give Oracle VM a leg up against VMware in the midmarket space, where in many cases the cost of shared SAN storage can be the biggest hurdle to virtualization adoption.
