---
author: slowe
categories: Information
comments: true
date: 2011-09-12T09:00:00Z
slug: technology-short-take-14
tags:
- FCoE
- Networking
- Storage
- Vblock
- vCloud
- Virtualization
- VLAN
- VXLAN
- Encryption
- Hardware
title: 'Technology Short Take #14'
url: /2011/09/12/technology-short-take-14/
wordpress_id: 2415
---

Welcome to Technology Short Take #14, another collection of links and tidbits of information I've gathered over the last few weeks. Let's dive right in!

## Networking

Much of my focus in the networking space recently has been around virtualization-centric initiatives, so the items on this list might seem a bit skewed in that direction. Sorry!

* I've been doing some reading on [802.1Qbg (Edge Virtual Bridging)](http://www.ieee802.org/1/pages/802.1bg.html). I still have a long way to go, but I think that I'm starting to better understand this draft and the problem(s) it's attempting to address. As usual when I'm dealing with networking-related technologies, especially data center-focused networking technologies, I'm finding some of Ivan Pepelnjak's articles useful. For example, he wrote an article on [how EVB should ease VLAN configuration pains](http://blog.ioshints.info/2011/05/edge-virtual-bridging-evb-8021qbg-eases.html); this article is helpful in translating the terms the IEEE uses (like "virtual station interface" and "EVB station") into terms virtualization-friendly folks like me can understand (like "vNIC" and "Hypervisor supporting EVB", respectively). Ivan also provides [a rough comparison of 802.1Qbh/FEX and 802.1Qbg](http://blog.ioshints.info/2011/08/vm-fex-how-convoluted-can-you-get.html), which I also found helpful in better understanding both technologies. There is still much that I want/need to understand, such as how 802.1Qbg affects or is affected by VXLAN, the recent darling of the cloud networking space.

* Speaking of VXLAN, a number of articles have emerged since the announcement of VXLAN last week at VMworld 2011 in Las Vegas. Jon Oltsik of Network World called it ["Cloud Network Segmentation at Layer 2.5"](http://www.networkworld.com/community/blog/vmware-vxlan-cloud-network-segmentation-over-), which is catchy but doesn't really delve into the details of VXLAN and how it plays into/with related data center protocols. Of course, there's also [the obligatory VMware post](http://communities.vmware.com/community/vmtn/cto/security-and-networking/blog/2011/09/06/vxlans-and-the-cloud-infrastructure-suite) on the technology, talking about how great it is---naturally---but failing to again provide substantive information on the relationships between VXLAN and other, related data center technologies. If anything, Allwyn's post made VXLAN _seem_ even more proprietary and linked to vCloud Director and vShield Edge than I'd understood it to be. Fortunately, Ivan [weighed in on the proposed new standard](http://blog.ioshints.info/2011/08/finally-mac-over-ip-based-vcloud.html) and also provided some information on [the relationship between VXLAN, OTV, and LISP](http://blog.ioshints.info/2011/09/vxlan-otv-and-lisp.html). I'm still digging into VXLAN myself and I plan to post an article within the next week or so (I've been a bit busy with [moving halfway across the United States][1]).

* Ivan also has a post with more details on the [Brocade VCS fabric load-balancing behaviors](http://blog.ioshints.info/2011/04/brocade-vcs-fabric-has-almost-perfect.html) that's worth having a look.

## Servers

* [This article on AES-NI](http://datacenteroverlords.com/2011/09/07/aes-ni-pimp-your-aes/) in the newer Intel CPUs is a great look at the benefits of adding symmetric encryption support at the CPU level. Almost makes me want to go out and buy a new MacBook Pro so that I could use File Vault 2 with hardware encryption support

## Storage

* One cool find recently is this series of "hands-on" posts by Henri Hamalainen (aka [@henriwithani](http://twitter.com/henriwithani)) on the EMC VNXe 3300. I had the pleasure of meeting Henri in person at VMworld this year, and he mentioned that he'd started a series of posts on the VNXe 3300. His posts are here: [part 1](http://henriwithani.wordpress.com/2011/08/18/hands-on-with-vnxe-3300-part-1/), [part 2](http://henriwithani.wordpress.com/2011/08/22/hands-on-with-vnxe-3300-part-2/), [part 3](http://henriwithani.wordpress.com/2011/08/23/hands-on-with-vnxe-3300-part-3/), [part 4](http://henriwithani.wordpress.com/2011/09/02/hands-on-with-vnxe-3300-part-4/), [part 5](http://henriwithani.wordpress.com/2011/09/07/hands-on-with-vnxe-3300-part-5/), and [part 6](http://henriwithani.wordpress.com/2011/09/08/hands-on-with-vnxe-3300-part-6/). (Part 7 hasn't yet been written.)

* There's been quite a hubbub going on in the FCoE space, with so many articles flashing back and forth from various contributors I'm still having a hard time deciphering all of it. From what I can tell, it all started with an article by a VP at Juniper about FCoE over TRILL. That sparked Ivan Pepelnjak [to coin some new terms](http://blog.ioshints.info/2011/06/fcoe-over-trill-this-time-from-juniper.html): "dense-mode FCoE" (in which FCFs exist at every hop) and "sparse-mode FCoE" (in which LAN switches may forward FCoE frames without any FCoE awareness). That, in turn, sparked an article by Tony Bourke in which he creates [more new modes of operation](http://datacenteroverlords.com/2011/08/16/jinkies-its-an-fcoe-mystery/): single hop FCoE (SHFCoE), dense-mode FCoE (DMFCoE), and sparse-mode FCoE (SMFCoE). A fantastic (and very informative) discussion ensued in the comments to that article and [the follow-up article](http://datacenteroverlords.com/2011/08/25/the-case-for-fcoe-terminology/). Ivan also responded to Tony's post as well with a post on [FCoE network elements classification](http://blog.ioshints.info/2011/08/fcoe-networking-elements-classification.html). I'm not sure that all the contributors ever came to a consensus, but you'll learn a lot about FCoE and related technologies just following along, that's for sure.

* By the way, [this transcript of questions and answers from a live FCoE webcast](https://supportforums.cisco.com/docs/DOC-15882) has some great information buried in it as well.

* This is an older article, but Stephen Foskett does a good job with [discussing FCoE vs. iSCSI](http://blog.fosketts.net/2011/05/20/fcoe-iscsi-convergence-ethernet/). Like so many other IT-related decisions, it's not an "either-or" discussion---it's about finding the right tool to do the job.

* [This article](http://goingvirtual.wordpress.com/2011/09/09/how-i-zone-vnx-storage-arrays/) provides one suggestion for handling zoning with multiple storage arrays, and provides some good information on EMC CLARiiON/VNX arrays in the process.

## Virtualization

* The idea of stretched clusters and interconnecting data centers continues to be an idea many people are interested in exploring. Rawley Burbridge, of IBM, discusses how this might be done using IBM SVC and VMware vSphere in this three-part series ([part 1](http://virtualstoragespeak.wordpress.com/2011/05/25/bridging-datacenters-with-vmware-vsphere-and-ibm-san-volume-controller---part-1/), [part 2](http://virtualstoragespeak.wordpress.com/2011/05/27/bridging-datacenters-with-vmware-vsphere-and-ibm-san-volume-controller---part-2/), and [part 3](http://virtualstoragespeak.wordpress.com/2011/06/21/bridging-datacenters-with-vmware-vsphere-and-ibm-san-volume-controller---part-3/)).

* Kendrick Coleman, in conjunction with a collection of folks from both VMware and VCE, recently published an article on [design considerations for vCloud Director on a Vblock](http://www.kendrickcoleman.com/index.php?/Tech-Blog/vcloud-director-on-vblock-design-considerations.html). I haven't yet read the full document, primarily because it appears to require a Facebook login in order to download. (I don't use Facebook.)

* Andre Leibovici---who I had the pleasure of meeting in person at VMworld---has an article on how to modify the Windows Registry settings (or apply Group Policy) for the VMware View Client in order to [integrate self-service password reset](http://myvirtualcloud.net/?p=1754).

* The VMware vCloud Architecture Toolkit (vCAT) version 2.0 is now available; get it [here](http://www.vmware.com/cloud-computing/cloud-architecture/vcat-toolkit.html) (via [Beaker](http://www.rationalsurvivability.com/blog)).

* Forbes Guthrie---the lead author with whom I worked on _VMware vSphere Design_, published earlier this year--[posted](http://www.vReference.com/2011/08/29/vmworld-spo3040-best-practises-using-10gbe-for-vsphere-5-0/) some great 10Gb Ethernet-related information from VMworld session SPO3040.

* This is a slightly older post from Hany Michael, but a good one nevertheless; he posts information on [how to publish the vCloud Director portal on the Internet](http://www.hypervizor.com/2011/07/publishing-the-vcloud-director-portal-on-the-internet/).

I guess that will wrap things up for this time around. Thanks for reading, and I hope that you found something useful in this varied mix of links. Feel free to share more in the comments below!

[1]: {{< relref "2011-07-19-time-for-a-fresh-start.md" >}}
