---
author: slowe
categories: Musing
comments: true
date: 2008-04-25T17:01:54Z
slug: my-thoughts-on-the-live-migration-quick-migration-discussion
tags:
- ESX
- HyperV
- Microsoft
- Virtualization
- VMotion
- VMware
- VMwareHA
title: My Thoughts on the Live Migration-Quick Migration Discussion
url: /2008/04/25/my-thoughts-on-the-live-migration-quick-migration-discussion/
wordpress_id: 696
---

In his three recent articles about Quick Migration and live migration, Jeff Woolsey spends a lot of effort differentiating Quick Migration, VMotion, and VMware HA. Personally, I thought the distinction between these features and the purposes they were intended to serve were pretty clear already, but apparently Microsoft's earlier claims that Quick Migration and live migration were comparable confused everyone.

The three articles from Jeff can be found here:

[Hyper-V Quick Migration and VMware Live Migration, Part 1](http://blogs.technet.com/virtualization/archive/2008/04/09/hyper-v-quick-migration-vmware-live-migration-part-1.aspx)  

[Hyper-V Quick Migration and VMware Live Migration, Part 2](http://blogs.technet.com/virtualization/archive/2008/04/14/hyper-v-quick-migration-vmware-live-migration-part-2.aspx)  

[Hyper-V Quick Migration and VMware Live Migration, Part 3](http://blogs.technet.com/virtualization/archive/2008/04/24/hyper-v-quick-migration-vmware-live-migration-part-3.aspx)

In the first article, Jeff discusses the importance of high availability (HA) in virtualization scenarios. He's absolutely right on target with his statements: HA is critical in virtualization implementations. I couldn't agree more. VMware recognizes this fact and includes VMware HA, and Microsoft recognizes this fact and provides integration between Hyper-V and Windows Server 2008 Failover Clustering. So far, so good.

In part two, Jeff goes on to state that VMotion doesn't work for unplanned downtime. Again, he's absolutely correct: VMotion _doesn't_ work for unplanned downtime. Then again, apart from the comments that Jeff claims to have received from VMware supporters stating VMotion was "far superior for unplanned host downtime and that it was a much better HA solution", I don't think anyone has ever claimed that VMotion was an HA solution. I know I certainly haven't. I can't recall VMware ever making that statement. After all, if VMotion were an HA solution, why would VMware have VMware HA? What point would there be in two different HA solutions?

Further in that same article, Jeff compares VMware HA with Windows Server 2008 Failover Clustering, aka Quick Migration, and states that they are comparable technologies. Once again, I agree; VMware HA and Quick Migration _are_ comparable technologies. Both will restart virtual machines on another host automatically in the event of a host failure. OK, Jeff and I still agree thus far.

Part three of Jeff's series wraps things up by attempting to downplay the importance of VMotion. In his words, "Even customers with Live Migration still wait until off hours to service the hardware." Unfortunately, this is where Jeff and I have to disagree. I don't know how many customers they spoke to, but I know I have customers that have live migration functionality who use it during business hours. Besides, live migration isn't just about hardware servicing or patching the root partition/parent partition/Service Console, it's also about enabling dynamic load balancing _a la_ VMware DRS, or enabling power savings in off-hours via DPM. After all, just because you _can_ shut down and/or power off guests to migrate them doesn't mean you will necessarily _want_ to in every instance. It may be acceptable for some workloads, but not for all workloads.

I just can't help but feeling that if Microsoft hadn't made the comparisons between VMotion and Quick Migration themselves earlier in Hyper-V's development, this sort of "unequal comparison" that Jeff is trying so hard to clear up wouldn't have happened.
