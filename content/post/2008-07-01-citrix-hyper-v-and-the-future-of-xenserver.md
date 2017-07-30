---
author: slowe
categories: Musing
comments: true
date: 2008-07-01T20:54:15Z
slug: citrix-hyper-v-and-the-future-of-xenserver
tags:
- Citrix
- HyperV
- Microsoft
- Virtualization
- Xen
title: Citrix, Hyper-V, and the Future of XenServer
url: /2008/07/01/citrix-hyper-v-and-the-future-of-xenserver/
wordpress_id: 753
---

Yesterday, Brian Madden wrote an interesting editorial about how [he thinks that Citrix will drop the Xen hypervisor](http://www.brianmadden.com/blog/BrianMadden/Prediction-Citrix-will-drop-the-open-source-Xen-hypervisor-for-Hyper-V) in favor of Hyper-V, and will essentially "port" XenServer to run on Hyper-V. Keith Ward at Virtualization Review picked up on this in [his post titled "The End of Xen?"](http://virtualizationreview.com/blogs/weblog.aspx?blog=2331). Today, Brian posted [a follow-up article](http://www.brianmadden.com/blog/BrianMadden/Citrix-XenServer-is-here-to-stay) clarifying that he wasn't talking about XenServer, but the open source Xen hypervisor.

Architecturally speaking, the commercial XenServer product and the open source Xen hypervisor are inextricably linked to each other. I don't see how it would even be possible for Citrix to "port" XenServer, which is a Linux dom0/parent partition plus an "enhanced" build of the Xen hypervisor, to run on Windows Server 2008, or even to use Microsoft's hypervisor. Keith addresses this point in his article:

>I'm not sure what Brian's sources are on that, but I've talked to people in the know for both Microsoft and Citrix, and they state that although the two hypervisors interoperate very well, that they are not duplicates, or near duplicates, of each other. They were developed entirely separately, but there is a common perception, in fact, that Hyper-V is based upon Xen. Not true.

It's probably pertinent to clarify some architectural issues at this point. (Experts and gurus, feel free to correct me if I am wrong.) Both XenServer (and non-commercial Xen implementations) as well as Hyper-V _must_ have the parent partition present in order to function; they cannot function alone. This is because critical functions like networking and storage are routed through the dom0/parent partition. Without dom0 (a Linux instance for XenServer and non-commercial Xen implementations) or the parent partitions (Windows Server 2008 for Hyper-V), the hypervisor has no I/O functionality. This means that Xen is very closely tied to Linux, and Hyper-V is very closely tied to Windows. Making either run with the other would be a _monumental_ task, if it's even possible. I could be wrong; while these two products share some architectural similarities, they still seem worlds apart to me.

So, in my mind, the idea of Citrix dropping the use of the open source Xen hypervisor---or any commercial variants of the hypervisor---in favor of Hyper-V are so far-fetched so as to be nonexistent.

Now, that's not to say that Citrix won't try to provide some enhanced functionality for Hyper-V, such as live migration (what they call XenMotion and what VMware calls VMotion). This is a key feature that is missing from the initial release of Hyper-V. Is this even possible, though? If it is possible, is it worthwhile? Microsoft has already publicly stated on multiple occasions that live migration will come to Hyper-V in a future release. Why spend a great deal of time, money, and development cycles adding functionality that Microsoft is planning on building anyway?

It's also very possible, even likely, that Citrix will expand their XenDesktop offering to encompass virtual machines hosted on Hyper-V, thus combining their application/desktop delivery expertise with Microsoft's hypervisor and virtualization management capabilities. Now _that's_ quite a possibility, in my opinion. This would be just another example of how Citrix has survived over the years by plugging the gaps in Microsoft's product line, this time offering significant and beneficial desktop virtualization functionality to Hyper-V environments.
