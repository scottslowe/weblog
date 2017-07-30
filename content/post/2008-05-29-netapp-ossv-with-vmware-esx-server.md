---
author: slowe
categories: Explanation
comments: true
date: 2008-05-29T16:29:35Z
slug: netapp-ossv-with-vmware-esx-server
tags:
- ESX
- NetApp
- Snapshots
- Storage
- Virtualization
- VMware
title: NetApp OSSV with VMware ESX Server
url: /2008/05/29/netapp-ossv-with-vmware-esx-server/
wordpress_id: 718
---

A couple of days ago my good friend Nick Triantos---formerly of [Storage Foo](http://storagefoo.blogspot.com/) fame, now writing at the [Storage Nuts & Bolts Blog](http://blogs.netapp.com/storage_nuts_n_bolts/)--published [a short piece](http://blogs.netapp.com/storage_nuts_n_bolts/2008/05/open-systems-sn.html) of a new version of Open Systems SnapVault (OSSV) that offers new functionality when used with VMware Infrastructure 3 (VI3).

For those that aren't familiar with OSSV, it's basically a way to bring NetApp-style snapshots to non-NetApp storage. For example, it's pretty common to use OSSV to take snapshots of a server that is not attached to a NetApp SAN and store those snapshots on a NetApp SAN. Think of a remote office/branch office kind of scenario, for example, where the branch server can be easily backed up using incremental snapshots to a NetApp storage system in the main location. It's pretty handy technology, to be honest.

After reading Nick's piece on OSSV, one question popped in my mind: is he talking about running OSSV on the guest, or running OSSV on the ESX host? So I contacted Nick, obtained clarification, and wanted to post that clarification here. (BTW, thanks, Nick, for taking the time to answer my questions and allowing me to discuss this here.)

Nick's article primarily focuses on the use of OSSV at the ESX level, running within the ESX Console OS (or Service Console). In this respect, OSSV will be taking snapshots of the VMDK files via the Service Console and replicating them over to a NetApp storage system. As with running OSSV in other environments, the snapshots are block-level incremental snapshots and thus only the changed blocks will be replicated across the wire when a snapshot is taken and shipped off to the destination storage system.

What's apparently new here is that NetApp has optimized the OSSV code so that the initial baseline will only capture the utilized portions of a VMDK file. Keep in mind that ESX, by default, uses thick-provisioned disks, so that a 30GB virtual disk takes up 30GB on the physical storage. OSSV 2.6, the new version, understands that if only 10GB of that 30GB VDMK is actually being utilized, it will only replicate 10GB of data. That is really important in reducing the overhead required for the initial baseline transfer. Thereafter, OSSV operates as usual by capturing and replicating only changed blocks.

This is a nice addition to OSSV, and it greatly increases the usefulness of OSSV in VI3 environments. But there is a drawback...has anyone figured it out yet?

If you guessed that taking block-level incremental snapshots of the VMDK files via the Service Console means that we lose file-level granularity within the guest file system, you would be correct! What does this mean? Basically, it means that if you have to restore a VMDK from an OSSV snapshot, you have to restore the _entire_ VMDK. You can't restore individual files within a guest from an OSSV snapshot taken while OSSV is running at the ESX layer.

Before you start knocking OSSV as worthless, however, consider that VCB suffers the same limitation when creating image-level backups. Also keep in mind that file-level backups are only possible with VCB when the guest is running Microsoft Windows, so you're forced to use image-level backups with any other operating system. Also keep in mind that file-level backups with VCB are slower than image-level backups. With these considerations in mind, it becomes clearer that the limitation is not inherent to OSSV _per se_, but rather a limitation of technologies operating at the ESX layer.

Of course, there are a number of workarounds to this; one way is to attach the restored VMDK to a different VM, then pull the individual files out that are needed. If you do need file-level granularity from within the guest OS (such as the ability to quickly and easily restore a specific file within a guest), then you can always run OSSV inside the guest and replicate block-level incremental snapshots from the guest over to a target NetApp storage system. Just be sure to keep these distinct procedures in mind as you plan backups of VMs using OSSV.

As always, I encourage you to ask questions, make comments, or add your thoughts below.

**UPDATE:** Keep in mind that OSSV is VMotion aware, meaning that block-level incrementals are preserved even after a VMotion operation. This is true even within DRS/HA clusters; you just have to install the OSSV agent on all ESX servers within the cluster. The real question: what effect will Storage VMotion have?
