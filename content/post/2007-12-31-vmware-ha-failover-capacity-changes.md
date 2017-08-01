---
author: slowe
categories: Explanation
comments: true
date: 2007-12-31T11:09:51Z
slug: vmware-ha-failover-capacity-changes
tags:
- ESX
- Virtualization
- VMware
- VMwareHA
title: VMware HA Failover Capacity Changes
url: /2007/12/31/vmware-ha-failover-capacity-changes/
wordpress_id: 596
---

Continuing the discussion regarding VMware HA failover capacity started [in this article][1] and continued in [this follow-up article][2], it appears that VMware has added the ability to modify the "slot size" used in calculating VMware HA failover capacity as part of ESX Server 3.5 and VirtualCenter 2.5.

Alerted to [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1002080&sliceId=2&docTypeID=DT_KB_1_1&dialogID=38398839&stateId=0%200%2038394611) by Duncan Epping of Yellow Bricks in [this posting](http://www.yellow-bricks.com/2007/12/30/new-version-of-best-practices-and-advanced-features-for-vmware-ha-pdf/) on his site, there's a reference in the PDF from VMware that discusses a new option for setting the default "slot size". To quote from Duncan's site:

>If no VM reservations are set in a cluster VMware HA assumes cluster-wide average CPU and memory reservation sizes of 256 Mhz and 256 MB to use in admission control calculations. Alternative values can be specified instead...  
>
>Add the `das.vmMemoryMinMB = <value>` and `das.vmCpuMinMHz = <value>` option/value pairs to the cluster's settings where `<value>` represents the desired values in terms of MB and MHz. Higher values will reserve more space for failovers.

So this _looks_ like it's allowing us to specify how VMware HA should calculate the default slot size, but as in so many areas of VMware HA there is precious little documentation. I like VMware HA; I really do. But VMware needs to get somebody on the ball to document the exact configuration and operation of VMware HA so that this solution becomes less of the "black box" that it is today. As it stands currently, many customers are forgoing the benefits of VMware HA because it can't be reliably and consistently configured and debugged.

&lt;aside&gt;Cases in point: I was at a meeting before Christmas with a customer who is having problems with their VMware HA clusters and we can't find anyone---inside or outside of VMware---that can speak definitively about VMware HA, how it should be configured, or how it operates. Back in October, I blogged about [problems with isolation response][3], and still haven't gotten those problems resolved. C'mon, VMware, don't drop the ball!&lt;/aside&gt;

If anyone can shed some light on these new settings---I plan on testing them in the lab as soon as possible---that would be very useful. In the meantime, I encourage everyone to check out the PDF linked in the [VMware KB article on VMware HA best practices](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1002080&sliceId=2&docTypeID=DT_KB_1_1&dialogID=38398839&stateId=0%200%2038394611). And, just for fun, check out [this white paper](http://www.vmware.com/pdf/vi3_35_25_vmha.pdf) on the VMware HA VM failure monitoring functionality that's new in ESX Server 3.5.

[1]: {{< relref "2007-12-04-calculating-vmware-ha-failover-capacity.md" >}}
[2]: {{< relref "2007-12-07-more-discussion-on-vmware-ha-failover-capacity.md" >}}
[3]: {{< relref "2007-10-05-troubleshooting-vmware-ha-isolation-response.md" >}}
