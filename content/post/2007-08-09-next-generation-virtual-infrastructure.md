---
author: slowe
categories: News
comments: true
date: 2007-08-09T09:34:13Z
slug: next-generation-virtual-infrastructure
tags:
- ESX
- Virtualization
- VMware
title: Next-Generation Virtual Infrastructure
url: /2007/08/09/next-generation-virtual-infrastructure/
wordpress_id: 503
---

If the [list of features in ESX Server 3.1.0/VirtualCenter 2.1.0](http://www.virtualization.info/2007/08/vmware-esx-server-31-virtualcenter-21.html) as exposed by [virtualization.info](http://www.virtualization.info/) actually make it into the shipping product, [VMware](http://www.vmware.com/) will even more rapidly increase the gap between them and the rest of the virtualization market. The features list sounds like a "wish list" from virtualization customers far and wide:

* DMotion (the ability to live migrate a VM and relocate the VM storage to a different SAN LUN)

* Host and guest patch management functionality

* VCB/VMware Converter integration

* VMware HA guest-level visibility (to restart a guest after a failure occurs inside the guest)

* CDP support (perhaps furthering the rumors of [Cisco switches running on ESX Server][1])

* Various important networking upgrades, such as 10Gb Ethernet support, TOE card support, IPv6 support, and support for various network load balancing mechanisms

* SATA support

* N_Port ID Virtualization (NPIV) support (this will allow us to present virtual Fibre Channel HBAs to guests)

* iSCSI support within VCB (available today with VCB 1.0.3)

And the list goes on and on...

If all these features make it into the shipping product (and I haven't seen or heard about potential ship dates yet), VMware will be, in my opinion, virtually untouchable. (Sorry, bad pun.) Is anyone else out there in the virtualization market anywhere close to matching this feature set? (I'm not aware of any other products that can even come close, but if I'm missing something I'm sure my readers will let me know!)

I heartily recommend you hop over to [Alessandro's site](http://www.virtualization.info/) and have a look at the full report. Outstanding work, Alessandro!

[1]: {{< relref "2007-07-29-cisco-switches-on-vmware.md" >}}
