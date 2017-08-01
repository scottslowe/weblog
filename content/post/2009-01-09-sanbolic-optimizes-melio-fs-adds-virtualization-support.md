---
author: slowe
categories: News
comments: true
date: 2009-01-09T12:37:06Z
slug: sanbolic-optimizes-melio-fs-adds-virtualization-support
tags:
- Citrix
- HyperV
- Microsoft
- SAN
- Storage
- Virtualization
- VMware
- Windows
- Xen
title: Sanbolic Optimizes Melio FS, Adds Virtualization Support
url: /2009/01/09/sanbolic-optimizes-melio-fs-adds-virtualization-support/
wordpress_id: 1131
---

Back in June, I wrote about [Sanbolic and Melio FS][1] as a workaround for the "one-VM-per-LUN" limitation that Hyper-V's Quick Migration imposes. By running Melio FS on a Windows Server 2008 Hyper-V host, users could put multiple VMs in the same LUN and still use Quick Migration. At the same time, Sanbolic was also announcing that they were supporting the use of Melio FS, their clustered file system, inside Windows Server-based VMs running on VMware ESX.

In September, Sanbolic announced that they would be supporting [Melio FS in VMs running on Hyper-V][2]. This expanded their clustered file system support so that VMs running on either VMware ESX or Hyper-V could use Melio FS for shared storage access.

Yesterday, Sanbolic added support for running Melio FS in guests on Citrix XenServer, bringing support for their Windows-based clustered file system to VMs on all three major virtualization platforms. In addition, yesterday's announcement also indicated that Sanbolic was adding support for the Windows Server 2008 R2 beta, and that Melio FS had been optimized for block objects like virtual disk files and databases. The full press release is available [here as a PDF file](http://www.sanbolic.com/pdfs/Sanbolic_Press_Release_Heterogenous_Virtual_Data_Centers1.pdf).

Clearly, Sanbolic wants to protect the value of Melio FS as Microsoft prepares to enter the clustered file system market with Cluster Shared Volumes (CSV), included in the R2 beta. It's unclear to me whether CSV is going to be limited to virtualization only, addressing the "one-VM-per-LUN" issue, or whether Microsoft will also support CSV in other applications. By optimizing Melio FS for shared access to objects like virtual disk files and by extending support to run Melio FS in VMs on all the major platforms, Sanbolic hopes to establish Melio FS as a "de facto" standard in Windows-based clustered file systems.

[1]: {{< relref "2008-06-16-melio-fs-hyper-v-and-vmware-esx.md" >}}
[2]: {{< relref "2008-09-08-sanbolic-now-providing-shared-lun-access-for-hyper-v-guests.md" >}}
