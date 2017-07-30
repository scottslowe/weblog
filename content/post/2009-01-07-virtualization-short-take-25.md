---
author: slowe
categories: Information
comments: true
date: 2009-01-07T14:28:33Z
slug: virtualization-short-take-25
tags:
- Citrix
- ESX
- HyperV
- Microsoft
- NetApp
- NFS
- Solaris
- Virtualization
- VMFS
- VMware
- VMwareHA
- Windows
title: 'Virtualization Short Take #25'
url: /2009/01/07/virtualization-short-take-25/
wordpress_id: 1122
---

Welcome to Virtualization Short Take #25, the first edition of this series for 2009! Here I've collected a variety of articles and posts that I found interesting or useful. Enjoy!

* We'll start off today's list with some Hyper-V links. First up is [this article](http://blogs.msdn.com/robertvi/archive/2008/12/19/howto-manually-add-a-vm-configuration-to-hyper-v.aspx) on how to manually add a VM configuration to Hyper-V. It would be interesting to me to know some of the technical details---i.e., the design decisions that led Microsoft to architect things in this way---that might explain why this process is, in my opinion, so complicated. Was it scalability? Manageability? If anyone knows, please share your information in the comments.

* It looks like this post by John Howard on [how to resolve event ID 4096 with Hyper-V](http://blogs.technet.com/jhoward/archive/2008/12/28/hyper-v-resolving-event-id-4096.aspx) is also closely related.

* This blog post brings to light a clause in Microsoft's licensing policy that [forces organizations to use Windows Server 2008 CALs](http://blogs.vmware.com/virtualreality/2008/12/do-i-really-need-to-upgrade-all-my-windows-server-2003-cals-in-order-to-run-on-windows-hyper-v.html) when accessing a Windows Server 2003-based VM hosted on Hyper-V. In the spirit of disclosure, it's important to note that this was written by VMware, but an independent organization apparently verified the licensing requirements. So, while you may get Hyper-V at no additional cost (not free) with Windows Server 2008, you'll have to pay to upgrade your CALs to Windows Server 2008 in order to access any Windows Server 2003-based VMs on those Hyper-V hosts. Ouch.

* Wrapping up this edition's Microsoft virtualization coverage is this post by Ben Armstrong warning Hyper-V users about [the use of physical disks with VMs](http://blogs.msdn.com/virtual_pc_guy/archive/2008/12/15/being-careful-with-physical-disks.aspx). Apparently, it's possible to connect a physical disk to both the Hyper-V parent partition as well as a guest VM, and...well, bad things can happen when you do that. The unfortunate part is that Hyper-V doesn't block users from doing this very thing.

* Daniel Feller asks the question, "Am I the only one who has trouble understanding Cloud Computing?" No, Daniel, you're not the only one---I've written before about how amorphous and undefined cloud computing is. In [this post over at the Citrix Community site](http://community.citrix.com/pages/viewpage.action?pageId=50364953), Daniel goes on to indicate that cloud computing's undefined nature is actually its greatest strength:  

	>As I see it, Cloud Computing is a big white board waiting for organizations to make their requirements known.  Do you want a Test/QA environment to do whatever? This is cloud computing. Do you want someone to deliver office productivity applications for you? That is cloud computing. Do you want to have all of your MP3s stored on an Internet storage repository so you can get to it from any device?  That is also cloud computing.

	Daniel may be right there, but I still insist that there need to be well-defined and well-understood standards around cloud computing in order for cloud computing to really see broad adoption. Perhaps cloud computing is storing my MP3s on the Internet, but what happens when I want to move to a different MP3 storage provider? Without standards, that becomes quite difficult, perhaps even impossible. I'm not the only one who thinks this way, either; check [this post by Geva Perry](http://gevaperry.typepad.com/main/2008/12/vendor-vision-lockin-in-the-cloud.html). Until some substance appears in all these clouds, people are going to hold off.

* Rodney Haywood shared a useful command to use with VMware HA in [this post about blades and VMware HA](http://rodos.haywood.org/2008/12/blade-enclosures-and-ha.html). He points out that it's a good idea to spread VMware HA primary nodes across multiple blade chassis so that the failure of a single chassis does not take down all the primary nodes. One note about the using the `ftcli` command is that you'll need to set the FT\_DIR environment variable first using `export FT_DIR=/opt/vmware/aam` (assuming you're using bash as the shell on VMware ESX). Otherwise, the advice to spread clusters across chassis as well as to ensure that primary agents are spread across chassis is advice that should be followed.

* Joshua Townsend has a good post at VMtoday.com about [using PowerShell and SQL queries](http://vmtoday.com/2008/12/obtaining-vmware-guest-disk-free-space-for-nfs-sizing/) to determine the amount of free space within guest VMs. As he states in his post, this can often impact the storage design significantly. It seems to me that there used to be a plug-in for vCenter that added this information, but I must be mistaken as I can no longer find it. Oh, and [one of Eric Siebert's top 10 lists](http://vmware-land.com/Top_10_Lists.html#top10_admin_tools) also points out a free utility that will provide this information as well.

* I don't have a record of where this information turned up, but [this article from NetApp](https://now.netapp.com/Knowledgebase/solutionarea.asp?id=kb41202) (NOW login required) on troubleshooting NFS performance was quite helpful. In particular, it linked to [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1003710&sliceId=2&docTypeID=DT_KB_1_1&dialogID=7006425&stateId=0%200%202781970) that provides in-depth information on how to identify IRQ sharing that's occurring between the Service Console and the VMkernel. Good stuff.

* Want more information on scaling a VMware View installation? Greg Lato posts [a notice about the VMware View Reference Architecture Kit](http://www.latogalabs.com/2008/12/vmware-view-building-blocks-architecture-guide/), available [from VMware](http://www.vmware.com/resources/wp/view_reference_architecture_register.html), that provides more information on some basic "building blocks" in creating a large-scale View implementation. I've only had the opportunity to skim through the documents thus far, but I like what I've seen thus far. Chad also [mentions the Reference Architecture Kit on his site](http://virtualgeek.typepad.com/virtual_geek/2008/12/vmware-view-managercomposer-1000-client-reference-architecture.html) as well.

* Duncan at Yellow Bricks posts yet another useful "in the trenches" post about [VMFS-3 heap size](http://www.yellow-bricks.com/2008/12/19/heap-size-vmfs3/). If your VMware ESX server is handling more than 4TB of open VMDK files, then it's worth having a look at [this VMware KB article](http://kb.vmware.com/selfservice/microsites/search.do?cmd=displayKC&docType=kc&externalId=1004424&sliceId=1&docTypeID=DT_KB_1_1&dialogID=11082836&stateId=1%200%2011084649).

* The [idea of "virtual routing"](http://www.nemertes.com/virtual_routing_antimatter_network_routing) is an interesting idea, but I share the thoughts of one of the commenters in that technologies like VMotion/XenMotion/live migration may not be able to respond quickly enough to changing network patterns to be effective. Perhaps it's just my server-centric view showing itself, but it seems more "costly" (in terms of effort) to move servers around to match traffic flow than to just route the traffic accordingly.

* [CrossBow looks quite cool](http://cuddletech.com/blog/pivot/entry.php?id=999), but I'm having a hard time understanding the real business value. I am quite confident that my lack of understanding about CrossBow is simply a reflection of the fact that I don't know enough about Solaris Containers or how Xen handles networking, but can someone help me better understand? What is the huge deal with Crossbow?

* Jason Boche shares some information with us about [how to increase the number of simultaneous VMotion operations](http://www.boche.net/blog/?p=806) per host. That information could be quite handy in some cases.

* I had high hopes for [this document on VMFS best practices](http://communities.vmware.com/docs/DOC-9276), but it fell short of my hopes. I was looking for hard guidelines on when to use isolation vs. consolidation, strong recommendations on VMFS volume sizes and the number of VMs to host in a VMFS volume, etc. Instead, I got an overview of what VMFS is and how it works---not what I needed.

* Users interested in getting started with PowerShell with VMware Infrastructure should have a look at [this article by Scott Herold](http://www.vmguru.com/index.php/articles-mainmenu-62/scripting/74-getting-started-with-powershell-and-powergui-in-your-virtual-infrastructure). It's an excellent place to start.

* Here's a list of some of the [basic things you should do](http://www.techhead.co.uk/10-basic-things-to-do-when-creating-a-microsoft-server-gold-build-for-use-on-vmware-esx-template) on a "golden master" template for Windows Server VMs. I actually disagree with #15, preferring instead to let Windows manage the time at the guest OS level. The only other thing I'd add: be sure your VMDK is aligned to the underlying storage. Otherwise, this is a great checklist to follow.

I think that should just about do it for this post. Comments are welcome!
