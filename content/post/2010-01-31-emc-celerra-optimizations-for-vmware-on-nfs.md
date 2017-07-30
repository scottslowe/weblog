---
author: slowe
categories: Explanation
comments: true
date: 2010-01-31T18:34:40Z
slug: emc-celerra-optimizations-for-vmware-on-nfs
tags:
- Celerra
- EMC
- NFS
- Storage
- Virtualization
- VMware
title: EMC Celerra Optimizations for VMware on NFS
url: /2010/01/31/emc-celerra-optimizations-for-vmware-on-nfs/
wordpress_id: 1809
---

Recently Jason Boche posted [some storage performance numbers](http://www.boche.net/blog/index.php/2010/01/23/vmtn-storage-performance-thread-and-the-emc-celerra-ns-120/) from his EMC Celerra NS-120. Out of those storage performance tests, Jason noted that the NFS performance of the NS-120 seemed a bit off, so he contacted EMC's vSpecialist team (also known as "Chad's Army", but I like vSpecialists better) to see if there was something that could or should be done to improve NFS performance on the NS-120. After collaborating internally, one of our team members (a great guy named Kevin Zawodzinski) responded to Jason with some suggestions. I wanted to reproduce those suggestions here for everyone's benefit.

Note that some of these recommendations are already found in the multi-vendor NFS post available on [here](http://virtualgeek.typepad.com/virtual_geek/2009/06/a-multivendor-post-to-help-our-mutual-nfs-customers-using-vmware.html) on Chad's site as well as [here](http://blogs.netapp.com/virtualstorageguy/2009/06/a-multivendor-post-to-help-our-mutual-nfs-customers-using-vmware.html) on Vaughn Stewart's site.

In addition, most if not all of these recommendations are also found in the VMware on Celerra best practices document available from EMC's web site [here](http://www.emc.com/collateral/hardware/technical-documentation/h5536-vmware-esx-srvr-using-celerra-stor-sys-wp.pdf).

Without further ado, then...

* As has been stated on multiple occasions and by multiple people, be sure that virtual machine disk/application partitions have been properly aligned. We recommend a 1MB boundary. Note that Windows Server 2008 aligns at a 1MB boundary automatically.

* Use a block size of 8KB unless other recommended or required by the application vendor. Note that the default NTFS block size is 4KB. (Pages 128 through 138 of the Celerra best practices document contain more information on this bullet as well as the previous bullet.)

* Turn on the uncached write mechanism for NFS file systems used as VMware datastores. This can have a significant performance improvement for VMDKs on NFS but isn't the default setting. From the Control Station, you can use the `server mount` command with the "uncached" option to turn on the uncached write mechanism. The whole command would look something like `server_mount <data mover name> -option <options>,**uncached** <file system name> <mount point>` (the asterisks around "uncached" are for emphasis; don't actually include them in the command). Be sure to review pages 99 through 101 of the VMware on Celerra best practices document for more information on the uncached write mechanism and any considerations for its use.

* Change the VMware ESX settings `NFS.SendBufferSize` and `NFS.ReadBufferSize` to a value that is a multiple of 32. The recommended value is 64. See page 73 of the best practices document for more details.

* If you've adjusted the `NFS.MaxVolumes` parameter in order to have access to more than 8 NFS datastores, you should also adjust `Net.TcpIpHeapSize` and `Net.TcpIpHeapMax` parameters. The increase should be proportional; if you increase the maximum volumes to 32 (a common configuration), then you should increase the other parameters by a factor of 4 as well. Page 73 of the best practices document covers this. [This VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1007909) and [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=2239) also have more information.

* Although not directly related to performance, best practices call for setting `NFS.HeartbeatFrequency` (or `NFS.HeartbeatDelta` in VMware vSphere) to 12, `NFS.HeartbeatTimeout` to 5, and `NFS.HeartbeatMaxFailures` to 10.

* Ensure that the LUNs backing the NFS file systems are allocated to the clar_r5_performance pool. This configuration will balance the load across the SPs, LUNs, etc., and help improve performance.

Depending upon the other workloads on the system, another NFS performance optimization is to ensure that the maximum amount of write cache on the SPs is configured. However, be aware this may impact other workloads on the array.

As Jason noted in his post, implementing these changes---especially the uncached write mechanism---offered performance benefits for NFS workloads.

Keep these configuration recommendations in mind when setting up your EMC Celerra for VMware on NFS.
