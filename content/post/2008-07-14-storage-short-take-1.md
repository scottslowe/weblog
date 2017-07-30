---
author: slowe
categories: Information
comments: true
date: 2008-07-14T18:36:26Z
slug: storage-short-take-1
tags:
- Cisco
- NetApp
- NFS
- SAN
- Storage
title: 'Storage Short Take #1'
url: /2008/07/14/storage-short-take-1/
wordpress_id: 761
---

My Virtualization Short Take series seems to be reasonably popular, so I thought I'd expand into another area that interests me: storage. Here is Storage Short Take #1! If this proves helpful or useful to readers, I'll continue the series on an irregular basis.

* If you're new to SnapMirror, especially synchronous SnapMirror, this [synchronous SnapMirror configuration guide](http://www.wideknowledge.net/synchronous-snapmiror-configuration-guide.html) may prove very helpful to you.

* Good friend Nick Triantos discusses [NetApp's Storage Recovery Adapter (SRA) for VMware Site Recovery Manager (SRM)](http://blogs.netapp.com/storage_nuts_n_bolts/2008/06/site-recovery-m.html). While the discussion is specific to NetApp, it's a good example of how the storage vendors are responsible for implementing vendor-specific functionality in the SRA. The other storage vendors supporting SRM are responsible for the same things for their storage arrays and storage array functionality.

* Isn't [this the truth](http://dcsblog.burtongroup.com/data_center_strategies/2008/07/storage-virtual.html)--everyone and their brother has "storage virtualization" functionality built into their products these days. Frankly, I'm tired of hearing about it.

* If you're running VMs on NFS on NetApp storage, you'll want to note [this knowledge base article](https://now.netapp.com/Knowledgebase/solutionarea.asp?id=kb37986) (NOW login required). It notes that a SCSI disk timeout increase may need to be set in order to accommodate cluster failover times.

* I recently came across [this white paper](http://www.emulex.com/white/hba/CiscoEmulexVirtualizationforVMware.pdf) from Emulex and Cisco regarding the use of N_Port ID Virtualization (NPIV) in a VMware environment. Personally, I found it to be a bit light on the technical details and a bit heavy on the marketing side, but otherwise useful.

* [This tool from NetApp](http://now.netapp.com/NOW/download/tools/sio_ntap/) (NOW login required) can help with approximating storage I/O. It's not perfect, but it might help provide some rough estimates. I'm sure other vendors have similar tools; readers are encouraged to share links to those tools in the comments.

Well, that's it for now. I'd love to hear feedback (good or bad) from readers as to whether this is even remotely useful or interesting.
