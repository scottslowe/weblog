---
author: slowe
categories: Information
comments: true
date: 2009-06-05T13:43:06Z
slug: the-return-of-virtualization-short-takes
tags:
- EMC
- HP
- Networking
- Solaris
- Sun
- Virtualization
- VMFS
- VMotion
- VMware
- VMwareHA
- vSphere
title: The Return of Virtualization Short Takes
url: /2009/06/05/the-return-of-virtualization-short-takes/
wordpress_id: 1386
---

My irregular "Virtualization Short Takes" series was put on hold some time ago after I started work on _Mastering VMware vSphere 4_. Now that work on the book is starting to wind down just a bit, I thought it would be a good time to try to resurrect the series. So, without further delay, welcome to the return of Virtualization Short Takes!

* Trigged by a series of blog posts by Arnim van Lieshout on VMware ESX memory management ([Part 1](http://www.van-lieshout.com/2009/04/esx-memory-management-part-1/), [Part 2](http://www.van-lieshout.com/2009/05/esx-memory-management-part-2/), and [Part 3](http://www.van-lieshout.com/2009/05/esx-memory-management-part-3/)), Scott Herold decided to join the fray with [this blog post](http://www.vmguru.com/index.php/articles-mainmenu-62/mgmt-and-monitoring-mainmenu-68/96-memory-behavior-when-vm-limits-are-set). Both Scott's post and Arnim's posts are good reading for anyone interested in getting a better idea of what's happening "under the covers," so to speak, when it comes to memory management.

* Perhaps prompted by my post on upgrading virtual machines in vSphere, a lot of information has come to light regarding the PVSCSI driver. Some are advocating [changes to best practices](http://vmjunkie.wordpress.com/2009/05/18/new-best-practices-for-vsphere/) to incorporate the PVSCSI driver, but others seem to be [questioning the need](http://www.vmwareinfo.com/2009/06/whats-deal-with-new-pvscsi-drivers.html) to move away from a single drive model (a necessary move since PVSCSI isn't supported for boot drives). Personally, I just want VMware to support the PVSCSI driver on boot drives.

* Eric Sloof confirms for us that [name resolution is still the Achilles' Heel](http://www.ntpro.nl/blog/archives/1124-vSphere-HA-and-short-hostnames.html) of VMware High Availability in VMware vSphere.

* I don't remember where I picked up [this VMware KB article](http://kb.vmware.com/selfservice/viewContent.do?externalId=1004901&sliceId=1), but it sure would be handy if VMware could provide more information about the issue, such as what CPUs might be affected. Otherwise, you're kind of shooting in the dark, aren't you?

* Upgraded to VMware vSphere, and now having issues with VMotion? Thanks to [VMwarewolf](http://www.vmwarewolf.com/vmotion-stops-working-in-vsphere/), this pair of VMware KB articles ([here](http://kb.vmware.com/kb/1011294) and [here](http://kb.vmware.com/kb/1011296)) might help resolve the issue.

* Chad Sakac of EMC and co-conspirator for the storage portion of _Mastering VMware vSphere 4_ (pre-order [here](http://www.amazon.com/Mastering-Vmware-Infrastructure-Scott-Lowe/dp/0470481382/ref=sr_1_3/189-1468669-0910930?ie=UTF8&s=books&qid=1241107850&sr=1-3)), has been putting out some very good posts:

  - [More on Exchange on vSphere (including FT)](http://virtualgeek.typepad.com/virtual_geek/2009/05/more-on-exchange-on-vsphere-including-ft.html)

  - [Integrated vSphere enterprise workloads - all together, at scale](http://virtualgeek.typepad.com/virtual_geek/2009/05/integrated-vsphere-enterprise-workloads-all-together-at-scale.html)

  - [Using vSphere and HW offload for improved Celerra VSA performance](http://virtualgeek.typepad.com/virtual_geek/2009/05/using-vsphere-and-hw-offload-for-improved-celerra-vsa-performance.html)

  - [vSphere and 2TB LUNs - changes from VI3.x](http://virtualgeek.typepad.com/virtual_geek/2009/06/vsphere-and-2tb-luns-changes-from-vi3x.html)

* [Leo Raikhman](http://blog.core-it.com.au) pointed me to [this article](http://www.tuxyturvy.com/blog/index.php?/archives/37-Troubleshooting-VMware-ESX-network-performance.html) about IRQ sharing between the Service Console and the VMkernel. I think I've mentioned this issue here before...but after over a 1,000 posts, it's hard to keep track of everything. In any case, there's also a [VMware KB article](http://kb.vmware.com/selfservice/viewContent.do?externalId=1003710&sliceId=2#determine) on the matter.

* And speaking of Leo, he's been putting up some great information too: notes on [migrating Ubuntu servers](http://blog.core-it.com.au/?p=524) (in turn derived from [these notes](http://professionalvmware.com/2009/03/10/ubuntu-cloning-mac-address-change-mayhem/) by Cody at ProfessionalVMware), a [rant on CDP support](http://blog.core-it.com.au/?p=522) in ESX, and [a note](http://blog.core-it.com.au/?p=490) about the EMC Storage Viewer plugin. Good work, Leo!

* If you are interested in a run-down of the storage-related changes in VMware vSphere, check out [this post](http://blog.fosketts.net/2009/04/21/storage-vmware-vsphere-4/) from Stephen Foskett.

* Rick Vanover notes a few changes to the VMFS version numbers [here](http://virtualizationreview.com/blogs/everyday-virtualization/2009/06/vstorage-vmfs-version-notes.aspx). The key takeaway here is that no action is _required_, but you may want to plan some additional tasks after your vSphere upgrade to optimize the environment.

* In [this article](http://www.channelregister.co.uk/2009/04/21/vsphere_storage_controller/), Chris Mellor muses on how far VMware may go in assimilating features provided by their technology partners. This is a common question; many people see the addition of thin provisioning within vSphere as a direct affront to array vendors like NetApp, 3PAR, and others who also provide thin provisioning features in the array themselves. I'm not so convinced that this feature is as competitive as it is complementary. Perhaps I'll write a post about that in the near future...oh wait, never mind, [Chad already did](http://virtualgeek.typepad.com/virtual_geek/2009/04/thin-on-thin-where-should-you-do-thin-provisioning-vsphere-40-or-array-level.html)!

* File [this one](http://vmetc.com/2009/06/03/things-that-make-you-go-hmmmm-vmware-requests-veeam-discontinue-support-for-free-esxi-in-veeam-backup/) away in the "VMware-becoming-more-like-Microsoft" folder.

* My occasional mentions of Crossbow prompted a [full-on explanation](http://blogs.sun.com/sunay/entry/crossbow_virtualized_switching_and_performance) of the Open Networking functionality of OpenSolaris by a Sun engineer. It kind of looks like SR-IOV and VMDirectPath to me...sort of. Don't you think?

* If you are thinking about how to incorporate HP Virtual Connect Flex-10 into your VMware environment, Frank Denneman has [some thoughts to share](http://frankdenneman.wordpress.com/2009/04/26/flex-10-lessons-learned/). I've been told by HP that I have some equipment en route with which I can do some additional testing (the results of which will be published here, of course!), but I haven't seen it yet.

OK, I guess that should just about do it. Thanks for reading, and please share your thoughts, interesting links, or (pertinent) rants in the comments.
