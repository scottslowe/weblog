---
author: slowe
categories: Information
comments: true
date: 2013-01-10T12:45:51Z
slug: technology-short-take-28
tags:
- Automation
- HyperV
- KVM
- macOS
- Microsoft
- Networking
- NFS
- Security
- Storage
- Virtualization
- VMotion
- VMware
- vSphere
- VXLAN
title: 'Technology Short Take #28'
url: /2013/01/10/technology-short-take-28/
wordpress_id: 3047
---

Welcome to Technology Short Take #28, the first Technology Short Take for 2013. As always, I hope that you find something useful or informative here. Enjoy!

## Networking

* Ivan Pepelnjak recently wrote a piece titled ["Edge and Core OpenFlow (and why MPLS is not NAT)"](http://blog.ioshints.info/2013/01/edge-and-core-openflow-and-why-mpls-is.html). It's an informative piece---Ivan's stuff is  always informative---but what really drew my attention was his mention of [a paper by Martin Casado, Teemu Koponen, and others](http://yuba.stanford.edu/~casado/fabric.pdf) that calls for a combination of MPLS and OpenFlow (and an evolution of OpenFlow into "edge" and "core" versions) to build next-generation networks. I've downloaded the paper and intend to review it in more detail. I'd love to hear from any networking experts who've read the paper---what are your thoughts?

* Speaking of Ivanit also appears that he's quite pleased with [Microsoft's implementation of NVGRE in Hyper-V](http://blog.ioshints.info/2012/12/hyper-v-network-virtualization-wnvnvgre.html). Sounds like some of the other vendors need to get on the ball.

* Here's a nice explanation of [CloudStack's physical networking architecture](http://www.shapeblue.com/2013/01/07/understanding-cloudstacks-physical-networking-architecture/).

* The first fruits of Brad Hedlund's decision to join VMware/Nicira have shown up in this joint article by Brad, Bruce Davie, and Martin Casado describing [the role of network virutalization in the software-defined data center](http://cto.vmware.com/network-virtualization-in-the-software-defined-data-center/). (It doesn't matter how many times I say or write "software-defined data center," it still feels like a marketing term.) This post is fairly high-level and abstract; I'm looking forward to seeing more detailed and in-depth posts in the future.

* Art Fewell speculates that the networking industry has "lost our way" and become a "big bag of protocols" in [this article](http://www.networkworld.com/community/node/82098). I do agree with one of the final conclusions that Fewell makes in his article: that SDN (a poorly-defined and often over-used term) is the methodology of cloud computing applied to networking. Therefore, SDN **is** cloud networking. That, in my humble opinion, is a more holistic and useful way of looking at SDN.

* It appears that the vCloud Connector posts ([here](http://blogs.vmware.com/vcloud/2012/10/announcing-vcloud-connector-2-0-one-network-one-catalog-one-cloud.html) and [here](http://blogs.vmware.com/vcloud/2012/12/vcloud-connector-2-0-now-available.html)) that (apparently) incorrectly identify VXLAN as a component/prerequisite of vCloud Connector have yet to be corrected. (Hat tip to Kenneth Hui at VCE.)

## Servers/Hardware

Nothing this time around, but I'll watch for content to include in future posts.

## Security

* Here's [a link](http://en.community.dell.com/techcenter/os-applications/w/wiki/3599.kvm-virtualization-security.aspx) to a brief (too brief, in my opinion, but perhaps I'm just being overly critical) post on KVM virtualization security, authored by Dell TechCenter. It provides some good information on securing the libvirt communication channel.

## Cloud Computing/Cloud Management

* Long-time VMware users probably remember Mike DiPetrillo, whose website has now, unfortunately, gone offline. I mention this because I've had this article on RabbitMQ AMQP with vCloud Director sitting in my list of "articles to write about" for a while, but some of the images were missing and I couldn't find a link for the article. I finally found a link to a [reprinted version of the article](http://cloud.dzone.com/articles/how-setup-rabbitmq-amqp-vcloud) on DZone Enterprise Integration. Perhaps the article will be of some use to someone.

* Sam Johnston talks about [reliability in the cloud](http://samj.net/2012/03/simplifying-cloud-reliability.html) with a discussion on the merits of "reliable software" (software designed for failure) vs. "unreliable software" (more traditional software not designed for failure). It's a good article, but I found the discussion between Sam and Massimo (of VMware) as equally useful.

## Operating Systems/Applications

* I found this article on [the use of Chef and Soloist to automate the setup of OS X 10.8](http://vanderveer.be/blog/2013/01/02/automating-the-setup-of-my-perfect-developer-environment-on-osx-10-dot-8-mountain-lion/) to be quite interesting. I hadn't heard of Soloist before reading this article.

* Perhaps it shows my lack of Unix/Linux experience, but I'm not familiar with RCS, so reading [this post about using RCS to "version control everything"](http://utcc.utoronto.ca/~cks/space/blog/sysadmin/VersionControlForEverything) was interesting to me. I'm going to have to give RCS a spin and see how I like it. I suppose the real question is whether it is worth trying to use RCS (or its equivalent) to version control everything, or just put everything into a configuration management system like Puppet or Chef.

* I think I mentioned this on Twitter, but [this PDF-to-Keynote conversion tool](http://www.cs.hmc.edu/~oneill/freesoftware/pdftokeynote.html) looks like it might be useful from time to time.

## Storage

* Want some good details on the space-efficient sparse disk format in vSphere 5.1? Andre Leibovici has you covered [right here](http://myvirtualcloud.net/?p=3829).

* Read [this article](http://myvirtualcloud.net/?p=4106) for good information from Andre on a potential timeout issue with recomposing desktops and using the View Storage Accelerator (aka context-based read cache, CRBC).

* Apparently Cormac Hogan, aka [@VMwareStorage](http://twitter.com/VMwareStorage) on Twitter, hasn't gotten the memo that "best practices" is now outlawed. He should have named this series on NFS with vSphere "NFS **Recommended** Practices", but even misnamed as they are, the posts still have useful information. Check out [part 1](http://cormachogan.com/2012/11/26/nfs-best-practices-part-1-networking/), [part 2](http://cormachogan.com/2012/11/27/nfs-best-practices-part-2-advanced-settings/), and [part 3](http://cormachogan.com/2012/12/12/nfs-best-practices-part-3-interoperability-considerations/).

* If you'd like to get a feel for how VMware sees the future of flash storage in vSphere environments, read [this](http://blogs.vmware.com/vsphere/2012/12/virtual-flash-vflash-tech-preview.html).

## Virtualization

* This is a slightly older post, but informative and useful nevertheless. Cormac posted an article on [VAAI offloads and KAVG latency](http://blogs.vmware.com/vsphere/2012/09/vaai-offloads-and-kavg-latency.html) when observed in `esxtop`. The summary of the article is that the commands `esxtop` is tracking are internal to the ESXi kernel only; therefore, abnormal KAVG values do not represent any sort of problem. (Note there's also an associated [VMware KB article](http://kb.vmware.com/kb/2012288).)

* More good information from Cormac [here](http://cormachogan.com/2013/01/07/sunrpc-maxconnperip-advanced-setting-explained/) on the use of the SunRPC.MaxConnPerIP advanced setting and its impact on NFS mounts and NFS connections.

* Another slightly older article (from September 2012) is this one from Frank Denneman on [how vSphere 5.1 handles parallel Storage vMotion operations](http://frankdenneman.nl/vmware/vsphere-5-1-storage-vmotion-parallel-disk-migrations/).

* A fellow IT pro contacted me on Twitter to see if I had any idea why some shares on his Windows Server VM weren't working. As it turns out, the problem is related to hotplug functionality; the OS sees the second drive as "removable" due to hotplug functionality, and therefore shares don't work. The problem is outlined in a bit more detail [here](http://social.technet.microsoft.com/Forums/en-US/winserver8gen/thread/179c72b2-2957-43a0-8798-472e907a6e55).

* William Lam outlines [how to use new tagging functionality](http://blogs.vmware.com/vsphere/2012/12/tagging-vmkernel-traffic-types-using-esxcli-5-1.html) in `esxcli` in vSphere 5.1 for more comprehensive scripted configurations. The new tagging functionality---if I'm reading William's write-up correctly---means that you can configure VMkernel interfaces for any of the supported traffic types via `esxcli`. Neat.

* Chris Wahl has a nice write-up on [the behavior of Network I/O Control with multi-NIC vMotion traffic](http://wahlnetwork.com/2012/12/12/testing-vsphere-nioc-host-limits-on-multi-nic-vmotion-traffic/). It was pointed out in the comments that the behavior Chris describes is documented, but the write-up is still handy, and an important factor to keep in mind in your designs.

I suppose I should end it here, before this "short take" turns into a "long take"! In any case, courteous comments are always welcome, so if you have additional information, clarifications, or corrections to share regarding any of the articles or links in this post, feel free to speak up below.
