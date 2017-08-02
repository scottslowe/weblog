---
author: slowe
categories: Rant
comments: true
date: 2008-10-18T12:22:25Z
slug: important-note-regarding-vmware-over-nfs
tags:
- NAS
- NetApp
- NFS
- Storage
- Virtualization
- VMware
title: Important Note Regarding VMware over NFS
url: /2008/10/18/important-note-regarding-vmware-over-nfs/
wordpress_id: 991
---

We ran into an interesting set of problems at work this week, all of them related to running virtual machines on VMware ESX over NetApp NFS. While we haven't yet determined the root cause for all of the problem, we did uncover the root cause for one of the issues, and additional testing seems to indicate that one of the other problems may also be suffering from the same condition.

The problem seems to lie within some (now outdated) recommendations by NetApp for using NFS with VMware ESX. As late as June of this year, NetApp was recommending---via TR-3428, "NetApp and VMware Virtual Infrastructure 3 Storage Best Practices"--that users disable NFS locking on the storage system by setting the NFS.Lock.Disable setting to 1. Version 4.0 of TR-3428 was the version published in June of this year, and it still contained this recommendation.

As I understand it, this recommendation stemmed from a bug that existed in VMware ESX that caused long delays when removing VMware snapshots. As a workaround, NetApp recommended setting the NFS.Lock.Disable setting. Supposedly, VMware has fixed that bug with [this patch](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1005807), and the current version of TR-3428 (version 4.2, published in September 2008 and available [from NetApp's web site](http://www.netapp.com/us/library/technical-reports/tr-3428.html)) no longer contains the recommendation to set NFS.Lock.Disable.

Unfortunately, customers who followed (then current) best practices may have set this value, and in so doing may have exposed themselves to a "split brain" scenario in which a VM is simultaneously registered and running on multiple VMware ESX hosts. Users may be particularly at risk if they also run VMware HA and have not protected themselves against isolation response ([these articles][1] are tagged 'VMwareHA' and will provide more information on isolation response). We've had one large customer be affected by this problem, and another large customer who is having problems that we believe are related to this same issue.

There are a couple of important things to note:

* This is not a NetApp problem, but rather the result of a recommendation from NetApp. At that time, it was believed that this setting was necessary.

* In most cases VMs affected by this split-brain scenario will get corrupted and must be rebuilt or restored from backup.

We're not the only ones who have seen this behavior, either:

[VMware and NFS on NetApp Filers](http://thezendiary.blogspot.com/2008/08/vmware-and-nfs-on-netapp-filers.html)  

[NFS Datastores and what was their BIG issue...](http://vmwaretips.com/wp/?p=48)

The key takeaway from this is the following:

* If you haven't yet applied ESX350-200808401-BG, you should apply it. Don't wait. Now.

* If you did follow NetApp's recommendations and set NFS.Lock.Disable, then review the instructions below (credit to [Rick Scherer](http://vmwaretips.com/wp/) and several others, thank you!) to remedy the situation.

The instructions you should follow to restore NFS locking are here (these instructions include applying the patch):

1. Download patch ESX350-200808401-BG

2. Identify an ESX server against which you will apply the patch

3. Use VMotion and migrate the running VMs to other VMware ESX nodes

4. Install this patch on the selected VMware ESX server

5. In the Advanced Configuration settings ensure that NFS file locking is enabled with NFS.Lock.Disable=0 (you can also use the esxcfg-advcfg command to set this value)

6. Edit the `/etc/vmware/config` file and add the line `prefvmx.ConsolidateDeleteNFSLocks = "TRUE"`.

7. Save the changes to the file and reboot the VMware ESX host

8. Use VMotion to move VMs back to patched/NFS lock enabled/prefvmx enabled host

9. Repeat the steps above on each host in the cluster

So, again, if you haven't applied ESX350-200808401-BG, please do so as quickly as possible, and then ensure that NFS locking is enabled. If you've previously disabled NFS locking, then follow the steps above to restore NFS locking and protect yourself against this possible split-brain scenario.

[1]: /tags/vmwareha/
