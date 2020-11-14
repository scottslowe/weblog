---
author: slowe
categories: Information
comments: true
date: 2011-12-11T23:51:23Z
slug: technology-short-take-18
tags:
- HyperV
- macOS
- Networking
- Storage
- UCS
- vCloud
- VDI
- Virtualization
- vSphere
- OpenFlow
- VLAN
title: 'Technology Short Take #18'
url: /2011/12/11/technology-short-take-18/
wordpress_id: 2500
---

Welcome to Technology Short Take #18! I hope you find something useful in this collection of networking, OS, storage, and virtualization links. Enjoy!

## Networking

The number of articles in my "Networking" bucket continues to overflow; I have so many articles on so many topics (soft switching, OpenFlow, Open vSwitch, MPLS) that it's hard to get my head wrapped around all of it. Here are a few posts that stuck out to me:

* Ivan Pepelnjak has [a very well-written post](http://blog.ioshints.info/2011/12/decouple-virtual-networking-from.html) explaining the various ways that virtual networking can be decoupled from the physical network.

* I stumbled across a trio of articles by Denton Gentry on hash tables ([part 1](http://codingrelic.geekhold.com/2011/06/hash-tables-versus-cams.html), [part 2](http://codingrelic.geekhold.com/2011/06/care-and-feeding-of-hash-engines.html), and [part 3](http://codingrelic.geekhold.com/2011/06/ternary-hashing.html)). This is an interesting perspective I hadn't considered before; as we move more into software-defined networks (SDNs), why are we continuing to use the same mechanisms as before? Why not take advantage of more efficient mechanisms as part of this transition?

## Servers/Operating Systems

* Nigel Poulton and I traded a few tweets during HP Discover Vienna about SCSI Express (or SCSI over PCIe, SoP). He wrote up his thoughts about SoP and its future in the storage industry [here](http://blog.nigelpoulton.com/ive-seen-the-future-of-ssd-arrays/). Further Twitter-based discussions about fabrics led him to say that [HP buying Xsigo would bring the competition back against UCS](http://blog.nigelpoulton.com/xsigo-would-seriously-up-hps-game/). I'm not so sure I agree. Xsigo's server fabric technology/product is interesting, but it seems to me that it's still adding layers of abstraction that aren't necessary. As SR-IOV, MR-IOV, and PCIe extension matures, it seems to me that Ethernet as the fabric is going to win. If that's the case, and HP wants to bring the hurt against UCS, they're going to have to invest in Ethernet-based fabrics.

* Speaking of UCS, here's a "how to" on [deploying the UCS Platform Emulator on vSphere](http://www.vspecialist.co.uk/deploying-cisco-ucs-platform-emulator-2-0-on-vsphere/). You might also like [the UCS PE configuration follow-up post](http://www.vspecialist.co.uk/configuring-cisco-ucs-platform-emulator-v2-0-on-vsphere/).

* Here's what looks to be a handy Mac OS X utility to track [how long until your Active Directory password expires](http://yourmacguy.wordpress.com/adpassmon/). Sounds simple, yes, but useful.

## Storage

* Andre Leibovici bucks the "common wisdom" and recommends that [EFDs should be used for the linked clones, not the base replica](http://myvirtualcloud.net/?p=2513). I can't really argue with his recommendation; it's based on a pretty thorough analysis of I/O patterns against a VMware View environment. It's a great article for any  architect attempting to design a storage solution for a View environment.

* Andre also conducts a good [analysis of the effectiveness of FAST Cache in VDI environments](http://myvirtualcloud.net/?p=2502) that'a also worth a read.

* A "black box" for storage? Interesting idea---more information is available [here](http://www.virtualpro.co.uk/2011/12/07/recoverpoint-and-axxana-async-replication-with-zero-data-loss/).

## Virtualization

* Jason Boche, after some collaboration with [Bob Plankers](http://lonesysadmin.net/), wrote up a good procedure for [expanding the vCloud Director Transfer Server storage space](http://www.boche.net/blog/index.php/2011/12/05/expanding-vcloud-director-transfer-server-storage/). It's definitely worth a read if you're going to be working with vCloud Director.

* Microsoft has [released version 3.2 of the Linux Integration Services for Hyper-V](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/12/02/linux-integration-services-version-v3-2-for-hyper-v-now-available.aspx). The new release adds integrated mouse support, updated network drivers, and fixes an issue with SCVMM compatibility.

* Julian Wood, who I had the opportunity to meet in Copenhagen at VMworld 2011, has published a four-part series on managing vSphere 5 certificates. Follow these links for the series: [part 1](http://www.wooditwork.com/?p=2668), [part 2](http://www.wooditwork.com/?p=2671), [part 3](http://www.wooditwork.com/?p=2674), and [part 4](http://www.wooditwork.com/?p=2677).

* Thinking of deploying Oracle on vSphere? You should probably read this three-part series from VMware's Business Critical Applications blog: part 1 is [here](http://blogs.vmware.com/apps/2011/12/oracle-databases-on-vmware-vsphere.html), part 2 is [here](http://blogs.vmware.com/apps/2011/12/oracle-databases-on-vmware-vsphere-part2.html), and part 3 is [here](http://blogs.vmware.com/apps/2011/12/oracle-databases-on-vmware-vsphere-part3.html).

* I'm so used to dealing with VLANs in a vSphere environment, I didn't consider the challenges that might come up when using them with VMware Workstation. Fortunately, this author did---read his post on [mapping VLANs to VMnets in VMware Workstation](http://brandonjcarroll.com/using-vlans-with-vmware-workstation/).

* I thought that this article on [virtual disks with business critical applications](http://blogs.vmware.com/apps/2011/11/using-virtual-disks-for-business-critical-apps-storage.html) would be a deep dive on which virtual disk formats (thin, lazy zeroed, eager zeroed) are best suited for various applications. While the article does discuss the different virtual disk formats, unfortunately that's as far as it goes.

* Fellow _VMware vSphere Design_ co-author Forbes Guthrie highlights an important design concern with AutoDeploy: what about a virtual vCenter instance? Read [his full article](http://www.vreference.com/2011/12/05/auto-deploy-design-concern/) for the in-depth discussion.

* [This post](http://www.virtuallyghetto.com/2011/11/when-do-vsphere-morefs-change.html) by William Lam gives a good overview of when vSphere MoRefs change (or don't change).

* Here's a good explanation [why NIC teaming can't be used with iSCSI binding](http://blogs.vmware.com/vsphere/2011/12/nic-teaming-iscsi-binding.html).

* Cormac Hogan also [posted a nice overview](http://blogs.vmware.com/vsphere/2011/12/nice-vmkfstools-feature-for-extents.html) of some new `vmkfstools` enhancements in vSphere 5.

* Terence Luk posts [a detailed procedure](http://terenceluk.blogspot.com/2011/12/recovery-of-vmware-srms-site-recovery.html) to help recover VMware Site Recovery Manager in the event of a failure of one of the SRM servers. Good information---thanks Terence!

And that's it for this time around. Feel free to add your thoughts in the comments below---all comments are welcome! _(Please provide full disclosure of vendor affiliations/employment where applicable. Thanks!)_
