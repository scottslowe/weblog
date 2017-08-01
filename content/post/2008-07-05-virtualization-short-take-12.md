---
author: slowe
categories: Information
comments: true
date: 2008-07-05T23:58:53Z
slug: virtualization-short-take-12
tags:
- Apple
- Cisco
- Citrix
- HyperV
- iSCSI
- Networking
- SAN
- Storage
- Virtualization
- VMware
- VMwareHA
- Xen
title: 'Virtualization Short Take #12'
url: /2008/07/05/virtualization-short-take-12/
wordpress_id: 754
---

Here's Virtualization Short Take #12, a collection of links I've gathered over the last week or so and my thoughts on them. Enjoy!

* For those that missed it in the [Release Notes](http://www.vmware.com/support/vi3/doc/vi3_esx35u1_vc25u1_rel_notes.html), VMware added support for Storage VMotion and 10Gb Ethernet with iSCSI SANs, as outlined in [this VI Team blog entry](http://blogs.vmware.com/vi/2008/06/storage-vmotion.html). I went back and reviewed the Release Notes and didn't see this listed anywhere, so this is news to me. Of course, I already knew that Storage VMotion _worked_ just fine with iSCSI, but this added formal support for iSCSI.

* Virtualfuture.info published some good recommendations for [running Citrix in a VI3 environment](http://virtualfuture.info/2008/07/citrix-on-vi3x-recommendations/). If you run Citrix Presentation Server...er, XenApp...in a VI3 environment, these tuning tips may prove quite handy.

* VMware's Virtual Reality blog posted an entry on [some of the architectural advantages of VMware Infrastructure](http://blogs.vmware.com/virtualreality/2008/06/a-look-at-some.html) in comparison to the two leading competitors, Xen (any Xen-based solution) and Hyper-V. Many of the things listed as advantages by VMware are severe points of contention with the other vendors, such as the direct vs. indirect I/O model. Ultimately, time will tell which model was the best; I honestly don't know enough about the deep dark internals to really state which is better. One thing I _am_ glad to see pointed out is the true comparison of hypervisor sizes; Microsoft can say all they want that Hyper-V is only 600K in size and therefore is the "thinnest" hypervisor, but the truth of the matter is that Hyper-V can't run without Windows Server 2008 in the parent partition. As a result, it doesn't really matter how "thin" Hyper-V is, does it?

* Via [Mike Laverick](http://www.rtfm-ed.co.uk/?p=561), I learned that Microsoft may have brought up the whole 64-bit hypervisor vs. 32-bit hypervisor argument yet _again._ Mike used a snippet from [this Microsoft Virtualization Team Blog entry](http://blogs.technet.com/virtualization/archive/2008/07/01/Top-5-things-to-know-about-Hyper_2D00_V.aspx); in reading it myself, I don't get quite the same 64-bit vs. 32-bit that Mike picked up. That's good, because I didn't want to have to [go there][1] again. Personally, the tone I picked up from the whole article was one of educating people far too accustomed to Virtual Server/VirtualPC and trying to educate them on how Hyper-V is different.

* Virtualization analyst Chris Wolf recently posted an entry in which he questioned if [Apple would capitalize on the opportunity that virtualization is creating](http://dcsblog.burtongroup.com/data_center_strategies/2008/06/apple---opportu.html). It's an interesting scenario, one that is similar to a scenario that I discussed a couple of years ago in a piece titled ["Application Agnosticism."][2] In that article, I suggested that seamless host-guest interactions with virtualization software (now implemented by VMware as Unity and by Parallels as Coherence) would usher in a new wave of computing. I suggested that Mac OS X was ahead of the curve because of its ability to run native OS X applications, UNIX applications, X11 applications, Windows applications via WINE (or the commercial variant CrossOver Office), and applications from any other operating system via virtualization. Sounds like I may have been a bit ahead of my time!

* Chad continues discussing VMware HA with another post on [some additional configuration options for HA](http://virtualgeek.typepad.com/virtual_geek/2008/06/arghhh-oh-that.html). Also check out the comments with links to even more information on HA's advanced configuration options.

* [This VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1003973&sliceId=1&docTypeID=DT_KB_1_1&dialogID=12474855&stateId=1%200%2012476549) has some good information on getting LUN identification information. The breakdown of the command-line output from esxcfg-mpath is particularly helpful (and for that reason I've added it to [my del.icio.us bookmarks](http://del.icio.us/slowe)).

* Rich of VM /ETC shares with us a "Doh!" moment he had when he saw [this simple method for identifying VMs with snapshots](http://vmetc.com/2008/06/25/search-for-vm-snapshots-from-the-service-console/). Sometimes it's the simplest solutions that evade us the longest. Here's what I want to know: Aaron, what exactly does "/HEADDESK" mean, anyway?

* [This article at SearchNetworking.com](http://searchnetworking.techtarget.com/tip/0,289483,sid7_gci1317936,00.html) brings to light some of the challenges networking professionals face with server virtualization. I do agree with one point made in the article regarding the mapping of applications---what the end users really care about---to the networking infrastructure. VMware's support for CDP in recent versions of VMware Infrastructure is a step in the right direction, but there is still more work to do for sure. I'm not so sure about the rest of the points in the article, but I may be an exception to the norm; I was a CCNA for a while (on track for CCNP) and have done my fair share of Cisco configurations, so I'm no stranger to the networking world. The use of VLANs to ease configuration in a server virtualization environment seems just second nature to me. Also, I did note that the author indicated that "server administrators sometimes inappropriately configure the switches to create a loop" (referring to vSwitches in ESX). How exactly does that happen? I've never seen a way to link two vSwitches together without using a VM.

As always, readers' thoughts are welcome in the comments!

[1]: {{< relref "2008-02-20-a-few-thoughts-on-xen.md" >}}
[2]: {{< relref "2006-12-26-application-agnosticism.md" >}}
