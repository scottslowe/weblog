---
author: slowe
categories: Information
comments: true
date: 2009-02-26T22:43:55Z
slug: open-source-vmfs-driver
tags:
- Storage
- VCB
- Virtualization
- VMFS
- VMware
title: Open Source VMFS Driver
url: /2009/02/26/open-source-vmfs-driver/
wordpress_id: 1213
---

It looks like my post discussing a [universal cluster file system][1] may have been prescient. Shared with me first [via Twitter](http://twitter.com/rbrambley/statuses/1253835920), it appears that a company has already started building an [open source VMFS driver](http://code.google.com/p/vmfs/). It was also covered [here](http://vmetc.com/2009/02/26/fluid-operations-ecloudmanager-provides-open-source-vmfs-driver/) and [here](http://blog.core-it.com.au/?p=416).

As we discussed in the post about [cross-hypervisor cluster file systems][2], the implications of hypervisors potentially being able to share clustered storage are quite significant. Of course, there are _many_ hurdles to yet overcome, but what if this project develops and matures and becomes a valid tool? What will happen if open source Xen adopts VMFS as their cluster file system of choice? And although it's quite unlikely---potentially even impossible---that GPL licensing would allow it, what would happen if Microsoft were to use this open source VMFS driver with Hyper-V? Everything has to start somewhere...and that includes universal cluster file systems. This could be the beginning.

Of course, there are numerous other implications as well. As was mentioned on Twitter, this opens the possibility of [an open source VCB equivalent](http://twitter.com/vseanclark/statuses/1254225163) and it sets a precedent for a potential _de facto_ standard in cluster file systems for virtualization. The creation of standards through broad adoption by the vendors is something that I mentioned in my coverage of Citrix's [open sourcing their VHD implementation][3], and I mentioned at that time that I believed the move to open source the VHD implementation put Citrix in a good spot to be the leader in the creation of a de facto virtual hard disk standard. If this open source VMFS driver takes off, that puts VMware in a good position to have their cluster file system established as a de facto standard.

Does this mean we'll be running our virtual machines in VHD files stored on a VMFS partition? It's possible...let me know what you think.

[1]: {{< relref "2009-02-10-what-if-hypervisors-shared-a-file-system.md" >}}
[2]: {{< relref "2009-02-10-what-if-hypervisors-shared-a-file-system.md" >}}
[3]: {{< relref "2009-02-19-citrix-open-sources-their-vhd-implementation.md" >}}
