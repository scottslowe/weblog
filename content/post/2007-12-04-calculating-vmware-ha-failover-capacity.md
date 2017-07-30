---
author: slowe
categories: Explanation
comments: true
date: 2007-12-04T17:02:22Z
slug: calculating-vmware-ha-failover-capacity
tags:
- ESX
- Virtualization
- VMware
- VMwareHA
title: Calculating VMware HA Failover Capacity
url: /2007/12/04/calculating-vmware-ha-failover-capacity/
wordpress_id: 588
---

Most readers probably know that VMware High Availability (or VMware HA) is the feature of VMware Infrastructure 3 that allows for virtual machines (VMs) to be rebooted on another available host in the event of an unexpected host failure. In these types of scenarios, a physical host goes down unexpectedly, typically due to hardware failure, and with it go a bunch of VMs. With VMware HA, these downed VMs will reboot on a different physical server in the HA cluster, thus minimizing downtime.

I had always considered that the "failover capacity," i.e., how the number of VMs that could be supported in an HA cluster with a failed host, was calculated by VMware HA in an intelligent fashion similar to that used by VMware Distributed Resource Scheduling (VMware DRS). In other words, VMware HA would look at the needs of the downed VM, consider what is available across the various hosts, and then place virtual workloads accordingly. Sadly, that is not the case.

This article, titled [HA Failover Capacity](http://www.vmwarewolf.com/ha-failover-capacity/), by a VMware technical support engineer---"VMwarewolf"---provides more detailed information on how failover capacity is actually calculated. What actually happens is that VMware HA calculates a number of "slots" based on the least amount of RAM installed in a server in the cluster divided by the most amount of RAM configured for any VM in the cluster. In the article, the example is given of a server that has 16GB with at least one VM that is configured for 2GB of memory. That would create 8 slots (16GB / 2GB = 8 slots) for VMware HA.

That in and of itself is bad enough, since not all VMs will require 2GB, but here's where it gets worse. After calculating the number of "slots" available on the smallest server in the cluster, it then extrapolates the total number of slots in the cluster _using the number from that smallest server._ So if one server in the HA cluster has 16GB but the remaining three have 64GB, all four servers will be treated as having only 16GB for the purposes of calculating HA "slots". So, instead of the three bigger servers coming up with 32 slots, they'll show up as having 8 slots. Ouch!

Be sure to keep this in mind when creating VMware HA clusters and planning for fault tolerance.

Also, if you aren't reading VMwarewolf's stuff, you may want to start. He (or perhaps she?) is posting some good stuff.
