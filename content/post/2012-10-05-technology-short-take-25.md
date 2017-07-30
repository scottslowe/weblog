---
author: slowe
categories: Information
comments: true
date: 2012-10-05T09:00:00Z
slug: technology-short-take-25
tags:
- Automation
- KVM
- Linux
- Networking
- OpenStack
- OSS
- OVS
- Security
- Storage
- vCloud
- Virtualization
- VMware
title: 'Technology Short Take #25'
url: /2012/10/05/technology-short-take-25/
wordpress_id: 2875
---

Welcome to Technology Short Take #25, my irregularly-published collection of links, articles, thoughts, and rants. It's been a while since my last Technology Short Take (almost three months!); my apologies for that. This is my first time publishing a Technology Short Take with my new filesystem-based approach of managing resources. We'll see how well it goes

In any case, let's get on with it!

## Networking

* Some folks from Nicira (now part of VMware) recently published [a blog post discussing the OVSDB IETF draft](http://networkheresy.com/2012/09/15/remembering-the-management-plane/) (see [here](https://datatracker.ietf.org/doc/draft-pfaff-ovsdb-proto/)). It's a valid point---people get all worked up over OpenFlow, but OpenFlow doesn't address the management plane (only the control plane). Unfortunately, the management plane is often the place where vendors choose to "innovate" and "differentiate" their offerings, which---in my humble view---makes any sort of standardization in the management plane extremely difficult. I could be wrong (wouldn't be the first time).

* I think this three-part series on new network models for cloud computing ([part 1](http://news.cnet.com/8301-19413_3-20119643-240/cloud-open-source-and-new-network-models-part-1/), [part 2](http://news.cnet.com/8301-19413_3-20121638-240/cloud-open-source-and-new-network-models-part-2/), and [part 3](http://news.cnet.com/8301-19413_3-20126245-240/clouds-open-source-and-new-network-models-part-3/)), while almost a year old, is quite good. James Urquhart, the author of the series, does a good job of breaking down some of the commonly-discussed "disruptive" technologies like Quantum (part of OpenStack) and OpenFlow. Worth a read if you are trying to get up to speed on these efforts, in my opinion.

* There's some additional information on the Quantum release on Folsom [here](https://www.ibm.com/developerworks/mydeveloperworks/blogs/e93514d3-c4f0-4aa0-8844-497f370090f5/entry/quantum_folsom12?lang=en).

* Erik Smith, notably known for his outstanding posts on storage and FCoE, takes a stab at describing some of the differences between SDN and network virtualization in [this post](http://brasstacksblog.typepad.com/brass-tacks/2012/08/network-virtualization-networkings-21st-century-equivalent-to-the-space-race.html).

* I found [this series of posts](https://brezular.wordpress.com/category/gns3/linux-switch/openvswitch/) to be helpful when I was working on configuring LACP with Open vSwitch (I hope to have a blog post on that up soon).

* Reading these [early OpenFlow meeting notes](http://networkstatic.net/2012/06/the-birth-unicorn-early-openflow-meeting-notes/) (via Brent Salisbury, aka @networkstatic on Twitter) was very fascinating. There's a lot to digest there (for me, anyway, there is a lot to digest).

## Servers/Hardware

Nothing this time around---but I'll keep my eyes peeled for interesting information to include next time!

## Security

* I came across [this post](http://blog.cloudfoundry.org/2012/07/23/uaa-intro/) on CloudFoundry's User Account and Authentication Service (the UAA). If you're seeking more information on UAA, this looks like a good place to start.

## Cloud Computing/Cloud Management

* This is an _awesome_ [overview of the OpenStack Folsom architecture](http://ken.pepple.info/openstack/2012/09/25/openstack-folsom-architecture/), courtesy of Ken Pepple. Definitely worth reading, in my view.

## Operating Systems/Applications

* I haven't had much time to spend working with Puppet (a shame, I really enjoy the product---hopefully I'll get back to it soon). When I do get back into working with Puppet again, I'm going to do my best to follow [this advice](http://bombasticmonkey.com/2011/12/27/stop-writing-puppet-modules-that-suck) regarding Puppet modules.

## Storage

* Sean Thulin has a nice write-up on [configuring VASA for use with a VNX](http://www.thulinaround.com/2012/08/05/configuring-vasa-for-use-with-a-vnx/).

* Is Cisco's Insieme effort producing a storage product? Some interesting speculation can be found [here](http://www.networkworld.com/community/blog/meet-ciscoinsiemes-recruiter) and [here](http://siwdt.com/2012/03/22/new-spin-in-called-insiemi-a-framing-exercise/) (hat tip to Erik Smith).

* Speaking of Erik Smiththis post on [the impact of bit errors on I/O consolidation](http://brasstacksblog.typepad.com/brass-tacks/2012/09/a-problem-with-io-consolidation-and-network-virtualization.html) is a great post. I learn something from just about every one of Erik's posts.

* Another great post by Jason Boche on [thin provisioning and VAAI UNMAP support](http://www.boche.net/blog/index.php/2012/06/28/storage-starting-thin-and-staying-thin-with-vaai-unmap/). He does a great job of pulling together resources and explaining how it all works, including some great practical advice for real-world usage.

* If storage is your thing---especially in VMware environments---I'd recommend having a look at Cormac Hogan's blog for his series on vSphere 5.1 storage enhancements. It starts [here](http://cormachogan.com/2012/09/04/vsphere-5-1-storage-enhancements-part-1-vmfs-5/).

* There's an interesting write-up here on [a globally distributed OpenStack Swift cluster](http://swiftstack.com/blog/2012/09/16/globally-distributed-openstack-swift-cluster/). What wasn't clear to me---I guess I'm just dense---was whether the functionality SwiftStack was describing in their post was actually in current releases of Swift (and, if so, is it only in their commercial Swift release, or the open source Swift versions) or if this was "pie-in-the-sky" thinking about functionality that should be added at some point in the future. Anyone have any clarification here?

## Virtualization

* Need to add an alias to your vCloud Director cell? Jason Boche [shows you how](http://www.boche.net/blog/index.php/2012/07/05/adding-an-ip-alias-to-the-vcloud-director-cell-server/).

* This is kind of a nice feature in Hyper-V 3: DHCP Guard. According to [this article](http://www.techrepublic.com/blog/networking/disable-guest-dhcp-with-hyper-v-dhcp-guard/5812) by Rick Vanover, this feature allows you to protect your network against rogue/unauthorized DHCP servers. Anyone actually tried this feature out yet (other than in a lab)?

* William Lam shows you [how to use ovftool to copy VMs directly between ESXi hosts](http://www.virtuallyghetto.com/2012/06/how-to-copy-vms-directly-between-esxi.html). That's pretty handy.

* Only a true geek would be interested in this, but here's some information on [running OpenBSD in KVM on Linux](http://scie.nti.st/2009/10/4/running-openbsd-4-5-in-kvm-on-ubuntu-linux-9-04). Given my past interest in OpenBSD and my present interest in KVM on Linux, this might be something I'll be trying myself soon. Sadly, it looks like that post is the author's last post in three yearsshame.

* I'm not sure if this should be considered "storage" or "virtualization," as the lines continue to blur every day. In any case, this article by Frank Denneman on [Storage DRS load balancing frequency](http://frankdenneman.nl/2012/05/storage-drs-load-balance-frequency/) might be useful to you.

* [This post](http://www.sebastien-han.fr/blog/2012/07/19/make-the-network-of-your-vms-fly-with-virtio-driver/) describes some of the benefits of KVM's VirtIO driver and how to use VirtIO with OpenStack. You'll note, by the way, that Nova uses [libvirt](http://libvirt.org) to manipulate KVM. This is one of the reasons I've been spending some time with libvirt---as part of the "glue" between Nova and KVM, I think it's important to understand how libvirt works. (This is also why I've been spending time with Open vSwitch, which is a critical construct in Quantum.)

I suppose I should wrap things up now. Feel free to speak up in the comments if you found something I included here useful, or if there's additional information that would benefit other readers. All courteous comments are welcome!
