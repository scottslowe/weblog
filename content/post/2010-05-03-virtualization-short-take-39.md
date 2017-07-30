---
author: slowe
categories: Information
comments: true
date: 2010-05-03T16:00:53Z
slug: virtualization-short-take-39
tags:
- EMC
- HP
- HyperV
- Microsoft
- Virtualization
- VMware
title: 'Virtualization Short Take #39'
url: /2010/05/03/virtualization-short-take-39/
wordpress_id: 1912
---

Welcome to Virtualization Short Take #39! This is my latest (as of May 3, 2010) collection of virtualization-related articles, links, and thoughts. I hope you find something useful buried in this random collection of bits of information I've stumbled across over the past few weeks.

* Dave Lawrence aka "the VMguy" had a recent post on [Changed Block Tracking and why you (should) care](http://vmguy.com/wordpress/index.php/archives/1351). The difference that CBT makes in backups, replication, and other storage-related tasks can be notable, but remember that you'll need to upgrade your VMs to VM hardware version 7 first.

* If you running HP Virtual Connect with VMware vSphere, be sure to check out [this post about a potential failover failure](http://www.virtualtroll.com/?p=368). According to the post, the problem can be resolved by running newer versions of the HP Virtual Connect firmware, the NIC driver, and the NIC bootcode; see the article for the full details.

* Have you been visiting the [Everything VMware at EMC community](https://community.emc.com/community/connect/everything_vmware?view=overview)? If you're like me, RSS feeds for areas like this are invaluable. So, here's a page with [all the RSS feeds](https://community.emc.com/community/feeds/tags/?community=2566) for the Everything VMware at EMC community. Enjoy!

* In [Part 3](http://blogs.technet.com/virtualization/archive/2010/04/07/dynamic-memory-coming-to-hyper-v-part-3.aspx) of a series of posts about Hyper-V's dynamic memory feature, Jeff Woolsey continues to methodically lay out Microsoft's position on advanced memory technologies and their use in virtualized environments. (There's also a [Part 4](http://blogs.technet.com/virtualization/archive/2010/04/21/dynamic-memory-coming-to-hyper-v-part-4.aspx), which is a follow-up/Q&A from Part 3.) Jeff provides some great technical information in this post, but to be honest I'm ready for him to just lay out exactly how dynamic memory is going to work.

* In the articles mentioned above, Jeff mentions that Address Space Layout Randomization (ASLR) should have a negative impact on VMware's transparent page sharing (TPS). Matt Liebowitz decided to test it and [posted the results of his testing](http://blogs.kraftkennedy.com/index.php/2010/04/26/effect-of-aslr-on-transparent-page-sharing-in-vmware-vsphere/). Turns out that---right now, anyway---there is no measurable difference in memory savings due to ASLR.

* If you've been hiding under a rock for a while, you might not have seen the news that VMware [finally released the vSphere 4.0 security hardening guide](http://blogs.vmware.com/security/2010/04/vsphere-40-hardening-guide-released.html). Now you know.

* Dave Rose posted a [guide on how to incorporate support](http://drcs.ca/blog/?p=181) for the VMXNET2 and VMXNET3 NICs into a PXEBOOT/Kickstart environment. It goes a bit deep for me (I'm not a Linux expert, just a tinkerer), but it appears to be good information.

* Anyone tested Tom Howarth's [instructions on how to remove FT](http://planetvm.net/blog/?p=813) from a host without vCenter?

* In reviewing the weekly VMware KB digest, I found a few interesting articles published this past week. The one that really caught my eye, though, was [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1020808) that provides additional information on the NIC configuration maximums for ESX/ESXi 4.0 and 4.0 U1.

* Jason Thomasser wrote up a good post on [how to install ESX4 from USB using unetbootin](http://jthomasser.wordpress.com/2009/08/10/install-esx-4-from-usb-using-unetbootin/).

* Joe Onisick has a good [write-up about the underlying technologies](http://definethecloud.wordpress.com/2010/05/01/hp-flex-10-cisco-vic-and-nexus-1000v/) used in HP Virtual Connect Flex-10, the Cisco Virtual Interface Controller (VIC), and the Cisco Nexus 1000V.

* I also wanted to highlight a few "best practices" documents that I spotted recently in the VMware Communities. These are not new documents by any stretch of the imagination, but they are useful for people who are newer to the virtualization scene. First is this [SQL Server best practices document](http://communities.vmware.com/docs/DOC-8964), prepared by the well-known performance guru Scott Drummonds. Also by Scott Drummonds is this [web server best practices document](http://communities.vmware.com/docs/DOC-5502) (plus an [IIS-specific document](http://communities.vmware.com/docs/DOC-5504) and an [Apache-specific document](http://communities.vmware.com/docs/DOC-5503)). There's also a [best practices document for Lotus Domino](http://communities.vmware.com/docs/DOC-9671) and a [best practices document for Oracle](http://communities.vmware.com/docs/DOC-5505).

Well, that's it this time around. Feel free to share any useful links or posts in the comments below (courteous comments are always welcome).
