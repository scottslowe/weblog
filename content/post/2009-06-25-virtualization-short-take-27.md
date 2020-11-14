---
author: slowe
categories: Information
comments: true
date: 2009-06-25T10:11:35Z
slug: virtualization-short-take-27
tags:
- Cisco
- ESXi
- Fusion
- SAN
- Security
- Virtualization
- VMware
title: 'Virtualization Short Take #27'
url: /2009/06/25/virtualization-short-take-27/
wordpress_id: 1423
---

Here's Virtualization Short Take #27, a collection of news, tidbits, thoughts, articles, and useless trivia I've gathered over the last week or so. Perhaps you'll find a diamond in the rough among these items!

* Interested in more information on how it is, exactly, that Cisco is going to provide so much memory in their UCS blades and rack mount servers to make them ideal virtualization hosts? This [article from CommsDesign](http://www.commsdesign.com/article/printableArticle.jhtml?articleID=217700103) and this ["Catalina" article by Rodos Haywood](http://rodos.haywood.org/2009/06/nehalem-memory-with-catalina.html) both provide some great information on how Cisco is working around the Intel reference architecture limitations introduced with the Xeon 5500 and Quick Path Interconnect (QPI).

* This article provides a handy reference on [how to unregister the Nexus 1000V vCenter Server plug-in](http://malaysiavm.com/blog/how-to-remove-cisco-nexus-1000v-plugin/). I wish I'd known this information several weeks ago...

* Need to view some configuration files on an ESX host? Just browse to `http://<IP address of ESX server>/host` and you're all set. I learned of this handy little trick [via Virtual Foundry](http://virtualfoundry.blogspot.com/2009/06/another-http-file-trick.html).

* And speaking of handy little tips, here's [one Eric Sloof shared](http://www.ntpro.nl/blog/archives/1153-VMware-vCenter-Operational-Dashboard.html) regarding the vCenter Ops Dashboard. Again, just browse over to `http://<IP address of vCenter Server>/vod/index.html` to view the vCenter Ops Dashboard.

* Adam Leventhal [describes](http://blogs.sun.com/ahl/entry/ss_7000_simulator_update_plus) using the latest version of VirtualBox---which now includes OVF support and host-only networking---to run the Sun Storage 7000 Simulator. This is pretty cool stuff. I hope Oracle doesn't kill it like Virtual Iron...

* I just mentioned Virtual Foundry a bit ago, but forgot to mention this great post on [hardening the VMX file](http://virtualfoundry.blogspot.com/2009/04/hardening-vmx-file.html). Good stuff.

* For others who are, like myself, pursuing the elusive VMware Certified Design Expert (VCDX) certification, Duncan's recent post describing [the VCDX design defense](http://www.yellow-bricks.com/2009/06/16/vcdx-defense-the-blog-article/) is a great resource. Thanks, Duncan!

* The VMware networking team addresses some questions around [using VMware for virtualized DMZs](http://blogs.vmware.com/networking/2009/06/lets-talk-security-dmzs-vlans-and-l2-attacks.html), and how to protect against Layer 2 attacks.

* Want to do manual linked clones in VMware Fusion? [Here's how](http://communities.vmware.com/docs/DOC-5611).

* Via [Matt Hensley](http://matthensley.wordpress.com/2009/06/22/best-practices-for-vmware-data-recovery-vdr/), I found [this VIOPS document](http://viops.vmware.com/home/docs/DOC-1551) on configuring a VMware vCenter Data Recovery dedupe store.

* [This article](http://solori.wordpress.com/2009/05/22/preview-install-esxi-4-0-to-flash/) has more information on installing ESXi 4.0 to a flash drive, a process I have yet to try. (Instructions for [burning ESXi 3.5 to a flash drive][1] can be found here.) Anyone else done it yet? I'd be interested in how it went.

* If you have any questions about SAN multipathing, Brent Ozar's two part series on the topic may help straighten things out (here's [Part 1](http://www.brentozar.com/archive/2009/05/san-multipathing-part-1-what-are-paths/) and [Part 2](http://www.brentozar.com/archive/2009/05/san-multipathing-part-2-what-multipathing-does/)). I'm not sure that I agree with Brent's statement regarding the ability of desktop-class SATA drives to saturate 4Gbps Fibre Channel, but I'm no storage expert so I could very well be wrong.

* VMware SE and friend Aaron Sweemer provides [a handy script that can help fix Service Console networking](http://www.virtualinsanity.com/index.php/2009/05/27/scripted-esx-installation-reconfiguring-cos-networking-with-kickstart/) when performing automated builds of VMware ESX.

That wraps it up for this edition of Virtualization Short Takes. Feel free to share thoughts, questions, or corrections in the comments, and thanks for reading!

[1]: {{< relref "2009-01-08-creating-a-bootable-esxi-usb-stick-on-mac-os-x.md" >}}
