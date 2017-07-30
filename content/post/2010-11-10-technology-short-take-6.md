---
author: slowe
categories: Information
comments: true
date: 2010-11-10T16:58:32Z
slug: technology-short-take-6
tags:
- Networking
- Storage
- Virtualization
- Cisco
- CLI
- FCoE
- VMware
- vSphere
- UCS
- HyperV
- EMC
title: 'Technology Short Take #6'
url: /2010/11/10/technology-short-take-6/
wordpress_id: 2159
---

Welcome to Technology Short Take #6, the latest collection of links, articles, and thoughts on virtualization, networking, storage, and the intersection of the three. I'm going to try a slightly new format for this post in my Technology Short Take series; I'm interested in knowing what you think about the format.

## Networking

* If you've worked with UNIX (or UNIX variants like Linux or BSD), then you're probably familiar with regular expressions. This post breaks down the use of [regular expressions in Cisco IOS](http://www.ciscozine.com/2010/09/29/cisco-regular-expressions/), which I found useful.

* To get a better understand of some of the IEEE standards that serve to make Ethernet lossless (and thus be a viable transport for FCoE), I recommend having a look at two articles on Cisco IOS Hints and Tricks. The first article is on [802.1Qaz (Enhanced Transmission Selection, or ETS)](http://blog.ioshints.info/2010/09/introduction-to-8021qaz-enhanced.html) and the second article touches on [PFC/ETS and how it impacts storage traffic](http://blog.ioshints.info/2010/10/pfcets-and-storage-traffic-real-story.html). The article on PFC's interaction with storage traffic is particularly good, as it really sheds light on how PFC/ETS will interact with FCoE in a way that at first seems counterintuitive. Once you think about it for a little bit, though, it does make sense.

* This article also provides [a quick summary of the various Data Center Bridging (DCB) standards](http://packetattack.wordpress.com/2010/10/06/dont-drop-the-baby-data-center-bridging-wants-storage-to-trust-ethernet/), in case you need a review of what's involved. (Another post on the DCB standards is available [here](http://www.networkworld.com/community/blog/ethernet-adapts-data-center-applications---pa) as well.)

* Aaron Conaway has a good post on using [SLA monitoring on the PIX/ASA](http://aconaway.com/2010/10/15/sla-monitoring-on-the-pixasa/).

* Need to add a static ARP entry on a Nexus switch? Jeff (aka [fryguy_pa](http://twitter.com/fryguy_pa) on Twitter) has a quick how-to on adding [static ARP entries on NX-OS](http://fryguypa.wordpress.com/2010/10/28/static-arp-entries-on-nx-os/).

* Carole Warner Reece posted what I thought was a useful how-to on [setting up back-to-back vPCs on Cisco Nexus switches](http://www.netcraftsmen.net/resources/blogs/configuring-back-to-back-vpcs-on-cisco-nexus-switches.html). In her example, she used two pairs of Nexus 7000 switches, but it could have just as easily been Nexus 7000s and Nexus 5000s. I'm not sure I entirely understand the benefit of this configuration; she states that it's loop-free but I think I'm going to need to think on this for a bit longer in order to fully get why someone might use this particular configuration.

* And speaking of ways to eliminate Spanning Tree, Jeremy Filliben has a [comparison of a few of the methods](http://www.jeremyfilliben.com/2010/10/comparison-of-current-spanning-tree.html). In the spirit of redundancy, here's another good post on the topic of [Spanning Tree and it's role in FCoE multipathing](http://www.networkworld.com/community/blog/ethernet-adapts-data-center-applications---p-0).

* Josh O'Brien reminds readers in [this post](http://www.staticnat.com/WP/2010/10/07/sometimes-we-just-dont-need-routing/) that there's a reason the `no ip routing` command exists in Layer 3-capable switches. "Just because you can, doesn't mean you should..."

* I experimented with policy-based routing in my virtualized GNS3 environment a while ago, but never could make it work. I didn't really have a use case; I was just experimenting and (hopefully) learning. I might have to use [this post](http://www.booches.nl/2010/10/13/policy-based-routing-in-a-nutshell/) for some guidance next time I give it another try.

## Storage

* If you're in the market for a new storage array, you might want to check support for the vStorage APIs for Array Integration (VAAI). [This page](http://v-reality.info/2010/10/list-of-vaai-capable-storage-arrays/) has a list of VAAI-capable arrays and the necessary firmware required to support VAAI.

* Erik Zandboer describes a [potential issue with VMware ESXi 4.1, host profiles, and CLARiion auto-registration](http://www.vmdamentals.com/?p=797). I'm not aware of any workaround for the problem yet.

* I think that this has been covered quite well elsewhere, but I did want to point it out for the sake of completeness. Fellow vSpecialist team member Nick Weaver ([lynxbat](http://twitter.com/lynxbat) on Twitter) has created a PowerShell front-end for the EMC Celerra arrays, which he's calling [UBERShell](http://nickapedia.com/2010/11/01/ubershell-ninja-scripting/). It's quite impressive what Nick's managed to create. Go have a look if you haven't already.

## Virtualization

* I don't know if this one counts as virtualization or networking, but either way I guess it doesn't really matter. I found [this VMware KB article](http://kb.vmware.com/kb/1013094) on the use of the "Route based on IP hash" setting and the fact that it is not supported on Cisco UCS. This is because the UCS 6100 series fabric interconnects currently don't support any form of cross-stack link aggregation.

* Here's another article that I'm not quite sure is networking or virtualization, but I'll put it under virtualization. Cisco UCS offers the ability to do fabric failover, i.e., provide NIC redundancy in hardware so that the OS/hypervisor doesn't need to. Prior to the 1.4 release of the UCS Manager software, though, there were problems with learned MAC addresses, like the ones given to VMs. I'll let Brad Hedlund explain the rest of it to you in this great post on [UCS fabric failover](http://bradhedlund.com/2010/09/23/cisco-ucs-fabric-failover/). As with so many of Brad's articles, this one is well-written and very informative.

* I think I'm missing something with regard to Hyper-V's new dynamic memory feature. I've been reading a few posts ([this one](http://blogs.technet.com/b/virtualization/archive/2010/11/08/hyper-v-dynamic-memory-test-for-vdi-density.aspx) springs to mind) about how using dynamic memory can increase VM density. I agree with that. As I understand Hyper-V's dynamic memory, it's about configuring a VM with the minimum amount of RAM and then letting it "burst" up to a pre-configured maximum, but without actually overcommitting memory on the host (because, according to Microsoft, overcommitment is bad). So what happens to the extra memory that VMs aren't using? You can't overcommit, so that memory sits there idle, even though you could probably use it. Or am I missing something?

* In the event you're interested in exploring Hyper-V's dynamic memory feature, Ben Armstrong describes [here](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/10/28/enabling-dynamic-memory-on-windows-server-standard-and-web-editions.aspx) what's required to use it.

* This two-part series on VSS and VMware is great reading to get a better understanding of how VSS integration in VMware works (or doesn't work). Part 1 is [here](http://www.blueshiftblog.com/?p=180); Part 2 is [here](http://www.blueshiftblog.com/?p=224). Also, there's a good follow-up to both these articles that provides [additional information on application-level quiescing](http://www.blueshiftblog.com/?p=473).

* Jason Boche has been on a bit of a blogging marathon recently, cranking out some good stuff. There's this article on [reducing FT logging traffic for disk read intensive workloads](http://www.boche.net/blog/index.php/2010/10/28/reducing-ft-logging-traffic-for-disk-read-intensive-workloads/), this post on [an issue with VMware DPM](http://www.boche.net/blog/index.php/2010/10/24/vmware-dpm-issue/), a post on [hardware status and maintenance mode](http://www.boche.net/blog/index.php/2010/10/20/hardware-status-and-maintenance-mode/) (applies only to pre-vSphere 4.1 environments), and finally a post on the [conversion from CPU Ready to %RDY](http://www.boche.net/blog/index.php/2010/10/21/cpu-ready-to-rdy-conversion/). All of these are good articles and well worth the read.

* Duncan's latest article on [using esxplot on Mac OS X](http://www.yellow-bricks.com/2010/11/09/esxplot-on-a-mac/) is pretty cool; I might have to try that myself. Of course, Duncan produces lots and lots of good stuff (which is why he's perennially voted #1). One example: this brief summary of [some VMware HA futures](http://www.yellow-bricks.com/2010/10/14/vmware-high-availability-futures-part-of-bc7803/). If you haven't visited his site recently (or if you aren't subscribed to his RSS feed), you're doing yourself a disservice.

* Reviewing [vscsiStats data using 3D surface charts](http://www.vmdamentals.com/?p=722) is awesome. I love it.

* These two articles by William Lam on [running VMs on Dropbox](http://www.virtuallyghetto.com/2010/10/how-to-run-vm-on-dropbox-storage.html) and [backing up VMs to Dropbox](http://www.virtuallyghetto.com/2010/10/how-to-backup-vms-in-esx-onto-dropbox.html) are interesting (and different), but not entirely practical. Of course, I don't think that William necessarily intended them to be practical, they struck me as more of "I wonder if I could..." type of situation. Still, it's interesting to see how data synchronization tools like [Dropbox](http://www.dropbox.com/) could change the way we view/use VMs. If only there was a hardware-based solution that was transparent to the hypervisor...oh, wait, there is: it's called VPLEX! (Sorry, I couldn't resist. I'll try to behave next time.)

I have a ton more links and posts that I'd love to include, but in the interest of time and length I'll stop here for today. I hope that you find something interesting and useful here!
