---
author: slowe
categories: Information
comments: true
date: 2010-12-29T12:44:48Z
slug: technology-short-take-9
tags:
- Cisco
- Citrix
- FCoE
- Microsoft
- Networking
- Nexus
- Storage
- UCS
- Virtualization
- VMware
- vSphere
- Windows
title: 'Technology Short Take #9'
url: /2010/12/29/technology-short-take-9/
wordpress_id: 2196
---

Welcome to Technology Short Take #9, the last Technology Short Take for 2010. In this Short Take, I have a collection of links and articles about networking, servers, storage, and virtualization. Of note this time around: some great DCI links, multi-hop FCoE finally arrives (sort of), a few XenServer/XenDesktop/XenApp links, and NTFS defragmentation in the virtualized data center. Here you go---enjoy!

## Networking

* Brad Hedlund has a great post discussing [Nexus 7000 connectivity options for Cisco UCS](http://bradhedlund.com/2010/12/01/cisco-nexus-7000-connectivity-solutions-for-cisco-ucs/). I'll include it in this section since it focuses more on the networking aspect rather than UCS. I haven't had the time to read the full PDF linked in Brad's article, but the other topics he discusses in the post---FabricPath networks, F1 vs. M1 linecards, and FCoE connectivity---are great discussions. I'm confident the PDF is equally informative and useful.

* This UCS-specific post describes [how northbound Ethernet frame flows work](http://jeremywaldrop.wordpress.com/2010/06/30/cisco-ucs-ethernet-frame-flows/). Very useful information, especially if you are new to Cisco UCS.

* Data Center Interconnect (DCI) is a hot topic these days considering that it is a key component of long-distance vMotion (aka vMotion at distance). Ron Fuller (who I had the pleasure of meeting in person a few weeks ago, great guy), aka [@ccie5851](http://twitter.com/ccie5851) on Twitter and one of the authors of _NX-OS and Cisco Nexus Switching: Next-Generation Data Center Architectures_ (available [from Amazon](http://www.amazon.com/NX-OS-Cisco-Nexus-Switching-Next-Generation/dp/1587058928/ref=pd_sim_b_2)), wrote a series on the various available DCI options such as EoMPLS, VPLS, A-VPLS, and OTV. If you're considering DCI---especially if you're a non-networking guy and need to understand the impact of DCI on the networking team---this series of articles is worth reading. Part 1 is [here](http://www.networkworld.com/community/blog/how-stretch-vlans-between-multiple-physical-d) and part 2 is [here](http://www.networkworld.com/community/blog/how-stretch-vlans-between-multiple-physical--0).

* And while we are discussing DCI, here's a brief post by Ivan Pepelnjak about [DCI encryption](http://blog.ioshints.info/2010/10/data-center-interconnect-dci-encryption.html).

* This post was a bit deep for me (I'm still getting up to speed on the more advanced networking topics), but it seemed interesting nevertheless. It's a [how-to on redistributing routes between VRFs](http://fryguypa.wordpress.com/2010/10/28/multi-vrf-redistribution-a-k-a-route-leaking-between-vrfs/).

* Optical or twinax? That's the question discussed by Erik Smith in [this post](http://brasstacksblog.typepad.com/brass-tacks/2010/12/should-i-use-optical-fiber-or-twinax-cable.html).

* Greg Ferro also discusses cabling in this post on [cabling for 40 Gigabit and 100 Gigabit Ethernet](http://etherealmind.com/notes-physical-connectors-40-100-gigabit-ethernet/).

## Servers

* As you probably already know, Cisco released version 1.4 of the UCS firmware. This version incorporates a number of significant new features: support for direct-connected storage, support for incorporating C-Series rack-mount servers into UCS Manager (via a Nexus 2000 series fabric extender connected to the UCS 61x0 fabric interconnects), and more. Jeremy Waldrop has [a brief write-up](http://jeremywaldrop.wordpress.com/2010/12/29/cisco-ucs-firmware-1-4/) that lists a few of his favorite new features.

* This next post might only be of interest to partners and resellers, but having been in that space before joining EMC I fully understand the usefulness of having a list of references and case studies. In this case, it's a [list of case studies and references for Cisco UCS](http://www.mseanmcgee.com/2010/10/cisco-ucs-references/), courtesy of M. Sean McGee (who I hope to meet in person in St. Louis in just a couple of weeks).

## Storage

* This could go in either the Networking or Storage sections, but I was alerted via Twitter (can't remember who pointed this out, sorry) that the NX-OS 5.0(2)N2(1) release includes support for VE Ports (see the "New and Changed Features" section of [the release notes](http://www.cisco.com/en/US/docs/switches/datacenter/nexus5000/sw/release/notes/Rel_5_0_2_N1_1/Nexus5000_Release_Notes_5_0_2_N1_1.html#wp162065)). VE Ports are important because they enable you to interconnect multiple Fibre Channel Forwarders (FCFs) together over a lossless Ethernet fabric. In other words, they enable multi-hop FCoE. Obviously, you'd need support for VE Ports on all interconnected FCFs, which today means multi-hop FCoE with Nexus 5000 series switches.

* I wrote a piece on [configuring SAN port channels from a Nexus to an MDS][1] a short while ago, and now here's a post on [configuring SAN port channels from a Nexus when in NPV mode](http://brasstacksblog.typepad.com/brass-tacks/2010/12/creating-san-port-channels-when-using-npv-mode-on-nexus-5k.html). Good stuff.

* Here's a post I found that discusses [bad flash recovery and HDD replacement on an Iomega](http://zepman.tweakblogs.net/blog/3552/iomega-ix2-200-bad-flash-recovery-and-hdd-replacement.html). Not exactly data center-class equipment, I know, but I also know that a great many readers and fellow geeks have this sort of equipment in their home labs. This might be useful information.

* I think it was Nigel Poulton who mentioned this post by Hu Yoshida regarding [Hitachi's "global pool of processors" approach with the VSP](http://blogs.hds.com/hu/2010/12/a-global-pool-of-processors-a-second-look.html). Based on what I read there, it sounds like the VSP and the VMAX share some architectural similarities in this regard. If I'm incorrect in that regard, I'd love to hear your thoughts why in the comments.

* Fellow vSpecialist Clint Kitson has a good discussion of [LUN trespassing and VMware multipathing](https://community.emc.com/thread/114381?tstart=0) over at the Everything VMware at EMC community.

* Richard Anderson has a comparison of [using VPLEX for SQL database resiliency versus SQL database mirroring](http://storagesavvy.com/2010/06/25/resiliency-vs-redundancy-using-vplex-for-sql-ha/).

## Virtualization

* Using XenServer and need to support multicast? Look to this article for the information on [how to enable multicast with XenServer](http://www.booches.nl/2010/12/20/xenserver-and-multicast-with-igmp-support/).

* A couple of colleagues over at Intel (I worked with Brian on one of his earlier white papers) forwarded me the link to their latest Ethernet virtualization white paper, which discusses the use of 10 Gigabit Ethernet with VMware vSphere. You can find the link to the latest paper in [this blog entry](http://communities.intel.com/community/wired/blog/2010/12/08/latest-intel-ethernet-virtualization-paper-for-your-reading-pleasure).

* Bhumik Patel has [a good write-up](http://community.citrix.com/display/ocb/2010/10/23/Technical+Insight+in+to+the+Citrix-Cisco+Validated+Design+Guides) on the "behind-the-scenes" technical details that went into the Cisco-Citrix design guides around XenDesktop/XenApp on Cisco UCS. Bhumik provides the details on things like how many blades were using in the testing, what the configuration of the blades was, and what sort of testing was performed.

* Thinking of carving your storage up into guest OS datastores for VMware? You might want to [read this first](http://blogs.vmware.com/kb/2010/12/purpose-built-guest-os-datastores-dont-do-it.html) for some additional considerations.

* I know that this has seen some traffic already, but I did want to point out Eric Sloof's [post on the Xenoss XenPack for ESXTOP](http://www.ntpro.nl/blog/archives/1627-Zenoss-Announces-Free-Tool-for-VMware-Power-Users-with-Esxtop.html). I haven't had the opportunity to use it yet, but would certainly love to hear from anyone who has. Feel free to share your experiences in the comments.

* As is usually the case, Duncan Epping has had some great posts over the last few weeks. His post on [shares set on resource pools](http://www.yellow-bricks.com/2010/12/14/shares-set-on-resource-pools/) highlights the need to adjust the shares value (and other resource constraints) based on the contents of the pool, something that many people forget to do. He also provides a breakdown of the [various vCenter memory statistics](http://www.yellow-bricks.com/2010/12/20/vcenter-and-memory-metrics/), and discusses an issue with [binding a Provider vDC directly to an ESX/ESXi host](http://www.yellow-bricks.com/2010/12/27/binding-a-vcloud-director-provider-vdc-to-an-esx-host/).

* PowerCLI 4.1.1 has [some improvements for VMware HA clusters](http://blogs.vmware.com/vipowershell/2010/12/ha-cluster-improvements.html) which are detailed in this VMware vSphere PowerCLI Blog entry.

* Frank Denneman has three articles which have caught my attention over the last few weeks. (All his stuff is good, by the way.) First is his two-part series on the impact of oversized virtual machines ([part 1](http://frankdenneman.nl/2010/12/impact-of-oversized-virtual-machines-part-1/) and [part 2](http://frankdenneman.nl/2010/12/impact-of-oversized-virtual-machines-part-2/)). Some of the impacts Frank discusses include memory overhead, NUMA architectures, shares values, HA slot size, and DRS initial placement. Apparently a part 3 is planned but hasn't been published yet (see some of the comments in part 2). Also worth a read is Frank's recent post on [node interleaving](http://frankdenneman.nl/2010/12/node-interleaving-enable-or-disable/).

* Here's yet another tool in your toolkit to help with the transition to ESXi: a post by Gabe on [setting logfile location, swap file, SNMP, and vmkcore partition in ESXi](http://www.gabesvirtualworld.com/setting-logfile-location-swap-file-snmp-and-vmkcore-partition-in-esxi/).

* Here's another guide to [creating a bootable ESXi USB stick](http://www.jadota.com/2009/05/how-to-create-your-own-bootable-esxi-4-usb-stick/) (on Windows). Here's [my guide to doing it on Mac OS X][2].

* Jon Owings had an idea about [dynamic cluster pooling](http://www.2vcps.com/2010/12/20/dynamic-cluster-pooling/). This is a pretty cool idea---perhaps we can get VMware to include it in the next major release of vSphere?

* Irritated that VMware disabled copy-and-paste between the VM and the vSphere Client in vSphere 4.1? Fix it with [these instructions](http://www.vladan.fr/how-to-re-enable-the-copy-paste-between-vi-client-and-vm-in-vsphere-4-1/).

* This white paper on [configuration examples and troubleshooting for VMDirectPath](http://www.vmware.com/resources/techresources/10170) was recently released by VMware. I haven't had the chance to read it yet, but it's on my "to read" list. I'll just have a look at that in my copious free time...

* David Marshall has posted on VMblog.com a two-part series on how NTFS causes I/O bottlenecks on virtual machines ([part 1](http://vmblog.com/archive/2010/12/16/how-ntfs-causes-io-bottlenecks-on-virtual-machines.aspx) and [part 2](http://vmblog.com/archive/2010/12/22/how-ntfs-causes-io-bottlenecks-on-virtual-machines-part-2.aspx)). It's a great review of NTFS and how Microsoft's file system works. Ultimately, the author of the posts (Robert Nolan) sets the readers up for the need for NTFS defragmentation in order to reduce the I/O load on virtualized infrastructures. While I do agree with Mr. Nolan's findings in that regard, there are other considerations that you'll also want to include. What impact will defragmentation have on your storage array? For example, I think that NetApp doesn't recommend using defragmentation in conjunction with their storage arrays (I could be wrong; can anyone confirm?). So, I guess my advice would be to do your homework, see how defragmentation is going to affect the rest of your environment, and then proceed from there.

* Microsoft thinks that [App-V should be the most important tool in your virtualization tool belt](http://windowsteamblog.com/windows/b/business/archive/2010/12/16/app-v-why-it-should-be-the-most-important-tool-in-your-virtualization-tool-belt.aspx). Do you agree or disagree?

* William Lam has instructions for [how to identify the origin of a vSphere login](http://www.virtuallyghetto.com/2010/12/how-to-identify-origin-of-vsphere-login.html). This might not be something you need to do on a regular basis, but when you do need to do it you'll be thankful you have the instructions how.

I guess it's time to wrap up now, since I have likely overwhelmed you with a panoply of data center-related tidbits. As always, I encourage your feedback, so please feel free to speak up in the comments. Thanks for reading!

[1]: {{< relref "2010-11-30-san-port-channels-from-nexus-5010-to-mds-9134.md" >}}
[2]: {{< relref "2009-01-08-creating-a-bootable-esxi-usb-stick-on-mac-os-x.md" >}}
