---
author: slowe
categories: Information
comments: true
date: 2012-07-30T13:14:25Z
slug: technology-short-take-24
tags:
- Networking
- Security
- Storage
- Virtualization
- VMware
- vSphere
- Xen
- SDN
- FibreChannel
- Citrix
- Solaris
title: 'Technology Short Take #24'
url: /2012/07/30/technology-short-take-24/
wordpress_id: 2704
---

Welcome to Technology Short Take #24, another instance of my irregularly-published collection of links, thoughts, and rants on various data center technologies like networking, operating systems, security, hardware, virtualization, and cloud computing. This is a slightly shorter version of my Technology Short Takes; I'm trying to pare down since some readers have indicated the previous Short Takes weren't short enough. Anyway, I hope you find something useful.

## Networking

* This page is a decent reference to the [open source software-defined networking (SDN) projects](http://www.sdncentral.com/open-source-projects/) that are out there. While I'm sure it's not comprehensive---open source projects can be difficult to track sometimes---it's at least a good starting point.

* Here's an older article by Brad Hedlund on [building a leaf-spine design with either 40G or 10G](http://bradhedlund.com/2012/01/25/construct-a-leaf-spine-design-with-40g-or-10g-an-observation-in-scaling-the-fabric/). Which is better? As usual, the IT answer is, "It depends." It's a good article overall, although it reminds me that I still have so much to learn in networking. It's a good thing there are smart folks like Brad who are willing to share their knowledge.

## Security

* Bromium finally "opened the kimono" to talk about what they're doing. I had the chance to chat with Simon Crosby, and I must say that it's pretty cool stuff. If you haven't yet read it, check out [Simon's post at BrianMadden.com](http://www.brianmadden.com/blogs/guestbloggers/archive/2012/06/20/guest-blog-from-simon-crosby-explaining-what-bromium-is-and-how-it-works.aspx).

* While I was in Indianapolis last week for the Indianapolis VMUG, I sat in on a session by Lancope on the use of Netflow to secure your network. The presenter showed a list of open source Netflow tools. I haven't gotten the specific list that the presenter used, but I did find [this list](http://mccltd.net/blog/?p=957)--perhaps it will be useful.

## Storage

* In 2009 I wrote a piece [explaining NPIV and NPV][1]. In May Tony Bourke posted [a write-up of NPIV and NPV](http://datacenteroverlords.com/2012/05/08/npv-and-npiv/) as well, and did a good job of drawing some analogies about these technologies. There's a great discussion going on in the comments as well, so I recommend reading the comments too.

* [This article](http://www.scalingbits.com/performance/io) is titled "Understanding IO," but it really seems like more of a write-up on various IO analysis tools. Still quite useful, even though it seems to be a bit focused on Solaris.

* I finally got around to reading Stephen Foskett's I/O Blender series ([part 1](http://blog.fosketts.net/2012/05/23/io-blender-part-1-ye-olde-storage-io-path/), [part 2](http://blog.fosketts.net/2012/05/24/io-blender-part-2-virtualization/), and [part 3](http://blog.fosketts.net/2012/05/25/io-blender-part-3-behold-power-demultiplexer/)), in which he describes the current state of storage and virtualization as a introduction to some of the ideas that VMware described in their "next-generation" storage [presented last year at VMworld 2011][2]; in particular, the demultiplexer.

## Virtualization

* Maish Saidel-Keesing has a three-part write-up on installing and configuring OpenIndiana in a VM ([part 1](http://technodrone.blogspot.com/2012/05/openindiana-installation-walkthrough.html), [part 2](http://technodrone.blogspot.com/2012/05/openindiana-installation-walkthrough_14.html), and [part 3](http://technodrone.blogspot.com/2012/05/openindiana-installation-walkthrough_15.html)). This is not something I've had the opportunity to work with, although I have worked some with Solaris in the past. (In fact, this weekend I tried to find a Solaris 10 x86 ISO I used to have somewhere because I was going to build a Solaris 10 VM for some Puppet testing and couldn't find it. Bummer.)

* Via Vladan Seget, I saw that VMware vSphere 5.0 has [achieved Common Criteria EAL4+ certification](http://www.vladan.fr/highest-level-security-certification-for-vsphere-5/).

* This [VMware KB article](http://kb.vmware.com/kb/2017642) has a great PDF attached that covers vSphere's various memory management techniques.

* Working your way toward taking the VCAP-DCD exam? [This site](http://www.seancrookston.com/vcap-dcd-index/), while a bit dated, has some good resources for VDCD410 (the vSphere 4 version of the exam). Of course, there's also this little [video training course that was recently released][3].

* Here's a [Citrix Knowledge Center article](http://support.citrix.com/article/CTX126624) that provides more information on SR-IOV (Single Root I/O Virtualization) support within XenServer (and, by extension, Xen Cloud Platform/XCP).

* There's an interesting note here about [interactions between SIOC and SRM 5](http://blocksandbytes.com/2012/06/15/sioc-is-fully-supported-by-srm-5-er-except-for-planned-migrations/).

That's it for this time around; feel free to add your own thoughts in the comments below. Courteous comments are always welcome!

[1]: {{< relref "2009-11-27-understanding-npiv-and-npv.md" >}}
[2]: {{< relref "2011-08-29-vsp3205-tech-preview-vstorage-apis.md" >}}
[3]: {{< relref "2012-07-30-designing-vmware-infrastructure-video-training-available.md" >}}
