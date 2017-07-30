---
author: slowe
categories: Rant
comments: true
date: 2008-12-15T14:35:40Z
slug: youve-got-to-be-kidding-me
tags:
- ESX
- Hardware
- HyperV
- Microsoft
- Virtualization
- VMFS
- VMotion
- VMware
- VMwareHA
- Windows
- Xen
title: You've Got to be Kidding Me
url: /2008/12/15/youve-got-to-be-kidding-me/
wordpress_id: 1096
---

Via [jtroyer on Twitter](http://twitter.com/jtroyer/statuses/1058924427), I learned of this post [comparing Hyper-V and VMware ESX](http://fawzi.wordpress.com/2008/12/14/hyper-v-vs-vmware/).

Now, I'll be the first to admit that I'm a VMware fan, but as others in the virtualization industry know I also recognize that VMware is not a "one size fits all" solution. There are many places where other virtualization solutions, Hyper-V included, may be a better solution for the customer. It really all depends upon the customer's needs.

That being said, I do have a few questions for the owner of this particular post:

* It's a subtle point, but there is a distinction between "free" and "available at no additional charge". I take vendors to task for this all the time. Hyper-V isn't free; it's available at no additional charge.

* What in the world is "para metal" virtualization? I've heard of bare-metal virtualization (the kind that VMware ESX, Xen, and Hyper-V all perform) and paravirtualization (the kind that VMware ESX and Xen can perform; I don't think Hyper-V does yet). Is "para metal" virtualization a blend of the two?

* Identical servers are not required in order to support VMware HA. They are required for VMotion. I would strongly suspect that Hyper-V will have similar requirements or will **require** hardware support like AMD-V/Intel FlexMigration when it's live migration feature arrives in 2010.

* Just because VMware ESX _can_ do memory overcommit doesn't mean you have to use it. It just gives you the flexibility to use it when you need it.

* I'm sorry, aren't Microsoft Windows Server 2008, NTFS, and Windows Failover Clustering every bit as "proprietary" as VMware ESX, VMFS, and VMware HA? Am I missing something here?

* VMware ESX installs just fine on x64 processors from both AMD and Intel. I have four x64 AMD servers sitting in my lab that are happily running both 32-bit and 64-bit guest operating systems.

* Since when does the hypervisor layer not containing any drivers---i.e., having your I/O drivers reside in the parent partition---have anything to do with direct hardware access by the guest OS? Unless I'm mistaken, these two items have nothing to do with each other. And the jury is still out as to whether having your I/O drivers in the parent partition, an approach used by both Hyper-V and Xen, is really a better approach.

Did I miss anything?

**UPDATE:** VMware blogger Jason Boche has [also responded](http://www.boche.net/blog/?p=690). Good points, Jason!
