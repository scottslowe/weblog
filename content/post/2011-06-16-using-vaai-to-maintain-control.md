---
author: slowe
categories: Musing
comments: true
date: 2011-06-16T09:40:36Z
slug: using-vaai-to-maintain-control
tags:
- SAN
- Storage
- Virtualization
- VMware
- vSphere
title: Using VAAI to Maintain Control?
url: /2011/06/16/using-vaai-to-maintain-control/
wordpress_id: 2313
---

Chris M Evans ([@chrismevans](http://twitter.com/chrismevans)) aka The Storage Architect recently posted an article on his site titled ["VAAI - Offloading or Maintaining Control?"](http://www.thestoragearchitect.com/2011/06/16/vaai-offloading-or-maintaining-control/) The article was a follow-up to a discussion with Nigel Poulton ([@nigelpoulton](http://twitter.com/nigelpoulton)) at HP Discover 2011.

The basic premise of the article as I understand it was a discussion of whether the introduction of VAAI (vStorage APIs for Array Integration), while it does bring benefits to the storage layer, also gives VMware more control. Quoting from the article:

>In comparison to NFS, it seems that by implementing VAAI features, VMware are looking to retain even more control over the storage layer than before.

At first glance, it would seem that this conclusion is logical: VMware is using its popularity and large market share to force storage vendors to write to their proprietary "array integration APIs", thus giving them another layer of control over the storage layer. VMware is the one pushing these integrations, so it seems like a reasonable conclusion, right?

The problem with this conclusion is that it overlooks one simple fact: the vStorage APIs for Array Integration aren't APIs at all; in fact, they are **nothing more than standard SCSI commands**. That's right: VAAI is composed of standard SCSI commands that have been defined and approved by the INCITS T10 committee. (If you're interested, you can get details on the commands in the SBC-3 standard available from [the T10 website](http://www.t10.org/).) Any hypervisor or OS running any file system on any array could leverage these commands and their functionality. There's absolutely nothing specific to VMware or VMFS, VMware's proprietary clustered file system, that would grant VMware any level of control. Microsoft could add support for these T10 commands and Hyper-V would reap the same benefits as VMware. The open source community could add support for these commands to Xen or KVM, if they so desired, and they would receive the same benefits that vSphere receives.

The flip side is true, too: there's nothing in any of these SCSI commands that grants a storage vendor a level of control, either. NetApp, EMC, HP, HDS, Dell/Compellent/EqualLogic---anyone of these vendors could (and many did) implement the commands and would support them regardless of the software running on the host.

In my mind, the fact that these commands are completely hypervisor- and OS---agnostic (meaning that any hypervisor or OS could implement them) completely deflates the theory that VAAI gives VMware another form of control at the storage layer.

What do you think? Speak up in the comments.
