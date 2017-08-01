---
author: slowe
categories: Liveblog
comments: true
date: 2007-09-13T16:12:31Z
slug: vmworld-2007-vcb-solutions-session
tags:
- Backup
- iSCSI
- SAN
- Virtualization
- VMware
- VMworld2007
title: VMworld 2007 VCB Solutions Session
url: /2007/09/13/vmworld-2007-vcb-solutions-session/
wordpress_id: 543
---

This session is titled "Best Practices for Architecting VMware Consolidated Backup Enabled Solutions," and it's being led by a couple of different presenters, one of whom is Dan Anderson, who also led the VCB session on Partner Day and leads the VCB labs (and led the VCB labs last year at VMworld 2006). So far, it looks like this session won't be a repeat of [Monday's Partner Day session][1], which is good.

The session started out with a review of VCB architecture, VCB components, and the interplay between the various portions of the VCB solution---the VCB proxy server, the pre-freeze and post-thaw scripts, file-level backups versus full VM backups, etc. This stuff I already knew and had already been covered in detail in previous sessions.

The discussion then moved into a mention of the various items that affect the design of a VCB solution. This would include things like the SAN architecture (Fibre Channel vs. iSCSI), software components (VCB version, VC version, ESX version, third-party backup software version), recovery mechanism, backup types, etc. Dan mentioned again the command-line interface for VMware Converter that was mentioned on Monday; I really need to dig up that information so that I can explore that possibility in greater detail.

It was stated again today that there are bad candidates for VMware snapshots---high I/O, high transaction VMs are bad candidates for snapshots, and therefore are bad candidates for use with VCB. (Recall that VCB uses snapshots to unlock the base VMDK for use by the VCB proxy, so a VM that is a bad candidate for snapshots is therefore a bad candidate for VCB-enabled backup solutions.)

I ended up leaving the session early because it turned out that a great deal more of the information that John and Dan were presenting was identical to the information that was presented on Monday at the partner session. This is not a reflection on the presenters, or the session materials; I'd just already seen most, if not all, of the materials before.

[1]: {{< relref "2007-09-10-vmworld-2007-partner-day-sessions.md" >}}
