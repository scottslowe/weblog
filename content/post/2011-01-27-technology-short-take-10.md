---
author: slowe
categories: Information
comments: true
date: 2011-01-27T10:37:33Z
slug: technology-short-take-10
tags:
- Cisco
- EMC
- HyperV
- Microsoft
- NetApp
- Networking
- Storage
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #10'
url: /2011/01/27/technology-short-take-10/
wordpress_id: 2228
---

Welcome to Technology Short Take #10, my latest collection of data center-oriented links, articles, thoughts, and tidbits from around the Internet. I hope you find something useful or informative!

## Networking

* Link aggregation with VMware vSwitches is something I've touched upon a great many posts here on my site, but one thing that I don't know I've ever specifically called out is that VMware vSwitches don't support LACP. But that's OK---Ivan Pepelnjak takes care of that for me with his [recent post on LACP and the VMware vSwitch](http://blog.ioshints.info/2011/01/vmware-vswitch-does-not-support-lacp.html). He's absolutely right: there's no LACP support in VMware vSphere 4.x or any previous version.

* Stephen Foskett does a great job of providing a [plain English guide to CNA compatibility](http://blog.fosketts.net/2011/01/24/vmware-esx-fcoe-cna-compatibility-plain-english/). Thanks, Stephen!

* And while we are on the topic of Mr. Foskett, he also authored this piece on [NFS in converged network environments](http://www.networkcomputing.com/next-gen-network/is-nfs-a-viable-protocol-for-converged-networking.php). The article seemed a bit short for some reason. It kind of felt like the subject could have used a deeper, more thorough treatment. It's still worth a read, though.

* Need to trace a MAC address in your data center? CiscoZine provides all the necessary details in their post on [how to trace a MAC address](http://www.ciscozine.com/2011/01/12/how-to-trace-mac-address/).

* Jeremy Stretch of PacketLife.net provides a good overview of [using WANem](http://packetlife.net/blog/2011/jan/12/emulating-wans-wanem/). If you need to do some WAN emulation/testing, this is worth reading.

* Jeremy also does a walkthrough of [configuring OSPF between Cisco and Force10](http://packetlife.net/blog/2010/nov/8/configuring-ospfv2-between-cisco-and-force10/) networking equipment.

* I don't entirely understand all the networking wisdom found here, but this post by Brad Hedlund on [Nexus 7000 routing and vPC peer links](http://bradhedlund.com/2010/12/16/routing-over-nexus-7000-vpc-peer-link-yes-and-no/) is something I'm going to bookmark for when my networking prowess is sufficient for me to fully grasp the concepts. That might take a while...

* On the other hand, this post by Brad on [FCoE, VN-Tag, FEX, and vPC](http://bradhedlund.com/2010/12/09/great-questions-on-fcoe-vntag-fex-vpc/) is something I can (and did) assimilate much more readily.

* Erik Smith documents the steps for enabling FCoE QoS on the Nexus 5548, something that Brad Hedlund alerted me to via Twitter. It turns out, as Erik describes in his post about [FCoE login failure with Nexus 5548](http://brasstacksblog.typepad.com/brass-tacks/2011/01/fcoe-login-failure-when-connecting-to-nexus-5548.html), that without the FCoE QoS enabled fabric logins will fail. If you're thinking of deploying Nexus 5548 switches, definitely keep this in mind.

## Servers

* In the event you haven't already read up on it, the UCS 1.4(1) release for Cisco UCS was a pretty major release. See [the write-up here](http://www.mseanmcgee.com/2010/12/ciscos-stocking-stuffer-for-ucs-customers-firmware-release-1-41/) by M. Sean McGee. By the way, Sean is an outstanding resource for UCS information; if you aren't subscribed to his blog, you should be.

* Dave Alexander also has a good discussion about some of the reasoning behind [why certain things are or are not in Cisco UCS](http://www.unifiedcomputingblog.com/?p=178).

## Storage

* Nigel Poulton tackles [a comparison between the HDS VSP and the EMC VMAX](http://blog.nigelpoulton.com/vmax-vs-vsp/). I think he does a pretty good job of comparing and contrasting the two products, and I'm looking forward to his software-focused review of these two products in the future.

* Brandon Riley provides [his view of the recently-announced EMC VNX](http://www.virtualinsanity.com/index.php/2011/01/21/my-take-on-emc-vnx/). The discussion in the comments about the choice of form factor (EFD) for flash-based cache is worth reading, too.

* Andre Leibovici discusses the need for proper storage architecture in this treatment of [IOPs, read/write ratios, and storage tiering with VDI](http://myvirtualcloud.net/?p=1421). While his discussion is VDI-focused, the things he discussed are important to consider with any storage project, not just VDI. I would contend that too many organizations don't do this sort of important homework when virtualizing applications (especially "heavier" workloads with more significant resource requirements), which is why the applications don't perform as well after being virtualized. But that's another topic for another day...

* Environments running VMware Site Recovery Manager with the EMC CLARiiON SRA should have a look at [this article](http://goingvirtual.wordpress.com/2011/01/07/vmware-srm-with-clariion-sra-gotcha/).

* Jason Boche recently published his results from [a series of tests on jumbo frames with NFS and iSCSI in a VMware vSphere environment](http://www.boche.net/blog/index.php/2011/01/24/jumbo-frames-comparison-testing-with-ip-storage-and-vmotion/). There's lots of great information in this post---I highly recommend reading it.

## Virtualization

What, you didn't think I'd overlook virtualization, did you?

* I like the idea of [CLASH](http://blog.mccrory.me/2011/01/11/clash-cloud-admin-shell/). It just needs more development.

* Running VMware Fusion? I am. Ever needed guest VMs on Fusion to have a static (fixed) IP address? I have. Here's [how it's done](http://www.stereoplex.com/blog/vmware-fusion-guests-with-a-static-ip) (hint: Fusion uses a hidden DHCP server with its own configuration file).

* This VMware KB article tells you [how to change the default password hashing mechanism from MD5 to SHA512](http://kb.vmware.com/kb/1032666).

* If you haven't yet upgraded to vSphere 4.1, [this VMware KB article](http://kb.vmware.com/kb/1022104) is a nice supplement to [the vSphere Upgrade Guide](http://www.vmware.com/pdf/vsphere4/r41/vsp_41_upgrade_guide.pdf).

* In [Technology Short Take #9][1], I referenced [Part 1](http://frankdenneman.nl/2010/12/impact-of-oversized-virtual-machines-part-1/) and [Part 2](http://frankdenneman.nl/2010/12/impact-of-oversized-virtual-machines-part-2/) of Frank Denneman's series of articles on the impact of oversized virtual machines. Since then Frank has published [part 3](http://frankdenneman.nl/2011/01/impact-of-oversized-virtual-machines-part-3/), in which he looks at oversized virtual machines from the perspectives of CPU scheduling, memory management, and bootstorms.

* I also liked Frank's article on [setting cluster resource percentages](http://frankdenneman.nl/2011/01/setting-correct-percentage-of-cluster-resources-reserved/) correctly. That's a "little thing" that people often overlook, I think.

* Oh, and one other thing: Frank, I think you can stop [beating the CPU affinity dead horse](http://frankdenneman.nl/2011/01/beating-a-dead-horse-using-cpu-affinity/) now, OK?

* Ben Armstrong posts a couple of useful Hyper-V posts (off-topic question: should he change his handle from "Virtual PC Guy" to "Hyper-V Guy"?). First is a discussion of [how Hyper-V handles VM shutdowns](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/01/11/shutting-down-a-virtual-machine.aspx), followed by a post on [how to use PowerShell to determine which physical computer you're running on](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/01/07/what-physical-computer-am-i-on.aspx).

* Duncan has [a great post](http://www.yellow-bricks.com/2011/01/20/enable-storage-io-control-on-all-datastores/) on the interaction between SIOC (Storage IO Control) and "external" workloads. This is something that both Scott Drummonds and I discussed earlier (see [my post][1] and see [Scott D's post](http://vpivot.com/2010/11/03/sioc-event-ignore-or-panic/)), but it's good to see it get some additional attention. Duncan and I traded some comments and some tweets about the issue, particularly with regard to the interaction of SIOC and low-RPO replication solutions. I've got some thoughts on that interaction that I need to work through, but don't be surprised to see a blog post on that in the near future.

* Hany Michael has a tutorial on [getting vCloud Director working with vShield Edge and vShield App](http://www.hypervizor.com/2011/01/integrating-vmware-vcloud-director-with-vshield-edge-and-vshield-app/).

Before I wrap up, I'll just leave with you a few other links from my collection:

[IOBlazer](http://labs.vmware.com/flings/ioblazer)  

[Backing up, and restoring, VMware vCloud Director provisioned virtual machines](http://blogs.vmware.com/uptime/2010/09/backing-up-and-restoring-vmware-cloud-director-provisioned-virtual-machines.html)  

[RSA SecurBook on Cloud Security and Compliance](http://rsa.com/node.aspx?id=3382)  

[Hyper-V Live Migration using SRDF/CE - Geographically Dispersed Clustering](http://msmvps.com/blogs/jtoner/archive/2010/01/05/hyper-v-live-migration-using-srdf-ce.aspx)  

[The VCE Model: Yes, it is different](http://nickapedia.com/2011/01/22/the-vce-model-yes-it-is-different/)  

[How to make a PowerShell server side VMware vCenter plugin](https://community.emc.com/message/522461#522461)  

[VMware vSphere 4 Performance with Extreme I/O Workloads](http://www.vmware.com/resources/techresources/10054)  

[VMware KB: ESX Hosts Might Experience Read Performance Issues with Certain Storage Arrays](http://kb.vmware.com/kb/1002598)  

[vSphere "Gold" Image Creation on UCS, MDS, and NetApp with PowerShell](http://www.theselights.com/2010/12/vsphere-gold-image-creation-on-ucs-mds.html)  

[Upgrading to ESX 4.1 with the Nexus 1000V](https://www.myciscocommunity.com/docs/DOC-17797)  

[My System Engineer's toolkit for Mac](http://teneo.wordpress.com/2010/12/30/my-system-engineers-toolkit-for-mac/)

That's going to do it for this time around. As always, courteous comments are welcome and encouraged!

[1]: {{< relref "2010-12-29-technology-short-take-9.md" >}}
[2]: {{< relref "2010-11-01-sioc-event-with-emc-mirrorview.md" >}}
