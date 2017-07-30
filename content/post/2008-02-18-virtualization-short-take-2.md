---
author: slowe
categories: Information
comments: true
date: 2008-02-18T14:30:30Z
slug: virtualization-short-take-2
tags:
- ESX
- Storage
- VCB
- Virtualization
- VMFS
title: 'Virtualization Short Take #2'
url: /2008/02/18/virtualization-short-take-2/
wordpress_id: 635
---

Before I begin the second installation of Virtualization Short Takes, I thought it was interesting to note that Thomas Bishop over at ScaleTheMind.com has [adopted a similar strategy](http://scalethemind.com/2008/02/quick-bits-1/). There's just so much happening that it's truly impossible to discuss everything in depth. Even so, it's often helpful to at least provide the readers with the links and some additional thoughts. It seems like this approach may be the best one to use. I'm certainly open to everyone's thoughts.

So, on to today's list of virtualization-related links:

* Frane Borozan has launched [p2vbackup.com](http://www.p2vbackup.com), a site that describes the process for incorporating virtualization into your backup and recovery process. I haven't had the time to review the site fully yet, but what I've seen looks pretty good.

* Either Duncan Epping at [Yellow Bricks](http://www.yellow-bricks.com/) just has really bad luck, or he has some sort of link into the VMware Knowledge Base so that he knows when new articles are published. If it's the former, then his misfortune is our good fortune, as he's [pointed out a potential problem with Storage VMotion](http://www.yellow-bricks.com/2008/02/13/storage-vmotion-fails-with-error-message-failed-to-unstun-vm-after-disk-reparent/) that can cause the storage migration to fail and the VM will then not power on. The associated [VMware KB article](http://kb.vmware.com/selfservice/dynamickc.do?externalId=1003874&sliceId=1&command=show&forward=nonthreadedKC&kcId=1003874) is also available.

* [Lou Springer](http://blog.louspringer.com) has written a paper on [estimating workload consolidation and placement](http://blog.louspringer.com/2008/02/12/vmware-migration-and-consolidation-without-the-vmware-capacity-planner/) _without_ the use of VMware Capacity Planner. Truth be told, there are organizations that cannot, for whatever reason, leverage Capacity Planner. Lou's document describes some alternative approaches and some ways of mitigating the risks of those alternative approaches.

* Again via Duncan, here's some [good information](http://www.yellow-bricks.com/2008/02/11/vcb-i-forgot-all-about-automount-disable-what-now/) on recovering VMFS partitions when you've forgotten to set "automount disable" on the Windows-based VCB proxy server. It seems like I recall seeing somewhere that automount was disabled by default on the Standard edition of Windows Server 2003, but enabled on Enterprise. Can anyone confirm that? By the way, it looks like Windows Server 2008 will [default to automount enabled](http://technet2.microsoft.com/windowsserver2008/en/library/4635fc91-a477-4f17-8dcc-aa08854bfe451033.mspx?mfr=true).

* And while we're talking about storage, check out [this information](http://www.yellow-bricks.com/2008/02/13/unidentified-flying-lun/) from Duncan on Dell's DRAC Virtual Media functionality and its interaction with VMware ESX Server. Anyone seen similar behavior from HP iLO?

* Via VMblog.com, I saw that Catbird had [announced their HypervisorShield](http://vmblog.com/archive/2008/02/12/catbird-launches-first-ever-dedicated-hypervisor-security-solution.aspx), which "is the first virtualized security technology that can monitor and control access to the hypervisor network". OK, sounds nifty, but I have to [side with Christofer Hoff](http://rationalsecurity.typepad.com/blog/2008/02/catbird-says-it.html) on this one. What exactly is Catbird saying here? Are they protecting the Service Console network interface(s), the VMkernel interface(s), the vSwitches, or something else entirely? Personally, I'm going to wait until I can see more information to make a judgment call on this one.

That's it for this edition. Feel free to submit any thoughts, suggestions, or rants in the comments below. Thanks for reading!

**UPDATE:** My recollection on the status of automount in Windows Server 2003 was incorrect. It is _enabled_ by default in Standard, and _disabled_ by default in Enterprise. Thanks to the readers who helped set me straight!
