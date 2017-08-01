---
author: slowe
categories: Information
comments: true
date: 2010-09-28T21:05:34Z
slug: technology-short-take-4
tags:
- Cisco
- EMC
- FCoE
- HyperV
- Microsoft
- Networking
- Storage
- Symmetrix
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #4'
url: /2010/09/28/technology-short-take-4/
wordpress_id: 2111
---

Welcome to Technology Short Take #4, the latest collection of virtualization, storage, networking, and other data center-related links, news, thoughts, and views. As always, I hope you find something interesting here!

* First up, we'll revisit the idea of multi-hop FCoE. You might recall a couple of articles I wrote a while ago [about Network Interface Virtualization (NIV)][1] and the [role of NIV in FCoE][2]. One of my new favorite networking bloggers, Ivan Pepelnjak, picked up this same topic recently in [this post on multi-hop FCoE](http://blog.ioshints.info/2010/09/multihop-fcoe-102-vnport-proxy-and-fip.html). (Did I also link to his [earlier multi-hop FCoE piece](http://blog.ioshints.info/2010/08/multihop-fcoe-101.html)?) Anyway, it's a good read and worth reviewing if you're at all unclear about how these various technologies play together.

* William Lam---whose Virtually Ghetto blog joined the Top 25 Virtualization Blogs in the last round of voting---has published [a primer on VMware vsish](http://www.virtuallyghetto.com/2010/08/what-is-vmware-vsish.html), the VMkernel System Interface Shell. William has done an awesome job of providing a great level of detail and information about this previously undocumented utility. Great work, William, and congratulations on your top 25 blog position!

* Frank Denneman has posted another great in-depth technical article on [how the ESX/ESXi CPU scheduler handles NUMA nodes and wide VMs](http://frankdenneman.nl/2010/09/esx-4-1-numa-scheduling/). This is seriously good stuff and should be on the must-read list for anyone wanting to better understand how ESX/ESXi works. Frank also had a post on [resource pools and simultaneous vMotions](http://frankdenneman.nl/2010/09/resource-pools-and-simultaneous-vmotions/) that is worth reading. Yet another reason not to use resource pools as a folder structure!

* Fellow co-worker Aaron Delp brought to light a key design consideration: how should a VMware architect handle 10GbE and vSphere 4.1's increased vMotion throughput? His article (available [here](http://blog.aarondelp.com/2010/09/keeping-vmotion-tiger-in-10gb-cage-part.html)) triggered a number of other articles, such as this article on [VMware QoS designs with Cisco UCS and Nexus](http://bradhedlund.com/2010/09/15/vmware-10ge-qos-designs-cisco-ucs-nexus/) by noted networking expert Brad Hedlund. I love it when a fellow blogger triggers a great conversation like this. Good stuff!

* Noted VMware performance guru and (now) vSpecialist Scott Drummonds recently posted a great piece on [optimizing vSphere for hyper-threading](http://vpivot.com/2010/09/13/optimizing-vsphere-for-hyper-threading/). In his article, Scott discusses the NUMA.preferHT configuration parameter and the potential implications of using that parameter. Also be sure to check out Scott's article on [databases, storage, and solid state disks](http://vpivot.com/2010/09/20/databases-storage-and-solid-state-disks/). This is just another example of needing to take a holistic approach to performance and how new technologies (like EMC FAST in this article) are affecting designs.

* Justin Guidroz had a good post with a script aimed at helping [update the IPMI settings on ESX hosts](http://geauxvirtual.wordpress.com/2010/09/20/working-with-ucs-and-vcenter-ipmi-settings/) with the settings from UCS Manager. Why is this important? IPMI is the mechanism whereby VMware vSphere can power servers down as part of VMware Distributed Power Management (DPM), and out-of-sync settings can prevent DPM from working properly.

* If you've ever wanted to better understand mirror positions in EMC's Symmetrix arrays, fellow vSpecialist Travers Nicholas has a [good write-up here](http://nickapedia.com/2010/09/21/dont-panic-mirror-positions/). Mirror positions are a key part of how the Symmetrix operates, and Travers does a good job of explaining the basics.

* On the Hyper-V side of the blogging world, I've seen a few interesting posts come from Ben Armstrong (aka Virtual PC Guy), who appears to be on something of a scripting kick. First there's this article on [setting up non-administrative control of Hyper-V](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/27/setting-up-non-administrative-control-of-hyper-v-through-powershell.aspx), followed up by [creating a Hyper-V Administrators local group](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/28/creating-a-hyper-v-administrators-local-group-through-powershell.aspx) through PowerShell. Oh, and Ben continued his series on working with Hyper-V dynamic memory with this post on [changing the minimum memory](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/09/15/scripting-dynamic-memory-part-5-changing-minimum-memory.aspx).

* If you are using VM failure monitoring with VMware HA, [this recent VMware KB article](http://kb.vmware.com/kb/1027734) shows you how to determine if a VM reboot was caused by VMware HA.

I guess that's about it for this time around. Before I go, though, here are a few miscellaneous links you might find interesting:

[10 Networking Books to read before you die](http://etherealmind.com/10-networking-books-to-read-before-you-die/)  

[EMC Replication Manager With vSphere 4 and LVM.EnableResignature](http://goingvirtual.wordpress.com/2009/09/26/emc-replication-manager-with-vsphere-4-and-lvm-enableresignature/)  

[RecoverPoint in Vblock for SAP](http://www.youtube.com/watch?v=volRHmsFM6g&feature=youtube_gdata)  

[Error Upgrading VMware Tools vSphere 4.1](http://kendrickcoleman.com/index.php?/Tech-Blog/error-upgrading-vmware-tools-vsphere-41.html)  

[Automating ESXi 4.1 Kickstart Tips & Tricks](http://www.virtuallyghetto.com/2010/09/automating-esxi-41-kickstart-tips.html)

As always, feel free to share other links or items in the comments below. Thanks for reading this far!

[1]: {{< relref "2010-03-16-understanding-network-interface-virtualization.md" >}}
[2]: {{< relref "2010-03-21-how-niv-and-fcoe-play-together.md" >}}
