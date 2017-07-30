---
author: slowe
categories: Musing
comments: true
date: 2009-02-10T22:07:11Z
slug: what-if-hypervisors-shared-a-file-system
tags:
- SAN
- Storage
- Virtualization
title: What If Hypervisors Shared a File System?
url: /2009/02/10/what-if-hypervisors-shared-a-file-system/
wordpress_id: 1181
---

This is more of a "thinking out loud" sort of post, so bear with me. It doesn't necessarily have a point, other than to prompt readers to start thinking.

VMware has VMFS. Microsoft has CSV, or will in Windows Server 2008 R2. (Does XenServer have a clustered file system? I don't know.) What would happen if all the major hypervisors had a clustered file system in common? How would that impact virtualization?

I've written before about how cloud computing needs standards in order to really see broad adoption. People like to point to stuff like OVF, and that's great, but what about brass tacks kind of stuff like file systems? Would a clustered file system that worked equally well with Hyper-V, ESX, and XenServer make any difference? What sort of flexibility or interoperability might it bring?

And, for further thought, what benefit would there be for that same clustered file system to be accessible from the guest operating systems running in virtual machines on these various platforms?

I'd love to hear everyone's thoughts.
