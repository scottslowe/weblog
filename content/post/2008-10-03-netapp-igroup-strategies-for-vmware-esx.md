---
author: slowe
categories: Explanation
comments: true
date: 2008-10-03T13:55:48Z
slug: netapp-igroup-strategies-for-vmware-esx
tags:
- ESX
- FibreChannel
- iSCSI
- NetApp
- Storage
- Virtualization
- VMware
title: NetApp iGroup Strategies for VMware ESX
url: /2008/10/03/netapp-igroup-strategies-for-vmware-esx/
wordpress_id: 964
---

Readers who have installed and configured VMware ESX in a storage area network (SAN) environment know that all the VMware ESX servers in an environment need to see the same LUNs with the same LUN IDs. This is necessary in order to avoid problems with VMFS resignaturing.

Similarly, readers who are familiar with configuring and managing NetApp storage arrays will know that NetApp igroups (initiator groups) are the mechanism whereby a host---or a group of hosts---are granted access to see a particular LUN on a specific LUN ID.

Because the igroup configuration is core to how LUNs are presented to hosts, and because VMware ESX has specific configuration requirements with regards to LUN presentation, it's necessary to take a closer look at strategies for how NetApp igroups should be configured and managed in a VMware ESX environment. There are basically two approaches:

1. Create a single igroup for all the VMware ESX hosts in the environment, then map LUNs to LUN IDs using that single igroup.

2. Create a single igroup for each VMware ESX host in the environment, and then map LUNs to LUN IDs for each igroup.

Obviously, each approach has its advantages and disadvantages, as described below.

## Using a Single Initiator Group

* Adding a new LUN to the entire group requires only one change: mapping a LUN to a LUN ID for that one initiator group.

* Similarly, only a single change is required to remove a LUN from the entire group of VMware ESX servers, by removing the one group-LUN ID  map.

* Storage administrators and VMware ESX administrators are assured that all the VMware ESX hosts will see the same LUNs with the same LUN IDs because all the hosts are placed into one group-LUN ID map. There is very little possibility for error.

* On the downside, there's no way to prevent a particular host from seeing a particular LUN. All hosts in the initiator group will see the LUN.

* The storage administrator kind of "loses track" of which hosts see the LUNs. Because all the initiators are thrown into the same group, it's more difficult to track down which hosts see a particular LUN. This is less true for iSCSI---where the hostname is often embedded in the IQN---but more prevalent for Fibre Channel, as the initiator group only contains World Wide Port Names (WWPNs). Mapping WWPNs to actual servers requires some additional steps.

## Using Multiple Initiator Groups

* It's easier for storage administrators to match hosts to initiators, because each host has its own initiator group on the NetApp storage array.

* The storage administrators and VMware ESX administrators have greater flexibility in determining which hosts see which LUNs, so it's possible to have a LUN visible to some VMware ESX servers and not others.

* There's greater room for error in accidentally mapping a LUN to a different LUN ID for one or more of the hosts, which can lead to an inability to access the VMFS datastore on that LUN.

* Multiple changes are required to add or remove a LUN from the entire set of VMware ESX servers; each LUN will have to be individually modified.

One could also mix-and-match these approaches to a certain extent; for example, an organization could employ a "hybrid" model that has the full set of base LUNs exposed to all servers via one large initiator group, but other LUNs exposed on a more granular basis via smaller initiator groups. Since an initiator can be included in more than one initiator group (as long as the initiator group uses the same OS type), this gives some additional flexibility.

I guess the purpose of this post is less to explain the different ways of using initiator groups and more to try to generate some discussion around the various ways they can be used. Hopefully, the initial explanation will be helpful to some readers, but what I'd really like to see is some more advanced and experienced readers sharing their strategies for using initiator groups in larger VMware ESX environments and what "best practices" they may be employing.
