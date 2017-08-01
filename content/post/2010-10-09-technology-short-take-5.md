---
author: slowe
categories: Information
comments: true
date: 2010-10-09T11:52:27Z
slug: technology-short-take-5
tags:
- Cisco
- EMC
- Networking
- Storage
- vCloud
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #5'
url: /2010/10/09/technology-short-take-5/
wordpress_id: 2131
---

Welcome to Technology Short Take #5, the latest collection of data center technology-related links, articles, blog posts, thoughts, and ideas. Some of this might be useful, some of it might not be helpful, but hopefully it will prove handy to someone out there. Enjoy!

* Brian Norris (of [Going Virtual](http://goingvirtual.wordpress.com)) recently posted a couple of useful "gotchas" about EMC Avamar and VMware Site Recovery Manager with EMC CLARiiON arrays. The issue with Avamar involves [renaming the Default Virtual Machine Group](http://goingvirtual.wordpress.com/2010/10/06/emc-avamar-default-virtual-machine-group-rename-issue/) and is a known issue that will be addressed in a future release; for now, the workaround (as also described by Brian) is either to not rename the Default Virtual Machine Group or not use it. The [second issue](http://goingvirtual.wordpress.com/2010/09/29/vmware-site-recovery-manager-with-clariion-gotcha/) involves an issue with the EMC Solutions Enabler, which is a required component for a number of EMC software solutions. In this particular case, Brian needed to use the x86 (32-bit) version of Solutions Enabler, not the x64 (64-bit version). As Brian mentions, be sure to double-check the release notes for the product in question to see which version of the Solutions Enabler is required (if it's require at all). Good posts, Brian!

* I don't think I've pointed this post out yet, but if I have I apologize. Duncan Epping (of [Yellow Bricks](http://www.yellow-bricks.com), although you probably already know that) recently produced this blog post on the [allocation of memory to the Service Console in VMware ESX](http://www.yellow-bricks.com/2010/09/21/service-console-memory-a-common-misunderstanding-esx-4-0/). As Duncan points out, the allocation of RAM to the Service Console is actually dynamic and is based on the amount of memory installed in the host. Duncan also links to [an updated VMware KB article](http://kb.vmware.com/kb/1003501) that also describes this behavior. Unfortunately, aside from Duncan's article, there is no official document that describes the algorithm VMware ESX uses to determine how much memory to assign.

* If you're interested in more details on NetIOC, [this document](http://www.vmware.com/resources/techresources/10119) is a good place to start.

* The "traffic trombone" (a term coined by Greg Ferro aka [Etherealmind](http://etherealmind.com) and used again by Ivan Pepelnjak of [Cisco IOS Hints and Tricks](http://blog.ioshints.info)) is something that I discussed during my Denver VMUG [presentation on stretched VMware clusters][1]. As originally described by Greg [here](http://etherealmind.com/vmware-vfabric-data-centre-network-design/) and then revisited again by Ivan [here](http://blog.ioshints.info/2010/09/long-distance-vmotion-and-traffic.html), the "traffic trombone" is introduced when you use a technology such as long-distance vMotion but the rest of the network is not/can not be aware that a particular VM has now migrated. I suspect that this is going to be a growing concern as long-distance vMotion (and supporting technologies, like Cisco OTV, EMC VPLEX, or NetApp MetroCluster) see continued adoption. If this isn't something you're factoring into your data center and network designs, then you've overlooked a key consideration. I'm scheduled to have a call with a few networking gurus very soon and I plan on discussing this issue and potential workarounds; I'll post more here when I am able.

* As a quick follow-up to Greg Ferro's article that coined the term "traffic trombone", the focus of that article is actually centered around "vFabric" (later revealed as vChassis), which was VMware's loose vision for a future network architecture. If indeed VMware is thinking along the lines that Greg envisions, then VMware themselves might provide the fix to the "traffic trombone" as part of their vChassis vision. Discussions of the "traffic trombone" also don't (yet) incorporate vCloud Director networking concepts. Hmmm....I might need to jump on that before any of the growing number of talented vCloud bloggers do!

* And speaking of talented vCloud bloggers, Hany Michael had two good posts in the last few days centered around vCloud. First was a [revision to his vCloud Director in a Box setup](http://www.hypervizor.com/2010/10/advanced-guide-vmware-vcloud-director-in-a-box-works-on-4gb-laptops/), which was followed by a post on [how to change or renew the SSL certificates on vCloud Director cells](http://www.hypervizor.com/2010/10/changingrenewing-your-ssl-certificates-on-vcloud-director-cells/).

* VMware vCloud Director also continues to see attention from other bloggers as well. Duncan posted [vCD Networking Part 3](http://www.yellow-bricks.com/2010/10/06/vcd---networking-part-3--use-case-2/); and David Hill posted both an article on [how to un-install the vCD agent through the vCloud Director UI](http://www.virtual-blog.com/2010/10/howto-un-install-the-vcd-agent-through-vcloud-director-ui/) as well as [Part 2 of vCloud Director Q&A](http://www.virtual-blog.com/2010/10/vmware-vcloud-director-qa-part-2/).

* Brian Feeny posted a good article [comparing methods of combining FCIP tunnels, Ethernet port channels, and FC port channels](http://www.feeny.org/?p=510). All in all, it sounds like using FC port channels built with multiple FCIP tunnels is better. (Brian also recently posted [an errata list](http://www.feeny.org/?page_id=87) for the MDS SAN-OS 3.x CLI command reference. Handy!)

In addition to the links mentioned above, here are some additional links you might find interesting or useful:

[Installing VMware vShield App fails with the error: Previous installation of host services encountered an error](http://kb.vmware.com/kb/1028003) (thanks Itzik)  

[Cannot Remove a vSphere host from vCenter](http://ict-freak.nl/2010/10/06/cannot-remove-a-vsphere-host-from-vcenter/)  

[VMware vCenter 4.1 Upgrade/Migration Gotchas](http://jeremywaldrop.wordpress.com/2010/10/05/vmware-vcenter-4-1-upgrademigration-gotchas/)  

[VMware KB: Cisco Nexus 1000V drops packets when Mac Pinning](http://kb.vmware.com/kb/1027731)  

[How Hyper-V responds to disk failure](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/10/07/how-hyper-v-responds-to-disk-failure.aspx)  

[Boot a VM from iSCSI? Yes. We. Can!](http://vinternals.com/2010/10/boot-a-vm-from-iscsi-yes-we-can/)  

[Lights, Camera, Replication : UBER SRM Video Guide](http://nickapedia.com/2010/10/07/lights-camera-replication-uber-srm-video-guide/)

Well, that ought to do it for this time around. Trying to get these things published is difficult sometimes because there's just so much material out there! I'm already collecting for Technology Short Take #6...

Thanks for reading!

[1]: {{< relref "2010-09-30-stretched-cluster-presentation-from-denver-vmug.md" >}}
