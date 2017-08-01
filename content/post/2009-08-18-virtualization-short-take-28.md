---
author: slowe
categories: Information
comments: true
date: 2009-08-18T20:48:01Z
slug: virtualization-short-take-28
tags:
- Cisco
- Nexus
- UCS
- Virtualization
- VMware
title: 'Virtualization Short Take #28'
url: /2009/08/18/virtualization-short-take-28/
wordpress_id: 1537
---

Welcome to another edition of Virtualization Short Takes. Here are a few links I've gathered over the past few weeks that I found interesting, useful, or that just plain caught my eye. I hope you find something helpful!

* Back in early July Brad Hedlund (of [InternetworkExpert.org](http://www.internetworkexpert.org/)) published this [VMware vSwitch design](http://www.internetworkexpert.org/2009/07/05/cisco-ucs-vmware-vswitch-design-cisco-10ge-virtual-adapter/) of using Cisco UCS with the "Palo" adapter. If you've read any of my UCS articles (here's a [good one][1]), you'll know that the Palo adapter is the adapter that can be carved up (via SR-IOV) into multiple virtual instances. As Brad points out in his article, this allows you to mimic today's multi-NIC implementations using the Palo adapter. This design is interesting, but more interesting is this more recent design incorporating the [Nexus 1000V and the Palo adapter](http://www.internetworkexpert.org/2009/08/11/cisco-ucs-nexus-1000v-design-palo-virtual-adapter/). In fact, I'll be referring to that second article myself, as in that article Brad lays out how to run the Nexus 1000V VSM on top of the VEMs it is managing (an interesting situation I've wrestled with myself). Keep the designs coming, Brad!

* A couple of different sites have provided more information on how to use software iSCSI with multiple paths in VMware vSphere 4. This site has [a good write-up with screenshots](http://goingvirtual.wordpress.com/2009/07/17/vsphere-4-0-with-software-iscsi-and-2-paths/), and Rich Brambley [discusses it on his site](http://vmetc.com/2009/08/12/vswitch-with-multiple-vkernel-portgroups-for-vsphere-iscsi-round-robin-mpio/) as well. I haven't had the opportunity to try this yet myself, but I hope to get the chance soon.

* If you are looking for a decent high-level overview of the Cisco Nexus 1000V, Ted Romer has [just what you need](http://shouldhavegonewithcisco.com/2009/08/04/understanding-the-nexus-1000v/).

* Running your VMware vSphere environment on an EMC Celerra over NFS? You'll want to have a look [here](http://virtualgeek.typepad.com/virtual_geek/2009/08/important-patch-for-celerranfsvmware.html)---there's an important patch you need to install.

* Here's an article on [how to enable port forwarding in NAT mode](http://blog.juliankamil.com/article/27/vmware-fusion-port-forwarding-in-nat-mode) on VMware Fusion. Sweet!

* Are you ready for vSphere, or are you like [this gentleman](http://www.vminstall.com/not-ready-for-vsphere/)? It's a tough position for VMware---many users are happy with ESX 3.5 and don't see an immediate need to upgrade. I'd be interested in hearing how many others are holding off because ESX 3.5 is "good enough."

* Steve Chambers has a good article on [vSphere VMM execution modes](http://viewyonder.com/2009/08/14/v-sphere-vmm-execution-modes-and-cisco-ucs-blades/). I'm not sure what this has to do with Cisco UCS other than the information is related to the Xeon 5500 series of Intel CPUs, but the article is useful nevertheless.

* Did you know about the [VMware Knowledge Base Weekly Digest](http://blogs.vmware.com/kbdigest/)?

* Dave Lawrence aka the VMguy has a [useful post on Microsoft Cluster Services](http://vmguy.com/wordpress/index.php/archives/1019) (MSCS, now known as Windows Failover Clustering I believe) on VMware vSphere. It's worth a read if this particular configuration is something that you need, although I suspect that VMware Fault Tolerance will lessen the need for this configuration somewhat.

* Forbes Guthrie has some [useful VMware vSphere notes here](http://www.vreference.com/2009/06/30/updated-vsphere4-notes/); might be useful for those preparing for/refreshing for the VCP4 beta exams that are taking place at VMworld 2009 in San Francisco.

* Broadening the virtualization scope, John Howard has a good post on [how to remove disabled network adapters from the parent partition](http://blogs.technet.com/jhoward/archive/2009/07/01/hyper-v-how-to-remove-disabled-virtual-network-adapters-from-the-parent-partition.aspx). (Wait a minute, isn't that supposed to be called the management OS?)

* [Here's a good idea](http://www.rtfm-ed.co.uk/?p=1391) brought to light by Mike Laverick. Why didn't VMware think of this?

* Duncan points out a number of useful white papers in [this post](http://www.yellow-bricks.com/2009/08/13/vsphere-cpu-scheduler-whitepaper-this-is-it/). Although the title names only the vSphere CPU scheduler white paper, Duncan also provides links for a white paper on VMM execution modes and FT architecture and performance. Great work, Duncan!

That's all I have this time around for virtualization links. Along the way, I've also run into a few other links that might be useful as well:

[Configuring Virtual Fibre Channel (vFC) Interfaces](http://blog.virtualtacit.com/2009/08/09/nexus-configuring-virtual-fiber-channel-vfcs-interfaces-viva-fcoe/)  

[vPC (Virtual Port Channel) and the Nexus Platform](http://jasonnash.wordpress.com/2009/08/10/vpc-virtual-port-channel-and-the-nexus-platform/)  

[Securing COMSTAR and VMware iSCSI Connections](http://blog.laspina.ca/ubiquitous/securing-comstar-and-vmware-iscsi-connections)  

[HA in Cisco UCS Menlo Card](http://rodos.haywood.org/2009/08/ha-in-cisco-ucs-menlo-card.html)

Feel free to share your thoughts, perspectives, or other interesting/useful links in the comments. Thanks for reading!

[1]: {{< relref "2009-08-02-ucs-class-wrap-up.md" >}}
