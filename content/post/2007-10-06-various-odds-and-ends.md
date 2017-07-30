---
author: slowe
categories: Information
comments: true
date: 2007-10-06T23:43:15Z
slug: various-odds-and-ends
tags:
- ESX
- Kerberos
- LDAP
- Linux
- Microsoft
- NFS
- Security
- Virtualization
- VMware
- Windows
title: Various Odds and Ends
url: /2007/10/06/various-odds-and-ends/
wordpress_id: 556
---

I was going through my list of flagged headlines in NetNewsWire and realized that I'd built up quite a list of articles that I intended to write something about. Some of them just don't merit a full-blown post, though, so I thought I'd just toss a bunch of them in here along with a brief sentence or two about them:

* [VMTN Discussion Forums: vdiskmanager GUI for OSX](http://www.vmware.com/community/thread.jspa?messageID=674493&): An enterprising Fusion user has written an OS X GUI for vdiskmanager, so that VMDKs on Fusion can be expanded or defragmented, or new virtual disks can be created. I haven't tried it yet, but it looks like it could be extremely useful, and it's nice to see Fusion users creating useful utilities like this.

* [Running ESX 3i Beta in a VM with VMware Fusion](http://scalethemind.blogspot.com/2007/09/running-esx-3i-beta-in-vm-with-vmware.html): Still thinking Fusion, this article discusses how a user managed to get ESX Server 3i (the beta version obtained at VMworld 2007) running as a VM under Fusion. There's also information on running [it under Workstation 6](http://www.virtualization.info/2007/06/tech-how-to-run-esx-server-3-on-vmware.html) as well.

* [Tech: How to get the command line in ESX Server 3i beta](http://www.virtualization.info/2007/10/tech-how-to-get-command-line-in-esx.html): Turns out ESX Server 3i has a command line after all, based on [BusyBox](http://www.busybox.net/). Richard Garsthagen has [more information about ESX 3i](http://www.run-virtual.com/?p=196) available at run-virtual.com. Also see Eric Sloof's [info on boot options](http://www.ntpro.nl/blog/archives/233-ESX-3i-Boot-Options.html).

* [Storm Worm Botnet Attacks Anti-Spam Firms](http://www.darkreading.com/document.asp?doc_id=134253): Is this botnet really as massive as everyone says? I've been seeing so many articles about the Storm botnet, but I have yet to see (perhaps I haven't looked hard enough yet) definitive information that describes the type of traffic these bots generate. Surely there's got to be something we can do about this.

* [Microsoft Updates Windows Without User Permission, Apologizes](http://www.darkreading.com/document.asp?doc_id=133901): Oh, goodness---where do I start with this one? Let's just say that I'm glad I'm using Little Snitch, which catches this kind of outbound traffic that so easily slips through the Windows "firewalls" onto the Internet. Otherwise, I might be getting product updates without anyone bothering to tell me so. (And perhaps it's just me, but an apology from Microsoft doesn't make me feel any more trusting of them.)

* [NFS vs iSCSI vs FC](http://playground.sun.com/pub/nfsconf/pres04/suggs.pdf): More information on why we should be interested in running VMware over NFS.

I guess that's all for now, as it's getting late and I have to get up in the morning and go to church. Feel free to share any comments or corrections below. Thanks for reading!
