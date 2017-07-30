---
author: slowe
categories: Information
comments: true
date: 2010-01-07T13:41:19Z
slug: virtualization-short-take-33
tags:
- Cisco
- ESXi
- Storage
- UCS
- Virtualization
- VMware
- vSphere
title: 'Virtualization Short Take #33'
url: /2010/01/07/virtualization-short-take-33/
wordpress_id: 1786
---

Welcome to Virtualization Short Take #33, the first installation of the Virtualization Short Take series for 2010! This installation will be a bit lean, but I hope that you find something useful among these nuggets of information.

* This [article](http://kennethvanditmarsch.wordpress.com/2009/12/02/vsphere-freezing-vms-after-deleting-a-volume-from-the-san/) by Kenneth Van Ditmarsch, backed up by [this post](http://virtualgeek.typepad.com/virtual_geek/2009/12/an-important-vsphere-4-storage-bug-and-workaround.html) by Chad Sakac, underscore the need for proper operational documentation for your virtualization environment. Organizations that have taken the time to prepare operational procedures and train their staff on using the documented procedures will, in my opinion, be far less likely to fall victim to this vSphere storage bug. I'm not saying you've got to go crazy on documentation, but take the time to document and validate the core procedures your team is using. I think you'll find the results beneficial.

* Speaking of vSphere bugs, Chad also [describes a bug](http://virtualgeek.typepad.com/virtual_geek/2009/12/vsphere-4-nmp-rr-iooperationslimit-bug-and-workaround.html) affecting vSphere 4 (including Update 1) involving NMP and Round Robin. If you change the I/O Operation Limit for Round Robin (using the esxcli command), you might find that the value gets changed to some random value upon reboot. The workaround is to not modify the I/O Operation Limit (the default value is 1000).

* Scott Drummonds of VMware has been publishing a great series of articles on host swapping and memory overcommit. The series starts with a [discussion on host swapping](http://vpivot.com/2009/12/23/your-performance-enemy-host-swapping/) and the fact that VMware ESX does not track working sets within every VM (it would be too much overhead). He continues with this post on [using SSDs](http://vpivot.com/2009/12/24/solid-state-disks-and-host-swapping/) to help alleviate potential host swapping performance concerns (also see [this article](http://communities.vmware.com/blogs/chethank/2009/12/22/using-solidstate-drives-to-improve-performance-of-sql-databases-on-vsphere-hosts-when-memory-is-overcommitted) that Scott references in his post). In the last two posts, Scott [debunks some misconceptions](http://vpivot.com/2010/01/04/misunderstanding-memory-management/) about memory management and then goes to show why memory overcommit is important in [optimizing memory utilization](http://vpivot.com/2010/01/06/optimizing-memory-utilization/). Definitely some good stuff.

* The VMware Communities blog post about using SSDs to improve performance when memory is overcommitted (found [here](http://communities.vmware.com/blogs/chethank/2009/12/22/using-solidstate-drives-to-improve-performance-of-sql-databases-on-vsphere-hosts-when-memory-is-overcommitted)) put me to thinking. In the tests documented in that post, local SSDs were used. What if EFDs were used instead? I'd be curious to know the results. This would support a boot from SAN approach that is more amenable to Cisco UCS model of stateless computing (although I've said before that I'm not entirely sold on stateless computing in a virtualized environment, since the hypervisor negates some of the benefits).

* There are _some_ areas where the Cisco UCS stateless computing model really shines; Steve Chambers describes one such use case in this post on [multi-tenant DR with Cisco UCS](http://viewyonder.com/2009/11/19/over-commiting-your-infrastructure-for-multi-tenant-dr-with-cisco-ucs/).

* Here's a [useful document](https://supportforums.cisco.com/docs/DOC-6157) on installing VMware ESXi on Cisco UCS using the UCS Manager KVM. Last time I tried the UCS Manager KVM on my Mac, it was barely usable and you couldn't attach media; hopefully it's improved since then.

* Arnim van Lieshout has a great post on [geographically dispersed VMware clusters](http://www.van-lieshout.com/2009/11/geographically-dispersed-cluster-design/). One thought that occurred to me as I was reading this post was that while Arnim's post was written from the perspective of a production site and a DR site, the same challenges affect the use of external cloud providers and vCloud. As VMware and VMware's partners start to address these challenges, not only does the idea of geographically dispersed clusters start to look more realistic and more flexible, but so too does the idea of leveraging additional capacity from a cloud provider via vCloud.

* Interested in triggering an ESXi kernel panic on demand? Eric Sloof [shows you how](http://www.ntpro.nl/blog/archives/1388-Lets-create-some-Kernel-Panic-using-vsish.html).

* Finally, for users with the Nexus 1000V who want to update their ESX/ESXi hosts to Update 1 using the vihostupdate utility, Duncan's [post](http://www.yellow-bricks.com/2009/11/20/esxi-4-0-esxi-4-0-u1-update/) (and this [associated VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1015879)) provides all the information necessary to make it work properly.

I did find a couple other useful posts that I haven't had the time to properly read but which look interesting:

[VMware Desktop Reference Architecture Workload Simulator (RAWC)](http://www.ivobeerens.nl/?p=447)  

[White Paper: VMware vSphere 4 Performance with Extreme I/O Workloads](http://www.vmware.com/resources/techresources/10054)

That's it for this time around. I welcome all courteous comments or thoughts on any of the links or posts I've mentioned here. Thanks for reading!
