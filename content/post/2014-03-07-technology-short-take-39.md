---
author: slowe
categories: Information
comments: true
date: 2014-03-07T09:51:19Z
slug: technology-short-take-39
tags:
- Hardware
- Intel
- iSCSI
- Networking
- Neutron
- NSX
- OpenStack
- OVS
- Security
- Storage
- Virtualization
- VSAN
- Windows
- Microsoft
- Automation
- HyperV
title: 'Technology Short Take #39'
url: /2014/03/07/technology-short-take-39/
wordpress_id: 3416
---

Welcome to Technology Short Take #39, in which I share a random assortment of links, articles, and thoughts from around the world of data center-related technologies. I hope you find something useful---or at least something interesting!

## Networking

* Jason Edelman has been talking about the idea of a Common Programmable Abstraction Layer (CPAL). He [introduces the idea](http://www.jedelman.com/1/post/2014/02/common-programmable-abstraction-layer.html), then goes on to explore---as he puts it---the [power of a CPAL](http://www.jedelman.com/1/post/2014/02/the-power-of-a-programmable-abstraction-layer.html). I can't help but wonder if this is the right level at which to put the abstraction layer. Is the abstraction layer better served by being integrated into a cloud management platform, like OpenStack? Naturally, the argument then would be, "Not everyone will use a cloud management platform," which is a valid argument. For those customers who won't use a cloud management platform, I would then ask: will they benefit from a CPAL? I mean, if they aren't willing to embrace the abstraction and automation that a cloud management platform brings, will abstraction and automation at the networking layer provide any significant benefit? I'd love to hear others' thoughts on this.

* Ethan Banks also [muses on the need for abstraction](http://ethancbanks.com/2014/02/10/abstract-all-the-things-or-why-clis-are-in-my-way/).

* Craig Matsumoto of SDN Central [helps highlight](http://www.sdncentral.com/news/valentines-day-draft-virtual-network-group-hug/2014/02/) a recent (and fairly significant) development in networking protocols---the submission of [the Generic Network Virtualization Encapsulation (Geneve) proposal](http://tools.ietf.org/html/draft-gross-geneve-00) to the IETF. Jointly authored by VMware, Microsoft, Red Hat, and Intel, this new protocol proposal attempts to bring together the strengths of the various network virtualization encapsulation protocols out there today (VXLAN, STT, NVGRE). This is interesting enough that I might actually write up a separate blog post about it; stay tuned for that.

* Lee Doyle provides [an analysis of the market for network virtualization](http://www.lightreading.com/carrier-sdn/sdn-architectures/understanding-the-market-for-network-virtualization/a/d-id/707801), which includes some introductory information for those who might be unfamiliar with what network virtualization is. I might contend that Open vSwitch (OVS) alone isn't an option for network virtualization, but that's just splitting hairs. Overall, this is a quick but worthy read if you are trying to get started in this space.

* Don't think this "software-defined networking" thing is going to take off? Read [this](http://aws.typepad.com/aws/2011/03/new-approach-amazon-ec2-networking.html), and then let me know what you think.

* Chris Margret has [a nice dissection of how bash completion works](http://www.fragmentationneeded.net/2014/02/tab-completion-on-cumulus-linux.html), particularly in regards to the Cumulus Networks implementation.

## Servers/Hardware

* Via Kevin Houston, you can get [more details on the Intel E7 v2 and new blade servers based on the new CPU](http://bladesmadesimple.com/2014/02/new-blade-servers-based-on-intel-e7-v2-announced/). x86 marches on!

* Another interesting tidbit regarding hardware: it seems as if we are now seeing the emergence of another round of "hardware offloads." The first round came about around 2006 when Intel and AMD first started releasing their hardware assists for virtualization (Intel VT and AMD-V, respectively). That technology was only "so-so" at first (VMware ESX continued to use binary translation [BT] because it was still faster than the hardware offloads), but it quickly matured and is now leveraged by every major hypervisor on the market. This next round of hardware offloads seems targeted at network virtualization and related technologies. Case in point: a relatively small company named Netronome (I've spoken about them previously, [first back in 2009][1] and [again a year later][2]), recently announced a new set of network interface cards (NICs) expressly designed to provide hardware acceleration for software-defined networking (SDN), network functions virtualization (NFV), and network virtualization solutions. You can get more details from [the Netronome press release](http://netronome.com/march-4-2014-netronome-launches-data-plane-hardware-and-software-for-sdn-and-nfv-designs/). This technology is actually quite interesting; I'm currently talking with Netronome about testing it with VMware NSX and will provide more details as that evolves.

## Security

* Ben Rossi tackles the subject of [security in a software-defined world](http://www.information-age.com/technology/security/123457721/rethinking-security-software-defined-world), talking about how best to integrate security into SDN-driven architectures and solutions. It's a high-level article and doesn't get into a great level of detail, but does point out some of the key things to consider.

## Cloud Computing/Cloud Management

* "Racker" James Denton has some nice articles on OpenStack Neutron that you might find useful. He starts out with discussing [the building blocks of Neutron](http://developer.rackspace.com/blog/neutron-networking-the-building-blocks-of-an-openstack-cloud.html), then goes on to discuss building [a simple flat network](http://developer.rackspace.com/blog/neutron-networking-simple-flat-network.html), using [VLAN provider networks](http://developer.rackspace.com/blog/neutron-networking-vlan-provider-networks.html), and [Neutron routers and the L3 agent](http://developer.rackspace.com/blog/neutron-networking-l3-agent.html). And if you need a breakdown of provider vs. tenant networks in Neutron, [this post](http://developer.rackspace.com/blog/beginning-to-understand-neutron-provider-and-tenant-networks-in-openstack.html) is also quite handy.

* Here's a couple ([first one](http://geekdocssecurity.blogspot.com/2014/02/installing-openstack-havana-on-ubuntu.html), [second one](http://cloudenablers.wordpress.com/2014/02/04/installing-openstack-all-in-one-node-using-single-nic/)) of quick walk-throughs on installing OpenStack. They don't provide any in-depth explanations of what's going on, why you're doing what you're doing, or how it relates to the rest of the steps, but you might find something useful nevertheless.

* Thinking of building your own OpenStack cloud in a home lab? Kevin Jackson---who along with Cody Bunch co-authored the _OpenStack Cloud Computing Cookbook, 2nd Edition_--has three articles up on his home OpenStack setup. (At least, I've only found three articles so far.) Part 1 is [here](http://openstackr.wordpress.com/2014/02/02/home-rackspace-private-cloud-openstack-lab-part-1/), part 2 is [here](http://openstackr.wordpress.com/2014/02/03/home-rackspace-private-cloud-openstack-lab-part-2/), and part 3 is [here](http://openstackr.wordpress.com/2014/02/11/home-rackspace-private-cloud-openstack-lab-part-3/). Enjoy!

* This post attempts to describe some of the core (mostly non-technical) [differences between OpenStack and OpenNebula](http://opennebula.org/opennebula-vs-openstack-user-needs-vs-vendor-driven/). It is published on the OpenNebula.org site, so keep that in mind as it is (naturally) biased toward OpenNebula. It would be quite interesting to me to see a more technically-focused discussion of the two approaches (and, for that matter, let's include CloudStack as well). Perhaps this already exists---does anyone know?

* CloudScaling recently added a Google Compute Engine (GCE) API compatibility module to StackForge, to allow users to leverage the GCE API with OpenStack. See more details [here](http://www.cloudscaling.com/blog/openstack/gce-api-available-now-on-openstack-stackforge/).

* Want to run Hyper-V in your OpenStack environment? Check [this](http://www.cloudbase.it/openstack-havana-2013-2-2-hyper-v-compute-installer-released/) out. Also from the same folks is [a version of cloud-init for Windows instances](http://www.cloudbase.it/cloud-init-for-windows-instances/) in cloud environments. I'm testing this in my OpenStack home lab now, and hope to have more information soon.

## Operating Systems/Applications

* Keep hearing the term "DevOps," but all the explanations you read are rather obtuse and obscure? I found this pair of articles on DevOps in straight English to be helpful. Read [part 1 here](http://developerblog.redhat.com/2014/01/15/devops-in-straight-english-part-1-of-2/) and [part 2 here](http://developerblog.redhat.com/2014/01/29/devops-straight-english-2-of-2/).

* Test your knowledge of UNIX and UNIX-like operating systems by seeing how many of these [less commonly used UNIX commands](http://www.danielmiessler.com/blog/collection-of-less-commonly-used-unix-commands) you recognize.

* Steve Jin [talks about MultiSSH](http://www.doublecloud.org/2014/01/multissh-productivity-multiplier-for-managing-multiple-servers-like-esxi/), a tool that allows you to connect to many servers and execute commands on them simultaneously. Cool. You can get more information about MultiSSH from [the Sourceforge project page](http://multissh.sourceforge.net).

* The Dell TechCenter Networking wiki has an article on [using Open vSwitch (OVS) and OpenFlow on Fedora 16](http://en.community.dell.com/techcenter/networking/w/wiki/3820.openvswitch-openflow-lets-get-started.aspx) that you might find helpful. It's a bit out of date, but the basics should still be applicable.

* By the way, have you heard about [Windows PowerShell Desired State Configuration](http://blogs.msdn.com/b/powershell/archive/2013/11/01/configuration-in-a-devops-world-windows-powershell-desired-state-configuration.aspx)? (There goes that great Microsoft naming expertise again.)

## Storage

* In late January Chad Sakac of EMC wrote an impressively long and detailed blog post [describing the four basic storage architectures](http://virtualgeek.typepad.com/virtual_geek/2014/01/understanding-storage-architectures.html) into which all products invariably fall. If you're looking to get some storage geek on, this is worth a read.

* One of the big news items recently was the formal "launch" of VMware VSAN (somebody needs to enforce some consistency on the whole "v" versus "V" thing in VMware product naming). Rather than try to provide you with a meager set of VSAN links, I will instead point you to [Eric Siebert's impossibly long and impressively complete list of VSAN links](http://vsphere-land.com/vsphere-links/vsan-links.html). Enjoy.

## Virtualization

* Brendan Gregg of Joyent has an [interesting write-up comparing virtualization performance](http://dtrace.org/blogs/brendan/2013/01/11/virtualization-performance-zones-kvm-xen/) between Zones (apparently referring to Solaris Zones, a form of OS virtualization/containerization), Xen, and KVM. I might disagree that KVM is a Type 2 hardware virtualization technology, pointing out that Xen also requires a Linux-based dom0 in order to function. (The distinction between a Type 1 that requires a general purpose OS in a dom0/parent partition and a Type 2 that runs on top of a general purpose OS is becoming increasingly blurred, IMHO.) What I _did_ find interesting was that they (Joyent) run a ported version of KVM inside Zones for additional resource controls and security. Based on the results of his testing---performed using DTrace---it would seem that the "double-hulled virtualization" doesn't really impact performance.

* Pete Koehler---via Jason Langer's blog---has a nice post on [converting in-guest iSCSI volumes to native VMDKs](http://www.virtuallanger.com/2014/03/04/converting-in-guest-iscsi-volumes-to-native-vmdks/). If you're in a similar situation, check out the post for more details.

* [This is interesting](http://www.vladan.fr/how-to-install-android-kitkat-in-vmware-workstation/). Useful, I'm not so sure about, but definitely interesting.

* If you are one of the few people living under a rock who doesn't know about PowerCLI, [Alan Renouf is here to help](http://www.virtu-al.net/2014/02/24/introduction-powercli/).

It's time to wrap up; this post has already run longer than usual. There was just so much information that I want to share with you! I'll be back soon-ish with another post, but until then feel free to join (or start) the conversation by adding your thoughts, ideas, links, or responses in the comments below.

[1]: {{< relref "2009-08-04-netronome-and-io-virtualization.md" >}}
[2]: {{< relref "2010-10-24-netronome-1-year-later.md" >}}
