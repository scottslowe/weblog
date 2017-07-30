---
author: slowe
categories: Information
comments: true
date: 2008-03-03T22:26:24Z
slug: virtualization-short-take-3
tags:
- Exchange
- Microsoft
- Virtualization
- VMware
title: 'Virtualization Short Take #3'
url: /2008/03/03/virtualization-short-take-3/
wordpress_id: 649
---

It's that time again, folks...time for another Virtualization Short Take!

* VMware's performance team [published some interesting information](http://blogs.vmware.com/performance/2008/02/16000-exchange.html) on running Exchange Server 2007 on VMware ESX Server 3.5. 16,000 mailboxes on 1 physical server, eh? Now all we need is for Microsoft to actually support running Exchange Server on top of ESX Server and we'll be good to go.

* Speaking of vendor support, Simon Gallagher at [vinf.net](http://vinf.net/) has started [an open discussion of vendor support](http://vinf.net/2008/02/21/support-for-virtualized-osapplications-an-open-debate/) for virtualization. This is truly a difficult sticking point for many organizations. Visionary vendors, like SAP, will fully support their solutions on virtualized platforms, but others---which I won't mention by name but are based in the Pacific Northwest area of the United States---won't. At least, they won't until they need to build market share for their own virtualization solution, and then they'll only support their own platform. There is no easy answer to the dilemma that Simon brings to light, but I am interested in the same question as Simon: what is everyone out there doing about vendor support?

* vinternals.net has discovered that the 2.5 version of the VI Client finally [supports passthrough authentication](http://www.vinternals.com/2008/02/virtualcenter-25-passthrough.html), including---if you are willing to edit the vpxd.cfg file---Kerberos support. Good information!

* The team over at [xtravirt.com](http://www.xtravirt.com/) has [updated their information on vimsh](http://www.xtravirt.com/index.php?option=com_remository&Itemid=75&func=startdown&id=21), a woefully underdocumented but extremely useful command-line utility. This is must-have information for ESX Server engineers and architects.

* Chris Hoff has [responded](http://rationalsecurity.typepad.com/blog/2008/02/vmwares-vmsafe.html) with [some thoughts](http://rationalsecurity.typepad.com/blog/2008/03/vmwares-vmsafe.html) on VMsafe, the new set of security APIs that VMware announced at VMworld Europe last week. Chris, along [with others](http://gregness.wordpress.com/2008/01/11/dispelling-virtsec-myths/), has been trumpeting the need for a sea change in security to accommodate the changes wrought by virtualization. In his words, it looks like the security vendors have been given another chance at life. Let's hope they don't blow this one.

* Many of you are probably aware that [not all virtualization solutions support memory page sharing](http://dcsblog.burtongroup.com/data_center_strategies/2007/06/virtualization_.html). The implications of supporting or not supporting memory page sharing may be greater than you think, though; have a look at this [analysis](http://vmmba.com/2008/01/03/why-does-oversubscription-matter.aspx).

* OK, take a deep breath here and don't faint, but I'm getting ready to do something I don't normally do: _I'm going to defend Microsoft._ That's right. [Via VMblog](http://vmblog.com/archive/2008/03/03/hyper-v-s-slow-guest-installation-explained.aspx), I was turned on to some apparent controversy about VM performance under Hyper-V during guest OS installation. As Ben Armstrong aka "Virtual PC Guy" [explains at his site](http://blogs.msdn.com/virtual_pc_guy/archive/2008/02/26/hyper-v-and-slow-guest-os-installation.aspx), this is due to the use of emulated drivers during the installation process. People, this is no different than VMware. Before you install VMware Tools, VMware uses emulated drivers as well. Perhaps VMware's emulated drivers are a bit more efficient than Hyper-V's at this point, but Hyper-V is still in beta. And which would you rather have---highly optimized "synthetic drivers" (equivalent to VMware's paravirtualized drivers) or efficient emulated drivers? Personally, I'll take the second. So give Microsoft and Hyper-V a break for once. Save your energy for after the product is released.

* Information on [booting VMs from an iSCSI LUN under Hyper-V](http://www.activewin.com/awin/comments.asp?HeadlineIndex=42769) is posted here. I don't know that I would actually call that booting from an iSCSI LUN, since to the VM it's a local drive, but I guess it all depends on your perspective.

That about wraps it up for now. As always, your thoughts, corrections, rants, and raves are welcome below.
