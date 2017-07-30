---
author: slowe
categories: Information
comments: true
date: 2012-05-03T13:16:33Z
slug: technology-short-take-22
tags:
- Hardware
- Microsoft
- Networking
- Security
- VDI
- Virtualization
- VMware
- vSphere
- Windows
- MPLS
- Cisco
- UCS
- HyperV
- CLI
title: 'Technology Short Take #22'
url: /2012/05/03/technology-short-take-22/
wordpress_id: 2617
---

Welcome to Technology Short Take #22! Once again, I find myself without too many articles to share with you this time around. I guess that will make things a bit easier for you, the reader, but it does make me question whether or not I'm "listening" to the right communities. If any readers have suggestions on sources of information to which I should be subscribing or I should be following, I'd love to hear your suggestions.

In any case, let's get into the meat of it. I hope you find something useful!

## Networking

* Trying to learn MPLS? (I am.) I found this article on [basic MPLS/VPN on Cisco IOS](http://networkstatic.net/2012/04/11/basic-mplsvpn-using-cisco-ios/) to be helpful.

* If you think your network is appropriately protected against bridging loops, read [this article](http://blog.ioshints.info/2012/04/stp-loops-strike-again.html) by Ivan Pepelnjak.

* Derick Winkworth does [a write-up on Infineta and parallelization](http://packetpushers.net/infineta-and-parallelization-part-1/) and how it enables their data center interconnect (DCI) acceleration platform.

## Security

* I have to agree with Tom Hollingsworth that we often create [backdoors by design](http://networkingnerd.net/2012/01/17/backdoors-by-design/) simply out of our own laziness. I've heard it said---in fact I may have used the statement myself---that no amount of security can fix stupidity. That might be a bit strong, but it does apply to the "shortcuts" that we create for ourselves or our customers in our designs.

## Servers/Hardware

* Kevin Houston (who works for Dell) posted [an article](http://bladesmadesimple.com/2012/04/test-report-power-efficiency-comparison-of-dell-and-cisco-high-memory-capacity-blade-servers/) about a recent test report comparing power usage between Dell blades and Cisco UCS blades. If you're comparing these two solutions, find a comparable report from Cisco and then draw your own conclusions. (Always get multiple views on a topic like this, because every vendor---and I know because I work for a vendor, too---will spin the report in their favor.)

## Virtualization

* Andre Leibovici has a couple of great posts on VMware View. In [this article](http://myvirtualcloud.net/?p=3103), he discusses how CBRC metadata is handled when the RecomputeDigest_Task is invoked. The second article I saw discusses [automating the "Already Used" state in VMware View](http://myvirtualcloud.net/?p=3107).

* Luc Dekens has a post on [how to test if you can unmount a VMFS datastore](http://www.lucd.info/2012/04/15/test-if-the-datastore-can-be-unmounted/).

* Sammy Bogaert has a nice post [summarizing some recommended BIOS settings](http://boerlowie.wordpress.com/2010/09/03/recommended-bios-setting-on-hp-proliant-dl580-g7-for-vsphere/) for HP ProLiant DL580 G7 servers for vSphere.

* Ben Armstrong discusses [storage migration performance](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/04/23/storage-migration-performance.aspx) in the upcoming release of Microsoft Hyper-V. He also discusses some [storage migration configuration settings](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/04/24/storage-migration-hyper-v-settings.aspx) in a separate post.

* Here's a "how to" on [using esxcli to mask a LUN from ESXi](http://www.virtuallanger.com/2012/05/01/using-esxcli-to-mask-a-lun-from-esxi/).

* [This article](http://nathancoutinho.wordpress.com/2012/04/19/windows-8-brings-new-virtualization-benefits/) touts the "benefits" that Windows 8 will bring to virtualized environments, but somehow this seems even more complicated---and more difficult to track---than before. Is it just me?

* Duncan Epping [helps us understand](http://www.yellow-bricks.com/2012/04/25/what-is-das-maskcleanshutdownenabled-about/) the need for the `das.maskCleanShutdownEnabled` setting when we're also taking advantage of the `disk.terminateVMOnPDLDefault` advanced setting (very useful in stretched cluster scenarios).

* Frank Denneman has a good write-up on the considerations around [aggregating multiple storage arrays in a single datastore cluster](http://frankdenneman.nl/2012/04/aggregating-datastores-from-multiple-storage-arrays-into-one-storage-drs-datastore-cluster/).

* Cody Bunch has launched a new series of articles about running vSphere on an OpenCompute platform. Part 1 is [here](http://professionalvmware.com/2012/04/vsphere-on-open-compute-part-1-lab-setup/). I'm looking forward to future installations of this series.

* Here's a nice summary of [CLI commands for manipulating VMs](http://blog.allanglesit.com/2012/04/esx-5-command-line-vm-manipulation/) on ESXi 5.

* [Mounting VMFS from Ubuntu Linux](http://ubuntuguide.net/how-to-mountaccess-vmware-vmfs-filesystems-in-ubuntu-linux)? Interesting

* How many people out there have played with/are using [VirtualBox](http://www.virtualbox.org/)? I'm interested in getting any feedback.

* Adrian Costea provides a "how to" on [adding a vCenter Server to Microsoft SCVMM](http://www.vkernel.ro/blog/how-to-add-a-vmware-vcenter-server-to-scvmm).

* Here's [a design](http://vrif.blogspot.com/2012/04/vsphere-5-host-network-design-10gbe-vds.html) by Paul Kelly that presents a highly redundant vSphere 5 network layout. It's a sweet layout, leveraging mutliple 10Gb Ethernet links, Nexus 2000s, and Nexus 5000s. At least he admits that it's costly.

* This site has what appears to be some good instructions for [getting up and running with oVirt](http://blog.jebpages.com/archives/how-to-get-up-and-running-with-ovirt/). I say "what appears to be good instructions" because I haven't yet had the opportunity to walk through these instructions to see how useful they are.

That's it for this time around. I hope that you have found something useful here. If anyone has any suggestions for sites/forums they've found helpful with data center-focused topics, I'd love for you to add that information in the comments.
