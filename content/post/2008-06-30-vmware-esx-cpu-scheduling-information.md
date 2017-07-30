---
author: slowe
categories: Information
comments: true
date: 2008-06-30T13:21:30Z
slug: vmware-esx-cpu-scheduling-information
tags:
- ESX
- Virtualization
- VMware
title: VMware ESX CPU Scheduling Information
url: /2008/06/30/vmware-esx-cpu-scheduling-information/
wordpress_id: 752
---

The VMware peformance team blog (found [here](http://blogs.vmware.com/performance/)) published an entry last week on [ESX scheduler support for multiprocessor VMs](http://blogs.vmware.com/performance/2008/06/esx-scheduler-s.html). The blog entry itself was quite informative; in particular, the information about ESX 3.x's "relaxed co-scheduling" mechanism was very useful.

Long-time users of ESX will recall that mixing uniprocessor (UP) and multiprocessor (MP) workloads on an ESX server was _very strongly_ discouraged, especially in systems with a lower number of cores. This was due to the "strict co-scheduling" mechanism used in ESX 2.x. The "relaxed co-scheduling" mechanism introduced in ESX 3.x that allows for far greater flexiblity in scheduling UP and MP VMs. To quote the blog entry:

>Relaxed co-scheduling significantly reduces the possibility of co-scheduling fragmentation, improving overall processor utilization.

I was not aware that the vCPU scheduling mechanisms had changed between 2.x and 3.x, so this is good news.

However, the real treasure in this blog entry---for me, anyway---was the link to the in-depth performance documents being posted in the [Performance Community Forum](http://communities.vmware.com/community/vmtn/general/performance/forum). Here are some very detailed documents that provide outstanding information on topics like:

* the [VMkernel scheduler](http://communities.vmware.com/docs/DOC-5501)

* how ESX [co-schedules SMP VMs](http://communities.vmware.com/docs/DOC-4960) (more detailed than the blog entry)

* [using (or not using) hyper-threading with ESX](http://communities.vmware.com/docs/DOC-5101)

Overall, this is a ton of good content which will be very helpful to anyone seeking to solidify their knowledge of the ESX platform.
