---
author: slowe
categories: Education
comments: true
date: 2009-10-30T15:01:36Z
slug: vmware-ha-vmware-ft-and-os-clustering
tags:
- Microsoft
- Virtualization
- VMware
- VMwareFT
- VMwareHA
- vSphere
title: VMware HA, VMware FT, and OS Clustering
url: /2009/10/30/vmware-ha-vmware-ft-and-os-clustering/
wordpress_id: 1716
---

With the release of VMware vSphere 4 earlier this year, VMware officially introduced VMware Fault Tolerance (VMware FT), a new mechanism for providing extremely high levels of availability to virtual machine workloads. As I've talked with customers, I've noticed a growing number of customers who are unaware of the differences between the types of high availability that VMware provides (in the form of VMware HA and VMware FT) and operating system-level clustering (such as Microsoft Windows Failover Clustering). Although both types of technology are intended to increase availability and reduce downtime, they are very different and offer different types of functionality.

Consider these points:

* While using VMware HA will protect you against the failure of an ESX/ESXi host, VMware HA won't---by default---protect you against the failure of the guest operating system. An OS-level cluster, on the other hand, does protect against the failure of the guest operating system. **_+1 for OS-level clustering._**

* VMware clusters that are using VMware HA can choose to use VM Failure Monitoring and gain some level of protection against the failure of the guest operating system, but you still won't get protection of the specific application within the guest operating system, unlike an OS-level cluster. **_+1 for OS-level clustering._**

* These same arguments also apply to VMware FT. VMware FT won't protect you against guest operating system failure---a crash of the OS in the primary VM generally means a crash of the OS in the secondary VM at the same time---and it won't protect you against application failure. **_+1 for OS-level clustering._**

* You can't failover between systems using VMware HA or VMware FT in order to perform OS upgrades or apply OS patches. **_+1 for OS-level clustering._**

* Similarly, you can't failover between systems using VMware HA or VMware FT in order to do a rolling upgrade of the application itself. **_+1 for OS-level clustering._**

* Of course, the VMware technologies do have some advantages. Both VMware HA and VMware FT are far, far simpler to enable and configure than an OS-level cluster. **_+1 for VMware._**

* Both VMware HA and VMware FT don't require any application support in order to protect the VM and its workloads. **_+1 for VMware._**

* Neither VMware HA nor VMware FT require that you license specific editions of the guest operating system or application in order to be able to use their benefits. **_+1 for VMware._**

* VMware HA can produce higher levels of utilization within a host cluster than using OS-level clustering. **_+1 for VMware._**

* VMware FT can provide higher levels of availability than what is available in most OS-level clustering solutions today. **_+1 for VMware._**

This is not a knock against any of technologies listed---VMware HA, VMware FT, or OS-level clustering---but rather an exploration of their advantages, disadvantages, similarities, and differences. Hopefully, this will help readers who might not be as familiar with these products make a more informed decision about which technologies to deploy in their data center. (Hint: You'll probably need all of them.)
