---
author: slowe
categories: Information
comments: true
date: 2012-06-12T13:50:41Z
slug: technology-short-take-23
tags:
- Citrix
- FCoE
- HyperV
- iSCSI
- Linux
- LISP
- Microsoft
- Networking
- NFS
- Security
- Storage
- vCloud
- Virtualization
- VMware
- vSphere
- VXLAN
- Windows
- Xen
title: 'Technology Short Take #23'
url: /2012/06/12/technology-short-take-23/
wordpress_id: 2633
---

Welcome to Technology Short Take #23, another collection of links and thoughts related to data center technologies like networking, storage, security, cloud computing, and virtualization. As usual, we have a fairly wide-ranging collection of items this time around. Enjoy!

## Networking

* A couple of days ago I learned that there are a couple open source implementations of LISP (Locator/ID Separation Protocol). There's [OpenLISP](http://www.openlisp.org/), which runs on FreeBSD, and there's also a project called [LISPmob](http://lispmob.org/) that brings LISP to Linux. From what I can tell, LISPmob appears to be a bit more focused on the endpoint than OpenLISP.

* In an [earlier post on STT][1], I mentioned that STT's re-use of the TCP header structure could cause problems with intermediate devices. It looks like someone has figured out how to allow STT through a Cisco ASA firewall; the configuration is [here](http://www.cupfighter.net/index.php/2012/05/allow-stt-stateless-transport-tunneling-through-an-cisco-asa/).

* Jose Barreto posted a nice [breakdown of SMB Multichannel](http://blogs.technet.com/b/josebda/archive/2012/05/13/the-basics-of-smb-multichannel-a-feature-of-windows-server-2012-and-smb-3-0.aspx), a bandwidth-enhancing feature of SMB 3.0 that will be included in Windows Server 2012. It is, unexpectedly, only supported between two SMB 3.0-capable endpoints (which, at this time, means two Windows Server 2012 hosts). Hopefully additional vendors will adopt SMB 3.0 as a network storage protocol. Just don't call it CIFS!

* Reading [this article](http://blog.ioshints.info/2012/05/virtual-networks-skype-analogy.html), you might deduce that Ivan really likes overlay/tunneling protocols. I am, of course, far from a networking expert, but I do have to ask: at what point does it become necessary (if ever) to move some of the intelligence "deeper" into the stack? Networking experts everywhere advocate the "complex edge-simple core" design, but does it ever make sense to move certain parts of the edge's complexity into the core? Do we hamper innovation by insisting that the core always remain simple? As I said, I'm not an expert, so perhaps these are stupid questions.

* Massimo Re Ferre posted a good article on [a typical VXLAN use case](http://it20.info/2012/05/typical-vxlan-use-case/). Read this if you're looking for a more concrete example of how VXLAN could be used in a typical enterprise data center.

* Bruce Davie of Nicira helps explain [the difference between VPNs and network virtualization](http://nicira.com/blog/vpns-meet-network-virtualization); this is a nice companion article to his colleague's post (which Bruce helped to author) on [the difference between network virtualization and software-defined networking (SDN)](https://networkheresy.wordpress.com/2012/05/31/network-virtualization/).

* The folks at Nicira also collaborated on this post regarding [software overhead of tunneling](http://networkheresy.com/2012/06/08/the-overhead-of-software-tunneling/). The results clearly favor STT (which was designed to take advantage of NIC offloading) over GRE, but the authors do admit that as "GRE awareness" is added to the cards that protocol's performance will improve.

* Oh, and while we're on the topic of SDNyou might have noticed that VMware has taken to using the term "software-defined" to describe many of the services that vSphere (and related products) provide. This includes the use of software-defined networking (SDN) to describe the functionality of vSwitches, distributed vSwitches, vShield, and other features. Personally, I think that the term software-based networking (SBN) is **far** more applicable than SDN to what VMware does. It is just me?

* Brad Hedlund wrote [this post](http://bradhedlund.com/2012/02/08/dodging-open-protocols-with-open-software/) a few months ago, but I'm just now getting around to commenting about it. The gist of the article---forgive me if I munge it too much, Brad---is that the use of open source software components might dramatically change the shape/way/means in which networking protocols and standards are created and utilized. If two components are communicating over the network via open source components, is some sort of networking standard needed to avoid being "proprietary"? It's an interesting thought, and goes to show the power of open source on the IT industry. Great post, Brad.

* One more mention of OpenFlow/SDN: it's great technology (and I'm excited about the possibilities that it creates), but it's [not a silver bullet for scalability](http://highscalability.com/blog/2012/6/4/openflowsdn-is-not-a-silver-bullet-for-network-scalability.html).

## Security

* I came across this interesting post on [a security attack based on VMDKs](http://www.insinuator.net/2012/05/vmdk-has-left-the-building/). It's quite an interesting read, even if the probability of being able to actually leverage this attack vector is fairly low (as I understand it).

## Storage

* Chris Wahl has a good series on NFS with VMware vSphere. You can catch the start of the series [here](http://wahlnetwork.com/2012/04/19/nfs-on-vsphere-a-few-misconceptions/). One comment on the testing he performs in [the "Same Subnet" article](http://wahlnetwork.com/2012/04/23/nfs-on-vsphere-technical-deep-dive-on-same-subnet-storage-traffic/): if I'm not mistaken, I believe the VMkernel selection is based upon which VMkernel interface is listed in the first routing table entry for the subnet. This is something about which I wrote back in 2008, but I'm glad to see Chris bringing it to light again.

* George Crump published this article on [using DCB to enhance iSCSI](http://www.storage-switzerland.com/Articles/Entries/2012/1/17_iSCSI_2.0_-_Using_Data_Center_Bridging_To_Enhance_iSCSI.html). (Note: The article is quite favorable to Dell, and George discloses an affiliation with Dell at the end of the article.) One thing I did want to point out is that---if I recall correctly---the 802.1Qbb standard for Priority Flow Control only defines a single "no drop" class of service (CoS). Normally that CoS is assigned to FCoE traffic, but in an environment without FCoE you could assign it to iSCSI. In an environment with both, that could be a potential problem, as I see it. Feel free to correct me in the comments if my understanding is incorrect.

* Microsoft is introducing data deduplication in Windows Server 2012, and here is a good post providing an [introduction to Microsoft's deduplication implementation](http://blogs.technet.com/b/filecab/archive/2012/05/21/introduction-to-data-deduplication-in-windows-server-2012.aspx).

* [SANRAD VXL](http://www.sanrad.com/VXL/4/1/8) looks interesting---anyone have any experience with it? Or more detailed technical information?

* I really enjoyed Scott Drummonds' recent [storage performance analysis post](http://vpivot.com/2012/05/10/storage-performance-analysis-singb-case-study/). He goes pretty deep into some storage concepts and provides real-world, relevant information and recommendations. Good stuff.

## Cloud Computing/Cloud Management

* After moving CloudStack to the Apache Software Foundation, Citrix published [this discourse on "open washing"](http://blogs.citrix.com/2012/04/11/beware-of-open-washing-%E2%80%93-three-key-questions-to-ask-your-software-vendor/) and provides a set of questions to determine the "openness" of software projects with which you may become involved. While the article is clearly structured to favor Citrix and CloudStack, the underlying point---to understand exactly what "open source" means to your vendors---is valid and worth consideration.

* Per [the AWS blog](http://aws.typepad.com/aws/2012/05/vm-export-for-ec2.html), you can now export EC2 instances out of Amazon and into another environment, including VMware, Hyper-V, and Xen environments. I guess this kind of puts a dent in the whole "Hotel California" marketing play that some vendors have been using to describe Amazon.

* Unless you've been hiding under a rock for the past few weeks, you've most likely heard about Nick Weaver's Razor project. (If you haven't heard about it, here's Nick's [blog post](http://nickapedia.com/2012/05/21/lex-parsimoniae-cloud-provisioning-with-a-razor/) on it.) To help with the adoption/use of Razor, Nick also recently [announced an overview of the Razor API](http://nickapedia.com/2012/06/05/api-all-the-things-razor-api-wiki/).

## Virtualization

* Frank Denneman continues to do a great job writing solid technical articles. The latest article to catch my eye (and I'm sure that I missed some) was this post on [combining affinity rule types](http://blogs.vmware.com/vsphere/2012/05/combining-affinity-rule-types.html). 

* This is an interesting post on [a vSphere 5 networking bug affecting iSCSI](http://vmtoday.com/2012/02/vsphere-5-networking-bug-affects-software-iscsi/) that was fixed in vSphere 5.0 Update 1.

* Make a note of [this VMware KB article](http://kb.vmware.com/kb/2019944) regarding UDP traffic on Linux guests using VMXNET3; the workaround today is using E1000 instead.

* This post is actually over a year old, but I just came across it: Luc Dekens [posted](http://www.lucd.info/2011/04/22/get-the-maximum-iops/) a PowerCLI script that allows a user to find the maximum IOPS values over the last 5 minutes for a number of VMs. That's handy. (BTW, I have fixed the error that kept me from seeing the post when it was first published---I've now subscribed to Luc's blog.)

* Want to use a Debian server to provide NFS for your VMware environment? [Here](http://www.mattpson.info/2011/03/28/using-a-debian-server-as-nfs-storage-for-vmware-esxi/) is some information that might prove helpful.

* Jeremy Waldrop of Varrow provides some [information on creating a custom installation ISO](http://jeremywaldrop.wordpress.com/2012/04/27/custom-esxi-5-iso-for-ucs-nexus-1000v-and-powerpath-ve/) for ESXi 5, Nexus 1000V, and PowerPath/VE. Cool!

* Cormac Hogan continues to pump out some very useful storage-focused articles on the official VMware vSphere blog. For example, both the [VMFS locking article](http://blogs.vmware.com/vsphere/2012/05/vmfs-locking-uncovered.html) and the article on [extending an EagerZeroedThick disk](http://blogs.vmware.com/vsphere/2012/06/extending-an-eagerzeroedthick-disk.html) were great posts. I sincerely hope that Cormac keeps up the great work.

* Thanks to [this Project Kronos page](http://wiki.xen.org/wiki/Project_Kronos), I've been able to successfully set up XCP on Ubuntu Server 12.04 LTS. Here's hoping it gets easier in future releases.

* Chris Colotti [takes on some vCloud Director "challenges"](http://www.chriscolotti.us/vmware/how-to-handle-some-vcloud-director-challenges/), mostly surrounding vShield Edge and vCloud Director's reliance on vShield Edge for specific networking configurations. While I do agree with many of Chris' points, I personally would disagree that using vSphere HA to protect vShield Edge is an acceptable configuration. I was also unable to find any articles that describe how to use vSphere FT to protect the deployed vShield appliances. Can anyone point out one or more of those articles? (Put them in the comments.)

* Want to use Puppet to automate the deployment of vCenter Server? See [here](http://puppetlabs.com/blog/module-of-the-week-puppetlabs-vcenter-vmware-vcenter-deployment/).

I guess it's time to wrap up now, lest my "short take" get even longer than it already is! Thanks for reading this far, and I hope that I've shared something useful with you. Feel free to speak up in the comments if you have questions, thoughts, or clarifications.

[1]: {{< relref "2012-03-12-some-thoughts-and-questions-about-stt.md" >}}
