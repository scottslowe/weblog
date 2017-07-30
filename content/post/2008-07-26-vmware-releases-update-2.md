---
author: slowe
categories: News
comments: true
date: 2008-07-26T19:38:30Z
slug: vmware-releases-update-2
tags:
- ESX
- ESXi
- Snapshots
- VCB
- Virtualization
- VMotion
- VMware
title: VMware Releases Update 2
url: /2008/07/26/vmware-releases-update-2/
wordpress_id: 770
---

VMware has released Update 2 for VMware Infrastructure 3 version 3.5, which includes updates to VMware ESX, VMware ESXi, VirtualCenter, and VMware Consolidated Backup (VCB). Check the [Release Notes](http://www.vmware.com/support/vi3/doc/vi3_esx35u2_vc25u2_rel_notes.html) for the full details; I won't reproduce them here, but instead I'll just point out the particularly interesting details.

Also reporting this information (at the time of this writing) are [David from VMblog.com](http://vmblog.com/archive/2008/07/26/vmware-releases-update-2-for-esx-3-5.aspx), [Rich from VM /ETC](http://vmetc.com/2008/07/26/esx-35-update-2-released-with-new-fixes-and-new-features/#more-546), and [Duncan at Yellow Bricks](http://www.yellow-bricks.com/2008/07/26/esx-35-update-2-available-now/). Rich's post also highlights in red the features that he finds most significant.

Some of the features and/or functionality added in Update 2 that I find most notable include:

* The biggest, in my mind, is VSS quiescing support. This allows VMware snapshots to leverage VSS for more consistent snapshots. Microsoft had been using the lack of VSS support as a key argument against VMware; this tackles that issue head-on. Also surf over to Duncan's site and see his post about [enabling VSS snapshot support](http://www.yellow-bricks.com/2008/07/26/vss-snapshots/) in VMware Tools.

* Users can now hot extend a virtual disk (extend a virtual disk while the VM is running).

* Users can clone a virtual machine while it is up and running (live cloning). There is now no need to shut down a VM in order to clone it. I suspect this functionality will have some very interesting repercussions from an operational perspective, and may serve as the basis for future functionality as well.

* VMware now officially introduces Enhanced VMotion Compatibility (EVC), which leverages Intel FlexMigration and AMD-V Extended Migration support. This functionality automatically configures CPUs within a cluster to be VMotion-compatible and won't allow you to add hosts to a cluster that can't be configured via EVC to be compatible.

This doesn't even touch on any of the other numerous features that are supported. Again, go check the Release Notes or one of the linked blogs above for complete details.

The introduction of new features that reduce service interruption---namely, hot extending virtual disks and live VM cloning---is exactly the move that VMware needs to take to further differentiate their virtualization solution from competitors' solutions. I've stated time and time again that innovation in the virtualization space will continue to set VMware apart from the competition.
