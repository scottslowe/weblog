---
author: slowe
categories: Information
comments: true
date: 2008-05-26T15:13:46Z
slug: virtualization-short-take-8
tags:
- Citrix
- ESX
- HyperV
- Microsoft
- Snapshots
- SSH
- VDI
- Virtualization
- VMware
- Xen
title: 'Virtualization Short Take #8'
url: /2008/05/26/virtualization-short-take-8/
wordpress_id: 716
---

It's that time again, friends, time for another Virtualization Short Take!

* [OpenSolaris on Fusion](http://blogs.sun.com/souvik/entry/getting_started_with_opensolaris_2008): As expected, Solaris/OpenSolaris fans are experimenting with OpenSolaris on Fusion. Apparently, it runs rather well.

* Brian Madden had [an interesting thought](http://www.brianmadden.com/blog/BrianMadden/Thinstall---Wine---Windows-apps-without-Windows) about Thinstall (now ThinApp) plus WINE to eliminate Windows. In the end, Brian feels like many companies will just want to deal with the larger vendors, and won't be willing to support this kind of "cobbled together" solution. The idea of using ThinApp on WINE on a non-Windows operating system is a pretty cool idea, but it may be a bit early for its time.

* Microsoft Hyper-V [made it to RC1](http://blogs.technet.com/virtualization/archive/2008/05/20/hyper-v-rc1-release-available-on-microsoft-download-center.aspx), apparently ahead of schedule. I wonder if they will try to make RTM in time for TechEd in Florida in June? In addition, Microsoft also [released information](http://blogs.technet.com/virtualization/archive/2008/05/20/msdn-and-technet-powered-by-hyper-v.aspx) about how they are "eating their own dog food" and using Hyper-V for the MSDN and TechNet web sites.

* Citrix has released XenDesktop 2.0, their VDI solution. Alessandro has a [fairly complete breakdown](http://www.virtualization.info/2008/05/release-citrix-xendesktop-20.html) of the components involved in the solution and the various editions under which it will be released. A lot of these components are pre-existing products that are being rebundled into XenDesktop; XenApp (Presentation Server) and Provisioning Server (Ardence) are two examples. VMware came out with a competitive response almost immediately, and Gareth [dissected that response](http://www.dabcc.com/blogs/Gareths-Blog-An-African-Perspective/post/VMware-says-Citrix-XenDesktop-software-is-complex-consisting-of-different-disparate-components-bundled-together) on DABCC. Having not actually installed XenDesktop yet, I don't know how integrated---or not integrated---the various components are, so I'll reserve judgment until later. I have my beefs with VDM; in particular, I don't like how it mandates VM provisioning in order to use pools. I hope that Leostream's [removal of their P>V product](http://www.virtualization.info/2008/05/leostream-drops-p-v-direct.html) as reported by Alessandro doesn't portend dark days for Leostream.

* According to Tony Asaro at Virtual Iron, Citrix's release of XenDesktop [signals the beginning of a "shift" in focus](http://blog.virtualiron.com/Virtual-Discourse/2008/05/citrix_shifting_focus.html) from server virtualization to desktop virtualization. One must consider this comment in the context of who is providing the comment; Virtual Iron is, of course, a competitor in the server virtualization market whose product is also based on the Xen hypervisor. Besides, even if that is true, so what? Citrix has made an existence out of focusing on client-side application delivery. This would be completely logical, in my mind, and would allow Citrix to focus on an area where they are strong instead of competing in a market where they are weak.

* Lou Springer brings us a [method of connecting to a VM's console](http://blog.louspringer.com/2008/05/19/vmware-esx-remote-console-from-os-x/) using VNC over SSH from Mac OS X. I'd seen references to using this with VMware Server, but didn't know that it worked with VI3. Thanks, Lou! (Lou's trick was based on information from this [VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1246&sliceId=1&docTypeID=DT_KB_1_1&dialogID=64867075&stateId=0%200%2064865245), by the way.)

* [From IPMer](http://www.ipmer.com/2008/05/70gb-snapshot-yikes.html), here's some information on using VMware Converter to assist with VM snapshots. This was [picked up by Rich over at VM /ETC](http://vmetc.com/2008/05/21/use-vmware-converter-to-solve-esx-snapshot-issues/) and also included in the first-ever [VMware Communities Roundtable podcast](http://blogs.vmware.com/vmtn/2008/05/vmware-commun-1.html) (which I've downloaded but not yet had the opportunity to actually review yet).

That's it for today. I hope that everyone has a great Memorial Day. Don't forget to thank a veteran or active serviceman/servicewoman for your freedom!
