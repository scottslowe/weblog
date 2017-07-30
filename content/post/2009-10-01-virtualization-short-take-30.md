---
author: slowe
categories: Information
comments: true
date: 2009-10-01T12:39:08Z
slug: virtualization-short-take-30
tags:
- ESX
- ESXi
- iSCSI
- Virtualization
- VMware
- VMwareFT
- vSphere
title: 'Virtualization Short Take #30'
url: /2009/10/01/virtualization-short-take-30/
wordpress_id: 1676
---

Welcome to Virtualization Short Take #30, my irregularly posted collection of links and thoughts on virtualization. I hope you find something useful here!

* I believe Jason Boche already mentioned this on his own blog (I couldn't find a link) and also started [this VMware Communities thread](http://communities.vmware.com/message/1335428) discussing the fact that the 8/6 patch breaks FT compatibility between ESX and ESXi hosts in the same cluster. [This VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1013637) is now available with more information on the problem. I'm hearing from VMware is that there is no short-term solution; the workaround is to use only ESX or only ESXi within a single cluster. (I don't recommend not patching the hosts until the problem is fixed.)

* And while we're talking VMware FT, [here's a good document](http://www.vmware.com/resources/techresources/10058) on VMware FT architecture and performance. (Eric Siebert's [Virtualization Pro blog post about VMware FT](http://itknowledgeexchange.techtarget.com/virtualization-pro/masters-guide-to-vmware-fault-tolerance/) is really good, too.)

* I'm also hearing reports that there are problems mixing ESX and ESXi in the same cluster when using host profiles. Theoretically, you should be able to use an ESX reference host and apply that to ESXi hosts, but in reality it's not working so well.

* If you're using AppSpeed, you'll need to manually turn off the AppSpeed sensor VMs in order to put ESX/ESXi hosts into Maintenance Mode. The sensor VM won't VMotion off the host, so this prevents the host from entering Maintenance Mode.

* Here's another topic that I think has been mentioned elsewhere (looks like Duncan [mentions it here](http://www.yellow-bricks.com/2009/09/14/site-recovery-manager-1-0-update-1-patch-4/)), but SRM 1.0 Update 1 Patch 4 was released a couple of weeks ago and it includes a fit for customizing the IP addresses of Windows Server 2008 guest operating system instances.

* Toward the end of August, VMware Infrastructure 3 support was added for NetApp MetroCluster (see [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?language=en_US&cmd=displayKC&externalId=1001783)). Now, how about some VMware vSphere 4 support?

* Most of you are aware by now (and if you aren't aware, go buy a copy of [my book](http://www.amazon.com/Mastering-VMware-vSphere-Computer-Tech/dp/0470481382/ref=sr_1_1?ie=UTF8&s=books&qid=1254413521&sr=8-1) so you will be aware) that you can use Storage VMotion to change virtual disks from thin provisioned to thick provisioned. The problem is this: the type of thick provisioned disk created when you do this via Storage VMotion is _eagerzeroedthick_, not _zeroedthick_. This means that it is not friendly to storage array thin provisioning!

* I'm still looking for a valid use case for this little trick, but it's mentioned by both Duncan and Eric: the ability to present multiple cores per socket to a virtual machine. Duncan's post is [here](http://www.yellow-bricks.com/2009/06/04/per-processor-licenses-for-your-application/); Eric's post is [here](http://www.vcritical.com/2009/09/use-coreinfo-to-view-vm-core-and-socket-count/). As Eric points out, licensing is one potential use. Anyone have any other valid use cases?

* Eric Sloof has [a great post](http://www.ntpro.nl/blog/archives/1283-vSphere-DvSwitch-caveats-and-best-practices!.html) on dvSwitch caveats and best practices that is definitely worth reading.

* Want to make linked clones work on vSphere? Tom Howarth points out in [this post](http://planetvm.net/blog/?p=777) [some information](http://engineering.ucsb.edu/~duonglt/vmware/vGhettoLinkedClone.html) made available by William Lam. Both articles are worth a look.

* Tom also posted some useful information on [enabling firewall logging](http://planetvm.net/blog/?p=746) on VMware ESX hosts.

* [This post](http://www.virtualinsanity.com/index.php/2009/09/27/capacity-conundrum-part-1/) over on Aaron Sweemer's blog was actually written by guest author John Blessing (aka [@vTrooper](http://twitter.com/vtrooper) on Twitter) and just goes to illustrate how difficult it can be to create a chargeback model.

* Of course, the "Super iSCSI Friends" recently produced a multi-vendor post on using iSCSI with VMware vSphere, a great follow-up to the [original multi-vendor VI3 post](http://virtualgeek.typepad.com/virtual_geek/2009/01/a-multivendor-post-to-help-our-mutual-iscsi-customers-using-vmware.html). Here's Chad's version of the [multi-vendor vSphere and iSCSI post](http://virtualgeek.typepad.com/virtual_geek/2009/09/a-multivendor-post-on-using-iscsi-with-vmware-vsphere.html).

That wraps it up for this time around. Thanks for reading, and feel free to submit any other useful or interesting links in the comments below.
