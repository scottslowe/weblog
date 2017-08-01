---
author: slowe
categories: Information
comments: true
date: 2010-11-29T09:00:00Z
slug: technology-short-take-7
tags:
- Cisco
- FCoE
- HyperV
- Microsoft
- Networking
- Security
- Storage
- Virtualization
- vSphere
- Linux
- VMware
title: 'Technology Short Take #7'
url: /2010/11/29/technology-short-take-7/
wordpress_id: 2163
---

Welcome to Technology Short Take #7! This time around I have a collection of links from networking, servers, storage, and virtualization. Our hot topics in this issue include Fibre Channel over Ethernet (FCoE) and its need---or lack thereof---for congestion management, Ubuntu on Hyper-V, the benefits of VAAI, and more!

## Networking

I have a lot of FCoE-related links this time around. I'm not sure if that means FCoE has been getting more coverage or if it's just a case of [confirmation bias](http://youarenotsosmart.com/2010/06/23/confirmation-bias/).

* Need to decrypt a Cisco type 7 password? [This page](http://fryguypa.wordpress.com/2010/11/18/type-7-password-decryption-via-ios-router-bonus/) provides instructions on how it can be done. (Please be sure to use your powers for good, not evil.)

* This [blog post catalogue](http://blog.ine.com/2010/09/21/blog-post-catalogue-2/) is a link list to a treasure trove of networking information.

* I suppose [this](http://blog.ioshints.info/2010/10/coping-with-long-distance-vmotion.html) is _one_ way of dealing with requests to do long-distance vMotion. I'm not so sure I agree that it's an effective way.

* The [use of NIV to create the equivalent of multi-hop FCoE][1] is something I discussed a while ago, but Brad Hedlund recently [revisited it again](http://bradhedlund.com/2010/11/19/the-end-to-end-fcoe-justifies-the-means-to-means/). I can see both sides of the argument---both "for" and "against" considering fabric extenders as multi-hop FCoE---and I can see the need to use standard terminology to describe things. Without standard terminology, "multi-hop FCoE" means different things to different vendors and it's hard for customers to make valid comparisons.

* Erik Smith, a relatively new blogger, has a great [introduction to FIP, FIP Snooping Bridges, and FCFs](http://brasstacksblog.typepad.com/brass-tacks/2010/11/fip-fip-snooping-bridges-and-fcfs-part-1-fip-the-fcoe-initialization-protocol.html). If you're new to FCoE---or even if you aren't and want more detail---this is a great read with loads of relevant information. I'm looking forward to more of Erik's posts on this topic.

* The blog battle over FCoE's need for QCN rages on. Joe Onisick does a good job of [explaining QCN and why it might/might not be necessary](http://www.definethecloud.net/whats-the-deal-with-quantized-congestion-notification-qcn), so if you're unfamiliar with the debate that's a good place to start. Ivan Pepelnjak [breaks down 802.1Qau](http://blog.ioshints.info/2010/11/data-center-bridging-dcb-congestion.html) (the QCN standard) even further, providing more details on its operation and behavior. He then weights into the debate with [this quick explanation](http://blog.ioshints.info/2010/11/does-fcoe-need-qcn-8021qau.html) and [this comparison to Frame Relay](http://blog.ioshints.info/2010/11/fcoe-qcn-and-analogies.html). In the end, the answer to the question of FCoE's need for QCN really boils down to everyone's favorite IT answer: "It depends." In this case, it depends upon your network design. With more DCB-capable switches between the end nodes and the FCFs, QCN becomes more valuable. With fewer (or no) DCB-capable switches between the end nodes and the FCFs, QCN offers far less benefit.

## Servers

I'm adding this section because I have some articles that apply to servers, but not necessarily to virtualization. Since it fits in nicely with the data center theme of Technology Short Takes, it seems like a reasonable addition.

* Jeff Allen, a UCS-focused CSE at Cisco, recently posted this guide to [SAN boot with Cisco UCS](http://jeffsaidso.com/2010/11/boot-from-san-101-with-cisco-ucs/). It's definitely worth a read, especially if you're new to UCS or haven't done boot from SAN with UCS before.

* I haven't had nearly the time to blog about Cisco UCS as I would have liked, but Brian Gracely included me in this [list of people to follow for Cisco UCS information](http://blogs.cisco.com/datacenter/who-to-follow-for-cisco-ucs/). Thanks, Brian! I'll do my best to earn my inclusion on the list.

* Chris Fendya of WWT posted instructions on [how to slipstream the Cisco UCS drivers](http://vblog.wwtlab.com/2010/10/01/windows-2003-and-cisco-ucs/) into the installation of Windows Server 2003.

## Storage

It's funny to me that the storage section of these posts is typically the shortest. There are plenty of storage-related blogs out there, but almost all of them are high-level and tend not to provide the sort of down-to-earth "in the trenches" information I like to include. If readers have any suggestions for blogs that provide this sort of information, I'd love to hear them.

* InformationWeek recently published this article on [how to break free from Tier 1 SAN vendors](http://www.informationweek.com/news/storage/virtualization/showArticle.jhtml?articleID=228000296). (Disclosure: I work for just such a Tier 1 SAN vendor.) I can't say that I agree with the author's reasoning; by the same token, customers should be able to go out and buy white box servers. Yet, companies such as HP and Dell are still selling lots of servers. Why is that? Because the value of a top-tier server is greater than the sum of its parts, and the same can be said for Tier 1 storage arrays. Now, having said that, I do agree that storage virtualization---which was the real focus of the InformationWeek article---can bring a lot of value and flexibility to the data center. I just don't think that storage virtualization and Tier 1 storage arrays are mutually exclusive.

* Here is a good "how to" on [enabling ALUA and Round Robin multipathing](http://www.marco-galli.net/?p=22) with ESX and a CLARiiON CX4 array.

* Bob Plankers has a great article on [the impact of VAAI](http://lonesysadmin.net/2010/11/08/if-you-ever-needed-convincing-about-vaai/) on storage operations. In this post, he shows how the write rate for his VAAI-capable HDS AMS 2500 drops to nothing when cloning templates. This is a great demonstration of how VAAI helps offload storage operations from the hosts to the array. Keep in mind that VAAI might not make operations faster, but it will make them more efficient. (It's a subtle distinction, but an important one nevertheless.)

* In the event you are considering pursuing CCIE Storage---a task that I've been strongly considering undertaking---Brian Feeny posted [a list of CCIE Storage preparation resources](http://www.feeny.org/?p=655).

## Virtualization

* If you're thinking of installing Ubuntu Server on Hyper-V, you should have a look at Ben Armstrong's [recent write-up](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/10/21/installing-ubuntu-server-10-10-on-hyper-v.aspx).

* A few other Hyper-V links also made it into this week's collection of links. First, we have a discussion of [Hyper-V differencing disks with VDI](http://blogs.msdn.com/b/rds/archive/2010/10/25/using-hyper-v-differencing-disks-with-vdi.aspx). Second, we have another post by Ben Armstrong, this one containing [a script to evacuate a Hyper-V cluster node on shutdown](http://blogs.msdn.com/b/virtual_pc_guy/archive/2010/10/25/script-to-evacuate-cluster-node-on-shutdown.aspx). Third, here's a post on [using Hyper-V dynamic memory to increase VDI density](http://blogs.technet.com/b/virtualization/archive/2010/11/08/hyper-v-dynamic-memory-test-for-vdi-density.aspx). (As an aside, I would _still_ like to hear from Microsoft or another Hyper-V resource regarding dynamic memory. It still seems to me that without allowing memory overcommitment, dynamic memory is of limited value. Anyone?) Finally, Patrick O'Rourke posted some Hyper-V news from Tech-Ed Berlin; specifically, he was discussing [the Hyper-V Cloud program](http://blogs.technet.com/b/virtualization/archive/2010/11/08/hyper-v-cloud-program-for-private-cloud.aspx). It's nice to see Microsoft finally embracing the idea of the private cloud, validating what other virtualization competitors have been discussing for a while now.

* Frank Brix aka "vFrank" posted more information on [MAC address generation and address conflicts](http://www.vfrank.org/2010/11/18/mac-address-conflicts/), including a list of the different VMware OUIs for MAC addresses.

* Eric Sloof has [a video on how to enable vsish](http://www.ntpro.nl/blog/archives/1639-How-to-enable-the-VMware-Interactive-Shell-vsish-on-ESX.html), the VMkernel Sys Info Shell.

* Frank Denneman has a good article on [a potential issue with active/standby NICs and vSwitch failback](http://frankdenneman.nl/2010/10/vswitch-failover-and-high-availability/). In his article, he describes a scenario (albeit a limited scenario) in which a cluster-wide isolation response could occur. The workaround is to use active/active NICs or set vSwitch failback to No.

* Frank also posted a follow-up article on the topic of [NUMA, hyperthreading, and the numa.preferHT advanced setting](http://frankdenneman.nl/2010/10/numa-hyperthreading-and-numa-preferht/).

* Gavin Adams has a good "how to" on [replacing the vCenter Server SSL certificate](http://www.gavinadams.org/blog/2010/07/14/replacing-vcenter-4-1-ssl-certificate-with-active-directory-issued-one). It might be worth a read if you are in a situation that requires a valid SSL certificate on your vCenter Server system.

* This article is a good [overview of VMware vShield](http://geeksilver.wordpress.com/2010/11/19/vmware-vsphere-vshield-4-1-understanding-part-1/), including Zones, App, Edge, and Endpoint.

* Several interesting VMware KB articles found their way to my URL Inbox, including this one on [changing the IP address of the vCloud Director server](http://kb.vmware.com/kb/1028657); this KB article on [an issue with VMXNET3 virtual adapters and cloning Windows 2008 R2 or Windows 7 templates](http://kb.vmware.com/kb/1020078); this one on [an issue with the in-box Broadcom bnx2x driver with ESX/ESXi 4.1](http://kb.vmware.com/kb/1029368); and an article on [an HA error with vCenter 4.1 and ESXi 4.0](http://kb.vmware.com/kb/1027628). It also looks like [Hany Michael](http://www.hypervizor.com/) has started contributing diagrams to the VMware KB: see [here](http://kb.vmware.com/kb/1030816) and [here](http://kb.vmware.com/kb/1030954).

* Eric Sloof provides more information on using `esxtop` to measure SCSI reservations and reservation conflicts; see [this post](http://www.ntpro.nl/blog/archives/1634-New-in-esxtop-RESVs-SCSI-reservations-per-second.html) and [this follow-up post](http://www.ntpro.nl/blog/archives/1638-New-in-esxtop-SCSI-Reservation-Conflicts-per-second-Revisited.html).

* While this is a bit old (published back in late September), Dave Hill has a post on [a building block design for VMware vCloud Director](http://www.virtual-blog.com/2010/09/vmware-vcloud-director-building-block-resource-group-design/).

That wraps up this installation of Technology Short Takes. As always, your comments, thoughts, suggestions, or clarifications are welcome, so please speak up in the comments!

[1]: {{< relref "2010-03-21-how-niv-and-fcoe-play-together.md" >}}
