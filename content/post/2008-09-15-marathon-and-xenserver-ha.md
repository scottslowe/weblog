---
author: slowe
categories: Explanation
comments: true
date: 2008-09-15T09:03:28Z
slug: marathon-and-xenserver-ha
tags:
- Citrix
- Virtualization
- VMwareHA
- Xen
title: Marathon and XenServer HA
url: /2008/09/15/marathon-and-xenserver-ha/
wordpress_id: 898
---

Marathon Technologies played a pivotal role in the development of the XenServer HA functionality [unveiled today in XenServer 5][1]. In addition to leading the development of the basic XenServer HA functionality that is included in XenServer, Marathon also offers enhanced levels of HA through the use of everRun VM.

In order to understand the enhanced levels of HA that everRun VM offers, it's important to first understand what XenServer HA does and does not address. Because many readers here are probably already familiar with VMware HA, I'll make some comparisons below.

1. First, XenServer HA is a best effort solution; there is no guarantee of a restart on another host. This is because XenServer HA does not ensure that resources are allocated on other hosts, so it's possible that a host failure might result in a situation where there is not enough resources to restart the VMs from that failed host. This would be similar to disabling availability constraints in VMware HA and allowing the HA cluster to power on more VMs than could be supported in the event of a host failure. In this regard, XenServer HA is a bit less powerful than VMware HA.

2. Second, XenServer HA heartbeats will not only leverage network interfaces, but will also send heartbeats across storage adapters as well. This is a significant advantage over VMware HA, as it helps to eliminate the dependency upon the network connectivity to the Service Console/Dom0.

With that in mind, adding everRun VM to an existing XenServer HA implementation now adds some useful new functionality:

* everRun will setup a separate compute environment (another VM) on a separate host to reserve memory in the event of a failure. This enables everRun to provide guaranteed recovery in the event of a host failure.

* everRun VM provides component-level fault tolerance, meaning that if a host's storage connectivity is lost, it can fail that connectivity over to a different host. This is accomplished through the use of private interconnects between members of the everRun VM resource pool. In my mind, this is pretty powerful stuff. Being able to fail disk I/O from the HBA in this server to the HBA in a different server over a set of interconnects is really powerful.

In the future, everRun VM will be extended again to provide "VM mirroring" functionality where two VMs are kept in lockstep with each other on two different hosts. In the event of any sort of failure---component or host---everything will fail over to the surviving host(s).

The addition of everRun VM to a XenServer 5 implementation can provide some significant features and functionality to help improve business continuity and help minimize downtime due to unplanned migrations.

[1]: {{< relref "2008-09-15-citrix-announces-xenserver-5.md" >}}
