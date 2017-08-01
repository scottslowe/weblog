---
author: slowe
categories: Liveblog
comments: true
date: 2008-12-03T15:43:52Z
slug: 2031-enhancements-to-netapp-cloning-technology
tags:
- Deduplication
- Insight2008
- NAS
- NetApp
- NFS
- ONTAP
- Snapshots
- Storage
- WAFL
title: '2031: Enhancements to NetApp Cloning Technology'
url: /2008/12/03/2031-enhancements-to-netapp-cloning-technology/
wordpress_id: 1081
---

This session provided information on enhancements to NetApp's cloning functionality. These enhancements are due to be released along with Data ONTAP 7.3.1, which is expected out in December. Of course, that date may shift, but that's the expected release timeframe.

The key focus of the session was new functionality that allows for Data ONTAP to clone individual files _without a backing Snapshot._ This new functionality is an extension of NetApp's deduplication functionality, and is enabled by changes within Data ONTAP that enable block sharing, i.e., the ability for a single block to appear in multiple files or in multiple places in the same file. The number of times these blocks appear is tracked using a reference count. The actual reference count is always 1 less than the number of times the block appears. A block which is not shared has no reference count; a block that is shared in two locations has a reference count of 1. The maximum reference count is 255, so that means a single block is allowed to be shared up to 256 times within a single FlexVol. Unfortunately, there's no way to view the reference count currently, as it's stored in the WAFL metadata.

As with other cloning technologies, the only space that is required is for incremental changes from the base. (There is small overhead for metadata as well.) This functionality is going to be incorporated into the FlexClone license and will likely be referred to as "file-level FlexClone". I suppose that cloning volumes with be referred to as "volume FlexClone" or similar.

This functionality will be command-line driven, but only from advanced mode (must do a `priv set adv` in order to access the commands). The commands are described below.

To clone a file or a LUN (the command is the same in both cases):

	clone start <src_path> <dst_path> -n -l

To check the status of a cloning process or stop a cloning process, respectively:

	clone status  
	clone stop

Existing commands for Snapshot-backed clones (`lun clone` or `vol clone`) will remain unchanged.

File-level cloning will integrate with Volume SnapMirror (VSM) without any problems; the destination will be an exact copy of the source, including clones. Not so for Qtree SnapMirror (QSM) and SnapVault, which will re-inflate the clones to full size. Users will need to run deduplication on the destination to try to regain the space. Dump/restores will work like QSM or SnapVault.

Now for the limitations, caveats and the gotchas:

* Users can't run single-file SnapRestore and a clone process at the same time.

* Users can't clone a file or a LUN that exists only in a Snapshot. The file or LUN must exist in the active file system.

* ACLs and streams are not cloned.

* The `clone` command does not work in a vFiler context.

* Users can't use synchronous SnapMirror with a volume that contains cloned files or LUNs.

* Volume SnapRestore cannot run while cloning is in progress.

* SnapDrive does not currently support this method of cloning. It's anticipated that SnapManager for Virtual Infrastructure (SMVI) will be the first to leverage this functionality.

* File-level FlexClone will be available for NFS only at first. Although it's possible to clone data regions within a LUN, support is needed at the host level that isn't present today.

* Because blocks can only be shared 256 times (within a file or across files), it's possible that some blocks in a clone will be full copies. This is especially true if there are lots of clones. Unfortunately, there is no easy way to monitor or check this. `df -s` can show space savings due to cloning, but that isn't very granular.

* There can be a maximum of 16 outstanding clone operations per FlexVol.

* There is a maximum of 16TB of shared data among all clones. Trying to clone more than that results in full copies.

* The maximum volume size for being able to use cloning is the same as for deduplication.

Obviously, VMware environments---VDI in particular---are a key use case for this sort of technology. (By the way, in case no one has yet connected the dots, this is the technology that I [discussed here][1]). To leverage this functionality, NetApp will update a tool known as the Rapid Cloning Utility (RCU; described in more detail in [TR-3705](http://www.netapp.com/us/library/technical-reports/tr-3705.html)) to take full advantage of file-level FlexCloning after Data ONTAP 7.3.1 is released. Note that the RCU is available today, but it only uses volume-level FlexClone.

[1]: {{< relref "2008-02-28-i-love-it-but-its-not-available.md" >}}
