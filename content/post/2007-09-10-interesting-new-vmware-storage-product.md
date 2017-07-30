---
author: slowe
categories: News
comments: true
date: 2007-09-10T10:53:32Z
slug: interesting-new-vmware-storage-product
tags:
- ESX
- iSCSI
- Storage
- Virtualization
- VMware
title: Interesting New VMware Storage Product
url: /2007/09/10/interesting-new-vmware-storage-product/
wordpress_id: 529
---

This [little blurb](http://lefthandnetworks.com/campaigns/vsa.php) popped up in [my virtualization pipe](http://pipes.yahoo.com/pipes/pipe.info?_id=MPM_9YC82xGHUHNFdbq02Q) this morning and, intrigued by what this might mean, I followed the link. It turns out that [LeftHand Networks](http://lefthandnetworks.com/) is introducing an virtual appliance that takes unused local storage on servers running [ESX Server](http://www.vmware.com/products/vi/esx/) and turns it into reusable iSCSI SAN storage that can then be re-presented to those same ESX servers for use in storing virtual machines. Very interesting!

One question that pops up in a lot of ESX deployments is the question of how to handle local disk storage on the ESX servers themselves:

* Should the ESX Servers even have local storage, or should they boot from SAN?

* Should we create a local VMFS datastore? If so, how large should it be?

* What can local VMFS datastores be used for?

Personally, I generally recommend local storage unless the customer has strong feelings or a good business case otherwise (it eases the complexity of the implementation), and I recommend creating a local VMFS datastore. (Like me, some of you out there have probably seen strange things that occur when a local VMFS datastore is not created.) If nothing else, these local VMFS datastores can be used as P2V destinations, destinations for cloned test VMs, etc., when you don't want to use (generally expensive) SAN storage. Now it appears that LeftHand Networks will let us use the extra storage on the ESX servers to create a SAN.

This is an interesting announcement, and it brings up additional questions that haven't yet been answered (Will this SAN allow for the use of VMware technologies such as VMotion, DRS, or HA? What happens to the SAN when a member host whose local storage is being used in the SAN fails? How will the SAN maintain redundancy and data consistency in such sitautions? How will the performance of this solution compare to a dedicated SAN? How does the pricing of this solution compare to a dedicated SAN?) Hopefully I'll be able to get some answers from LeftHand, since they'll be here in San Francisco this week for VMworld.
