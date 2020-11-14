---
author: slowe
categories: Information
comments: true
date: 2012-10-29T09:00:00Z
slug: technology-short-take-26
tags:
- Automation
- KVM
- Linux
- macOS
- Networking
- OVS
- RedHat
- Storage
- Ubuntu
- UNIX
- vCloud
- Virtualization
- VMware
- VXLAN
- Xen
title: 'Technology Short Take #26'
url: /2012/10/29/technology-short-take-26/
wordpress_id: 2910
---

Welcome to Technology Short Take #26! As you might already know, the Technology Short Takes are my irregularly-published collections of links, articles, thoughts, and (sometimes) rants. I hope you find something useful here!

## Networking

* Chris Colotti, as part of a changed focus in his role at VMware, has been working extensively with Nicira NVP. He's had a couple of good posts; [this one](http://www.chriscolotti.us/vmware/nicira-nvp/nicira-nvp-virtualized-networking-primer/) is a primer on how NVP works, and [this one](http://www.chriscolotti.us/vmware/nicira-nvp/how-the-nicira-nvp-esxi-vapp-works/) discusses the use of the Open vSwitch (OVS) vApp. As I mentioned before in other posts, OVS is popping up in more and more places---it might be a good idea to make sure you're familiar with it.

* This article by Ivan Pepelnjak on [VXLAN termination on physical devices](http://blog.ioshints.info/2011/10/vxlan-termination-on-physical-devices.html) is over a year old, but still _very_ applicable---especially considering Arista Networks recently announced their 7150S switch, which sports hardware VTEP (VXLAN Tunnel End Point) support (meaning that it can terminate VXLAN segments).

* Brad Hedlund dives into Midokura Midonet in this post on [L2-L4 network virtualization](http://bradhedlund.com/2012/10/06/mind-blowing-l2-l4-network-virtualization-by-midokura-midonet/). It's a good overview (thanks Brad!) and worth reading if you want to get up to speed on what Midokura is doing. (Oh, just as an aside: note that Midokura leverages OVS in their solution. Just saying)

* This blog post provides more useful information from Kamau Wanguhu on [VXLAN and proxy ARP](http://www.borgcube.com/blogs/2012/10/vxlan-and-proxy-arp/). Kamau also has an interesting [post on network virtualization](http://www.borgcube.com/blogs/2012/10/network-virtualization-overview/), although---to be honest---the post is long on messaging/positioning and short on technical information. I prefer the latter instead of the former.

## Servers/Hardware

* [This mention](http://bladesmadesimple.com/2012/10/first-lookdell-poweredge-m-io-aggregator/) of the Dell PowerEdge M I/O Aggregator looks interesting, although I'm still not real clear on exactly what it is or how it works. I guess this first article was a tease?

## Security

Nothing this time around, but I'll stay alert for items to include in future posts!

## Cloud Computing/Cloud Management

* Want to know a bit more about how to configure VXLAN inside VCD? Rawlinson Rivera has [a nice write-up](http://www.punchingclouds.com/2012/09/09/vcloud-director-5-1-vxlan-configuration/) that is worth reviewing.

* Clint Kitson, an EMC vSpecialist, talks about [some VCD integrity scripts he created](http://velemental.com/2012/10/10/what-changed-vcloud-director-integrity-scripts-unleashed/). Looks like some pretty cool stuff---great work, Clint!

* For the past couple of weeks I've been (slowly) reading Kevin Jackson's _OpenStack Cloud Computing Cookbook_; it's very useful. It's worth a read if you want to get up to speed on OpenStack; naturally, you can get it [from Amazon](http://www.amazon.com/OpenStack-Cloud-Computing-Cookbook-Jackson/dp/1849517320/ref=la_B009HPUFRW_1_1?ie=UTF8&qid=1351291152&sr=1-1).

## Operating Systems/Applications

* At the intersection of cloud-based storage and configuration management, I happened to find this very interesting Puppet module designed to [fetch and update files from an S3 bucket](http://puppetlabs.com/blog/module-of-the-week-branan-s3file/). Through this module, you could store files in S3 instead of using Puppet's built-in file server. (By the way, this module also works with OpenStack Swift as well.)

* One of the things I've complained about regarding newer versions of OS X is the "hiding" of the Unix underpinnings. Perhaps I should read [this book](http://www.tuaw.com/2012/10/15/tuaw-bookshelf-learning-unix-for-os-x-mountain-lion/) and see if my thinking is unfounded?

## Storage

* Chris Evans takes a look at Hyper-V 3.0's Virtual Fibre Channel feature in [this write-up](http://blog.thestoragearchitect.com/2012/10/05/windows-server-2012-windows-server-8?-virtual-fibre-channel/). From what I've read, it sounds like Hyper-V's NPIV implementation is more robust than VMware's broken and busted NPIV implementation. (If you don't know why I say that about VMware's implementation, ask anyone who's tried to use it.) The real question is this: is NPIV support in a hypervisor of any value any longer?

* Gina Minks (formerly of Dell, now with Inktank) recommended I have a look at [Ceph](http://ceph.com/) and [mentioned](http://ginaminks.com/wordpress/updates-from-the-land-of-the-cephalopods/) this post on [migrating to Ceph](http://www.hastexo.com/resources/hints-and-kinks/migrating-virtual-machines-block-based-storage-radosceph) (with a little libvirt thrown in).

* Gluster might be another project that I need to spend some time examining; this post on [using Gluster with oVirt 3.1](http://blog.jebpages.com/archives/ovirt-3-1-glusterized/) looks interesting. Anyone have any pointers for a Gluster beginner?

* Mirantis has a post about some [Nova Volume integration with Isilon](http://www.mirantis.com/blog/openstack-nova-volume-integration-with-isilon/). I've often said that I think scale-out platforms like Isilon (among others) are an important foundation for future storage solutions. It's cool to see some third-party development happening to integrate Isilon and OpenStack.

## Virtualization

* With Python seemingly all the rage these days, you might be interested in using Python with vSphere. If so, Cody Bunch can [hook you up right here](http://professionalvmware.com/2012/07/getting-started-with-psphere-on-osx/).

* Want to use USB 3 with QEMU-KVM? Looks like QEMU-KVM 1.1 added [experimental USB 3.0 support](http://www.linux-kvm.com/content/qemu-kvm-11-adds-experimental-support-usb-30) back in July of this year.

* Also from back in July, here's a blog post about [continued development on PCI passthrough in QEMU and Xen](http://blog.xen.org/index.php/2012/07/16/pci-passthrough-in-qemu/). More recently, there was this announcement about [Xen ARM in upstream Linux](http://blog.xen.org/index.php/2012/10/08/xen-arm-in-linux/) (making Xen the first hypervisor supported on the ARM platform). Cool.

* If you're interested in playing with oVirt 3.1 (the "cutting edge" version of RHEV; oVirt is to RHEV as Fedora is to RHEL), this post on [getting up and running with oVirt 3.1](http://blog.jebpages.com/archives/up-and-running-with-ovirt-3-1-edition/) might be helpful.

* I used this post on [virtualization with KVM on Ubuntu 12.04 LTS](http://www.howtoforge.com/virtualization-with-kvm-on-ubuntu-12.04-lts) when I was first getting started with KVM. The author of that post has now posted [an updated version for Ubuntu 12.10](http://blog.allanglesit.com/2012/10/linux-kvm-ubuntu-12-10-with-openvswitch/).

* VMware today released an update to vSphere 5.1 that brings compatibility with View 5.1x. There's [a VMware KB article](http://kb.vmware.com/kb/2035268) with all the details, and download links.

* Need to convert a VHD to a VHDX? Check [here](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/02/converting-a-vhd-to-a-vhdx.aspx) and [here](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/03/using-powershell-to-convert-a-vhd-to-a-vhdx.aspx).

* Want to manage VMware Tools using Puppet? Check out [this module](http://puppetlabs.com/blog/module-of-the-week-razorsedge-vmwaretools/), one of several that offer this functionality.

* Michael Webster has a great write-up on [replacing CA SSL certificates in vSphere 5.1](http://longwhiteclouds.com/2012/10/27/updating-ca-ssl-certificates-in-vsphere-5-1/). Thanks for all the effort pulling this together, Michael!

That's all for this time around. As always, courteous comments are welcome (encouraged, in fact!), so feel free to speak up in the comments below. I'd love to hear your feedback.
