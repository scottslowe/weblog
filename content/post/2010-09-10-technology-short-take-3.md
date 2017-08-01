---
author: slowe
categories: Information
comments: true
date: 2010-09-10T10:00:00Z
slug: technology-short-take-3
tags:
- FCoE
- HyperV
- Microsoft
- Networking
- Virtualization
- Storage
- vSphere
- Cisco
- UCS
title: 'Technology Short Take #3'
url: /2010/09/10/technology-short-take-3/
wordpress_id: 2099
---

Welcome to Technology Short Take #3, a collection of links about key data center technologies like virtualization, networking, and storage. I'm still striving to broaden the scope of these posts to include even more storage and networking posts, so I'd love to hear feedback from readers on how well I'm doing and what other sources I should consider for inclusion here.

But enough of that for now; on with the content!

* Priority Flow Control (PFC) is an as-yet-unratified IEEE standard (IEEE 802.1Qbb) that is often linked closely to Fibre Channel over Ethernet (FCoE). If you're interested in getting a bit more information on PFC and you're not (yet) a networking expert, this [introduction to 802.1Qbb](http://blog.ioshints.info/2010/09/introduction-to-8021qbb-priority-flow.html) is pretty handy.

* Didier Pironet recently documented some of [vSphere 4.1's advanced iSCSI settings](http://deinoscloud.wordpress.com/2010/08/19/vsphere-4-1-iscsi-advanced-settings-and-their-meanings/). Good information, although what would be _really_ handy was any recommendations around whether changing any of these settings should be considered and in what environments a change might be recommended. A future post, Didier?

* Ben Armstrong (aka "The Virtual PC Guy") has posted two articles so far dealing with how to script Hyper-V's dynamic memory. Part 1 shows [how to read the dynamic memory configuration](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/07/scripting-dynamic-memory-part-1-reading-the-configuration.aspx); part 2 shows [how to display the current usage information](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/08/scripting-dynamic-memory-part-2-displaying-current-usage-information.aspx). Ben also recently published an article on [parent memory reserve](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/03/parent-memory-reserve-with-dynamic-memory.aspx), which is how Hyper-V reserves money for the parent partition running Windows Server 2008.

* This SearchTelecom.com article on [Locator/ID Separation Protocol (LISP)](http://searchtelecom.techtarget.com/tip/0,289483,sid103_gci1518999,00.html) gave me just the introduction I needed to LISP. After you read that article, you can continue your LISP education by visiting [this brief blog post](http://blog.ioshints.info/2010/09/introduction-to-lisp.html) and checking out some of the other linked resources.

* I've written before about multi-hop FCoE ([here][1] and [here][2], for example), but this post on [multi-hop FCoE 101](http://blog.ioshints.info/2010/08/multihop-fcoe-101.html) is a great read and highlights some of the differences in vendor implementations. Based on the article, it's these differences in vendor implementations that often lead to disagreements between the vendors with regard to what's required or not required. (It seems like I'm really digging some of Ivan's stuff recently. I'm going to have to add him to my list of networking RSS feeds!)

* Tried SLES 11 SP1 for VMware yet? Jase McCarty recently took it for a quick spin. Here are [his thoughts](http://www.jasemccarty.com/blog/?p=1037).

* Jason Boche recently [posted a fix](http://www.boche.net/blog/index.php/2010/09/05/unable-to-retrieve-health-data/) for an error with vCenter Service Status in vCenter Server 4.1.

* For those readers who haven't had the opportunity to work with Cisco's Unified Compute System (UCS), there are lots of great bloggers out there writing about it---too many to name, in fact. Kevin Goodman captured a few of them in [this list of Cisco UCS people and blogs](http://blog.colovirt.com/2010/08/20/cisco-ucs-people-and-blogs/). While you're coming up to speed, though, this page from Cisco on [upgrading the BIOS on a Cisco UCS server blade](http://www.cisco.com/en/US/products/ps10280/products_configuration_example09186a0080af4547.shtml) gives you an idea of how the system uses service profiles as the vehicle for almost everything. This similar post on Cisco's web site breaks down the process of [creating a service profile in UCS](http://www.cisco.com/en/US/products/ps10280/products_configuration_example09186a0080af4547.shtml), a topic that I've tackled myself (with a four-part series that starts [here][3]).

* This page [listing the Cisco UCS B-series network adapters](http://www.cisco.com/en/US/prod/ps10265/ps10280/cna_models_comparison.html) shows some "Gen 2"-type cards, such as the M72KR-E Emulex CNA. Did I miss an announcement? Unlike the "Gen 1" cards, the "Gen 2" cards aren't hyperlinked for more information.

* William Lam ([@lamw](http://twitter.com/lamw) on Twitter and elsewhere), whom I had the great pleasure of meeting personally last week while at VMworld 2010, has published what is likely to be the definitive primer on vsish, a largely undocumented utility. Check out the [vsish write-up](http://www.virtuallyghetto.com/2010/08/what-is-vmware-vsish.html) on William's site. I also recently found an older article that William wrote on [how to remove stale targets from vMA](http://www.virtuallyghetto.com/2010/06/how-to-remove-stale-targets-from-vma.html). vMA-related articles are almost like gold these days since all the geeks are needing a new command-line fix for ESXi.

* Working on a VDI environment and want to disable some of the welcome stuff that Windows throws your way? [Check this out](http://www.winhelponline.com/blog/disable-ie8-tour-welcome-screen-runonce-all-users/).

There were a few other links that I collected as well but didn't really have anything to say about them; still, in the event they might prove useful, here they are:

[Krystaltek: What ESX Admins Group?  A Tale of RTSM and AD](http://gregmul.blogspot.com/2010/08/what-esx-admins-group-tale-of-rtsm-and.html)  

[vCenter SRM Automatic Failback Options Using EMC Storage](http://itzikr.wordpress.com/2010/08/05/vcenter-srm-automatic-and-your-failback-options-using-emc-storage/)  

[Support Insider: VMware Snapshots](http://blogs.vmware.com/kb/2010/06/vmware-snapshots.html)  

[Running the VMware vSphere Hypervisor stateless](http://www.ntpro.nl/blog/archives/1558-Running-the-VMware-vSphere-Hypervisor-stateless.html)  

[Best practice in LUN design (VMware Communities)](http://communities.vmware.com/docs/DOC-10990)  

[VMware KB: Using a VNC Client to Connect to Virtual Machines](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&externalId=1246)  

[VMware KB: Do I choose the PVSCSI or LSI Logic virtual adapter on ESX 4.0 for non-IO intensive workloads?](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1017652)  

[VMware ESXi 4.1 does not use whole disk capacity for VMFS3](http://www.claudiokuenzler.com/ithowtos/vmware_esxi_4.1_disk_not_full_used.php)

That should do it this time around. Thanks for reading, and feel free to suggest additional articles or links that you think other readers would find useful.

[1]: {{< relref "2009-08-11-why-no-multi-hop-fcoe.md" >}}
[2]: {{< relref "2010-04-21-thinking-out-loud-what-is-multi-hop-fcoe.md" >}}
[3]: {{< relref "2010-05-05-creating-ucs-service-profiles-part-1-networking-elements.md" >}}
