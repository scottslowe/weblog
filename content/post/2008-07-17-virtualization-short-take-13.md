---
author: slowe
categories: Information
comments: true
date: 2008-07-17T11:29:41Z
slug: virtualization-short-take-13
tags:
- HyperV
- NetApp
- Security
- Solaris
- Storage
- Virtualization
- VMware
- VMwareSRM
- Xen
title: 'Virtualization Short Take #13'
url: /2008/07/17/virtualization-short-take-13/
wordpress_id: 766
---

Here's the latest installation of Virtualization Short Takes, my occasionally-weekly view on various virtualization news, reviews, and other happenings. Hopefully I can share something interesting with you!

* Via VMblog.com, I saw that Transitive Corporation is [supporting the use of QuickTransit within Hyper-V](http://vmblog.com/archive/2008/07/14/transitive-quicktransit-integrates-with-microsoft-hyper-v.aspx) virtual machines. This is interesting because it extends the ability of Hyper-V to help customers consolidate applications. QuickTransit, in case you aren't aware, allows applications written for Solaris/SPARC environments to run in Linux/x86 environments. It was also the technology behind Apple's Rosetta, which allowed Mac users to run PowerPC apps on Intel Macs. Does anyone know if QuickTransit is supported within VMware VMs, or is this specific to Hyper-V?

* [This one](http://geekyschmidt.com/2008/07/12/just-say-no-to-os-level-virtualization/) was quite interesting to me. Question #2 is particularly applicable: why _is_ a reboot required, anyway? (Yes, yes, I know---there is a workaround that does not require a reboot. It's the principle of the matter.)

* Via various sources on the Internet, I learned about the [release of ESX Manager](http://www.esxguide.com/esx/). This looks like quite an interesting tool, although I have not yet had the opportunity to install or try it yet. Anyone out there tried this and have some feedback for us?

* Every now and then, something comes up about Citrix XenServer and Xen and it makes me wonder about the relationship between Citrix and the open source Xen community. The latest thing is what appears to be an offhand comment by Simon Crosby of Citrix where he says, "Because we own the hypervisor, we can do much more integration and development around it" (read it [in context here](http://searchstorage.techtarget.com.au/contents/25176-Open-source-hypervisors-pose-challenge-to-VMware)). What does that mean? What does "ownership" of the Xen hypervisor mean? And if the Xen hypervisor is licensed under an open source license (GNU GPL v2, according to [this page](http://www.xen.org/xen/)), how can Citrix make proprietary extensions to the hypervisor without being forced to release those extensions back to the community? I guess I just don't understand the relationship there and how it works. This is where the murky waters of a commercial entity "owning" an open source project come into play, in my mind.

* I ran across [this very useful tip](http://computing.dwighthubbard.info/index.php/2008/01/11/adding-a-virtual-switch-vswitch-to-vmware-esx-with-a-specific-number-of-ports/) for creating a vSwitch with a specific number of ports. It looks like Dwight Hubbard, the maintainer of the site, also has some other interesting posts. Might be worth adding his feed to your RSS reader.

* Nick Triantos [discusses](http://blogs.netapp.com/storage_nuts_n_bolts/2008/06/site-recovery-m.html) NetApp's Site Recovery Adapter (SRA) and its role with VMware Site Recovery Manager (SRM). Anyone have any links to similar discussions of the SRAs for other storage vendors?

* John Howard provides a great breakdown of [how Hyper-V generates dynamic MAC addresses](http://blogs.technet.com/jhoward/archive/2008/07/15/hyper-v-mac-address-allocation-and-apparent-network-issues-mac-collisions-can-cause.aspx) and how Hyper-V attempts to protect against MAC collisions in some circumstances.

* The VI3 Security Hardening Guide has been updated, which is good because some people felt it just [didn't go far enough](http://www.cio.com/article/426815/VMware_s_ESX_Hardening_Guideline_Falls_Far_Short_of_Secure_).

* VMware re-iterated their stance on being [storage protocol agnostic](http://blogs.vmware.com/vi/2008/07/vmware-is-stora.html), and in the article included a very useful table that summarizes the various products and technologies and which are supported with which storage protocols. While the rest of the post is helpful, that summary of supported features is probably the most helpful.

* Interesting in trying out Hyper-V, but don't have shared storage? Take a look at [this blog post](http://blogs.technet.com/josebda/archive/2008/07/16/failover-clustering-for-hyper-v-with-file-server-storage.aspx). I think you'll find it helpful.

I'm always on the lookout for other interesting or useful virtualization news, tips, and tricks, so feel free to share with me and other readers in the comments.
