---
author: slowe
categories: Information
comments: true
date: 2007-12-07T19:34:46Z
slug: more-discussion-on-vmware-ha-failover-capacity
tags:
- ESX
- Virtualization
- VMware
- VMwareHA
title: More Discussion on VMware HA Failover Capacity
url: /2007/12/07/more-discussion-on-vmware-ha-failover-capacity/
wordpress_id: 591
---

A few other bloggers have picked up VMwarewolf's article about [calculating VMware HA failover capacity](http://www.vmwarewolf.com/ha-failover-capacity/), which I [wrote about][1] a few days ago.

Thomas Bishop over at scalethemind.com (a fellow Planet V12n blogger) has this to say in [his blog posting](http://scalethemind.com/2007/12/how-vmware-ha-failover-capacity-is.html):

>As I would expect, HA errors on the side of caution when determining the capacity (uses the host with the least amount of RAM and the guest with the most amount of RAM as the basis of the calculation).

I can certainly see his point; after all, VMware HA is all about planning for _unexpected_ downtime. How is VMware HA going to know which VM is going to fail, and which hosts---if any---will have capacity to run the failed VM(s)? From that perspective, VMware HA almost _must_ take a worst-case scenario approach in order to be prepared for a situation in which the VM with the largest amount of configured or reserved memory must be restarted on a host with the least amount of physical RAM.

Unfortunately, the white paper to which Thomas linked (found [here](http://www.vmware.com/pdf/vmware_ha_wp.pdf) on VMware's web site) doesn't do a very good job of providing any additional detail on the calculation of VMware HA failover capacity; in fact, it seems to contradict VMwarewolf's settings to a certain extent. For example, take this statement from the tech doc:

>When computing required failover capacity, HA first considers the host with the largest capacity to run virtual machines with the highest resource requirements.

Unless I'm reading that statement incorrectly, that flies directly in the face of VMwarewolf's posting, which states just the opposite. However, the document goes on to say:

>HA might therefore be quite conservative in its estimates if the hosts in your cluster have a wide variance in the individual resources they provide.

In addition, the tech doc recommends the use of more uniform systems in HA clusters, so as to avoid issues such as what we've been discussing (where a 32GB host might be treated as a 16GB host for the purposes of calculating VMware HA failover "slots"). Otherwise, organizations may find themselves [in this boat](http://www.savagenomads.net/2007/12/05/calculating_vmware_ha_failover_capacity/) and VMware HA won't be able to accurately protect them against physical host failure.

I'll be sure to post more information here as soon as I have anything new to share. Likewise, if anyone can shed some definitive information to corroborate VMwarewolf's statements---just to validate them and ensure us that we aren't creating a storm of discussion over nothing---that would be great.

[1]: {{< relref "2007-12-04-calculating-vmware-ha-failover-capacity.md" >}}
