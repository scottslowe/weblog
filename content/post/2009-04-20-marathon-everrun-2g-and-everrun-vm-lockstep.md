---
author: slowe
categories: News
comments: true
date: 2009-04-20T18:00:22Z
slug: marathon-everrun-2g-and-everrun-vm-lockstep
tags:
- Citrix
- Microsoft
- Virtualization
- Windows
- Xen
title: Marathon everRun 2G and everRun VM Lockstep
url: /2009/04/20/marathon-everrun-2g-and-everrun-vm-lockstep/
wordpress_id: 1308
---

I first [wrote][1] about Marathon Technologies and their everRun VM product last September just prior to the start of VMworld 2008 in Las Vegas, NV. Back at the start of 2009 I also mentioned Marathon's joint development agreement with Microsoft and the intended plan to bring everRun VM to Hyper-V environments.

Today Marathon announced the availability of their everRun VM Lockstep product, which brings full circle the product announcement from last September. This product, which runs only on Citrix XenServer, puts into place the "three levels" of availability that Marathon has often spoke of:

* Auto-restart high availability (XenServer HA)
* Component-level fault tolerance
* Full system-level fault tolerance

With full system-level fault tolerance, Marathon is able to provide organizations with the ability to protect applications with the highest levels of availability, eliminating downtime due to physical server failure. If a physical server fails, the virtual machine continues running on another physical server without any disruption.

The announcement of everRun VM Lockstep gives Marathon and Citrix a slight edge over competitor VMware, whose similar VMware Fault Tolerance offering has been demonstrated and discussed extensively but has not been officially announced. Given that Marathon expects everRun VM Lockstep to be available within 30 days, they may also have an edge over VMware in getting their product to market as well. Marathon everRun VM Lockstep will run on the free version of Citrix XenServer.

At the same time, Marathon is also announcing everRun 2G, the successor to Marathon's everRun HA and everRun FT products for Windows Server environments. Marathon everRun 2G combines and extends the functionality of the previous generation of products, allowing organizations to provide high availability to any Windows application without modification or scripting. Like everRun VM, everRun 2G will offer "dialable" availability ranging from automated HA to full system-level fault tolerance.

Like everRun VM Lockstep, everRun 2G is expected to be available within the next 30 days.

Visit the Marathon Technologies web site for more information.

[1]: {{< relref "2008-09-15-marathon-and-xenserver-ha.md" >}}
