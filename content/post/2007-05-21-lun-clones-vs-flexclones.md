---
author: slowe
categories: Explanation
comments: true
date: 2007-05-21T12:13:58Z
slug: lun-clones-vs-flexclones
tags:
- NetApp
- Snapshots
- Storage
title: LUN Clones vs. FlexClones
url: /2007/05/21/lun-clones-vs-flexclones/
wordpress_id: 458
---

My recent article on [how to provision VMs using FlexClones][1] prompted a reader to ask the question, "What about using LUN clones?" That's an excellent question, and one that I myself asked when I first started using some of the advanced functionality of [Network Appliance](http://www.netapp.com/) storage systems. I had expected that this question would come up, and so I'd already begun preparing an article discussing LUN clones vs. FlexClones. My thanks go to Aaron for prompting the discussion!

LUN clones and [FlexClones](http://www.netapp.com/products/enterprise-software/storage-system-software/provisioning-volume-management/flexclone.html) share a lot of similarities:

* Both LUN clones and FlexClones are built on top of the [Snapshot](http://www.netapp.com/products/enterprise-software/storage-system-software/resiliency/snapshot.html) functionality resident within [Data ONTAP](http://www.netapp.com/products/enterprise-software/storage-system-software/storage-operating-systems/ontap-7g.html), the OS that runs on Network Appliance storage systems.

* Both LUN clones and FlexClones are _space conservative_, meaning the clones only take up as much space as required to store changes from the original.

* Both LUN clones and FlexClones can be created in seconds, and the size of the LUN (or FlexVol) does not significantly impact the time required to create the clone.

The key disadvantage to using LUN clones comes as a result of an interaction between how WAFL (the file system used by Data ONTAP) handles LUNs, and how Snapshots are performed and managed.

From WAFL's perspective, a LUN is really nothing more than a single file on the file system. You can see this by browsing via CIFS or NFS to a FlexVol that contains a LUN:

    [macosx:/Volumes/vol02$] slowe% ls -la
    total 25165856
    drwx------   1 slowe  admin        16384 Dec 31  1969 .
    drwxrwxrwt   8 root   admin          272 May 21 11:47 ..
    -rwx------   1 slowe  admin  12884901888 May 21 11:48 vswex02_vmfs

If I were to enable the `.snapshot` (or `~snapshot`) directory, we'd actually be able to see Snapshots of the LUN within that directory. In fact, this [NOW (NetApp on the Web; login required) article](http://now.netapp.com/Knowledgebase/solutionarea.asp?id=kb2130) describes mounting LUN snapshots inside the `.snapshot` (or `~snapshot`) as a way of recovering files or folders inside a snapshot. This technique is also applicable to recovering VMs from a LUN snapshot.

"OK," you may be saying, "so LUNs are implemented and managed as files. What's your point?"

My point is that Snapshots are handled _per volume_, and capture all the data in the active filesystem. A LUN exists as a file in the filesystem, so a Snapshot will capture that. When you create a LUN clone, you will then create another file in the active filesystem, which subsequent Snapshots will then capture. The end result is that you can end up with Snapshots that cannot be deleted because they reference a LUN clone which is, in turn, backed by another Snapshot. In these cases, you won't be able to delete Snapshots until you delete the LUN clone and all the Snapshots that reference that LUN clone. This [blog posting discusses](http://www.oneandonemakesthree.com/?q=node/50) this very problem and provides a Data ONTAP command to help track down the dependencies. (I'm also told that there is a NOW article on this problem as well, but I was unable to locate it.)

For short-term scenarios, LUN cloning works well and is, as some have pointed out, free (FlexClone requires a separate, paid license). For longer-term storage scenarios, however, LUN clones and the dependencies introduced by subsequent Snapshots of those LUN clones mean that FlexClones are a better solution. Since FlexClones are entire volumes stored within an aggregate, they aren't subject to the same problems as LUN clones (which are stored within a volume).

I hope this helps clear up some of the differences between using LUN clones and using FlexClones. Add your comments or questions below.

[1]: {{< relref "2007-05-11-how-to-provision-vms-using-netapp-flexclones.md" >}}
