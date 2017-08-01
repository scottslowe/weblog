---
author: slowe
categories: General
comments: true
date: 2007-01-09T13:11:51Z
slug: pending-articles
tags:
- ActiveDirectory
- ESX
- Interoperability
- Linux
- NetApp
- ONTAP
- Samba
- Virtualization
- VMware
title: Pending Articles
url: /2007/01/09/pending-articles/
wordpress_id: 398
---

With the holidays and all, a few articles that I've been working on for a while have been delayed. However, to assure you that more original content is on the way, here's a quick look at some of the articles that I hope to have published here within the next couple of weeks:

* _More information on using [Network Appliance Snapshots](http://www.netapp.com/products/software/snapshot.html) to recover virtual machines:_ I published an article on [using Snapshots to recover individual files from VMs][1], but I also want to look at recovering entire VMs using NetApp's snapshot functionality.

* _ESX-Active Directory integration:_ The last article I published on [ESX integration into Active Directory][2] needs to be updated for ESX Server 3.0.x. I may also roll a brief discussion of `esxcfg-auth` into this article as well; I'm not sure yet. I know that I want to talk more about `esxcfg-auth`, but I've not yet decided exactly what form that will take.

* _Data ONTAP-Active Directory integration:_ This one is just about done, but there are some technical details that still need work before it will be ready.

* _Using Data ONTAP to enhance VMware quick provisioning functionality:_ This article will look at the use of NetApp-specific functionality, like [FlexClone](http://www.netapp.com/products/software/flexclone.html), to enhance the quick provisioning functionality of VMware ESX Server and VirtualCenter. This article is one that I'm really excited about, but it's also the article that needs the most work before its ready. (Sorry.)

Other things that I plan to write about at some point in the future, but that aren't anywhere as close as the ones above (in other words, I haven't even started on them yet):

* Updated Linux-AD integration instructions that incorporate the use of Samba (this would be an amalgamation of earlier articles)

* Updated Solaris-AD integration instructions to account for any possible changes in Solaris 10 11/06 and, if possible, using Samba to assist with the process

* Using a NetApp storage system instead of a Windows server to provide the NFS storage for automounted home directories (similar to what I described in [this article][3])

* Using a Solaris server instead of a Windows server to provide the NFS storage for automounted home directories

* Using a NetApp storage system to provide NFS datastores to VMware ESX Server 3.0.x

If you see something on this list you'd like to see sooner rather than later, please let me know in the comments. If there are certain things that readers are more interested in seeing, I may be able to shuffle the order of articles around and accommodate the requests.

**UPDATE:** A number of the pending articles referenced above have been published:

* The updated Linux-AD integration instructions that incorporate the use of Samba are found in [Linux-AD Integration, Version 4][4]

* The updated Solaris-AD integration instructions that reference Solaris 10 11/06 (Update 3) and incorporate the use of Samba are found in [Solaris 10-AD Integration, Version 3][5]

* The use of Data ONTAP functionality, such as FlexClones, to enhance VMware quick provisioning functionality is discussed in three different articles:  

[How to Provision VMs Using NetApp FlexClones][6]  

[NetApp FlexClones with VMware, Part 1][7]  

[NetApp FlexClones with VMware, Part 2][8]

Enjoy!

[1]: {{< relref "2006-12-30-recovering-data-inside-vms-using-netapp-snapshots.md" >}}
[2]: {{< relref "2006-05-01-esx-server-integration-with-active-directory.md" >}}
[3]: {{< relref "2006-11-21-greater-ad-integration-via-nfs-and-automounts.md" >}}
[4]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[5]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
[6]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
[7]: {{< relref "2007-05-15-netapp-flexclones-with-vmware-part-1.md" >}}
[8]: {{< relref "2007-05-17-netapp-flexclones-with-vmware-part-2.md" >}}
