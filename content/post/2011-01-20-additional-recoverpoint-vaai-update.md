---
author: slowe
categories: Information
comments: true
date: 2011-01-20T14:56:46Z
slug: additional-recoverpoint-vaai-update
tags:
- EMC
- Interoperability
- Storage
- Virtualization
- VMware
- vSphere
title: Additional RecoverPoint-VAAI Update
url: /2011/01/20/additional-recoverpoint-vaai-update/
wordpress_id: 2220
---

In late October and early November 2010 I published a couple of articles on interoperability between EMC RecoverPoint and VAAI (vStorage APIs for Array Integration, part of VMware vSphere 4.1). If you'd like to go back and read those articles for completeness, here are the links:

[RecoverPoint and VAAI Interoperability][1]  

[RecoverPoint and VAAI Update][2]

In the comments to the second article, a reader indicated that he'd seen a problem when using RecoverPoint (which I'll abbreviate as RP from here on) and VAAI. In this particular situation, his consistency groups (CGs) were failing to initialize. The only way he could get his CGs to properly initialize and replicate was to disable VAAI. I provided his information to RP product management, who after some additional testing found that there was indeed a potential issue when using hardware-assisted locking (also referred to as CAS, after the SCSI command Compare and Swap) in conjunction with the FLARE 30 array splitter and VAAI.

The fix for this potential issue is found in a type 2 patch for FLARE 30; this patch brings the FLARE 30 version to 4.30.000.5.509.

&lt;aside&gt;For those of you that don't know, type 2 patches are patch revisions that run through full qualification cycles and have formal releases. These are the sorts of patches that you should install when you can in order to stay current.&lt;/aside&gt;

If you are running a revision of FLARE 30 prior to 509, you could see issues when using the RP array splitter and VAAI, and you will need to disable VAAI in order to resolve the issues. With the latest revision of FLARE 30 (4.30.000.5.509), this RP-VAAI interoperability issue is resolved.

If you have any questions, please feel free to ask them in the comments below. Thanks!

[1]: {{< relref "2010-10-28-recoverpoint-and-vaai-interoperability.md" >}}
[2]: {{< relref "2010-11-01-recoverpoint-and-vaai-update.md" >}}
