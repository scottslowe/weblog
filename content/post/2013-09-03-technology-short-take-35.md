---
author: slowe
categories: Information
comments: true
date: 2013-09-03T09:00:00Z
slug: technology-short-take-35
tags:
- CLI
- Networking
- OpenFlow
- OpenStack
- Puppet
- Security
- Storage
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #35'
url: /2013/09/03/technology-short-take-35/
wordpress_id: 3265
---

Welcome to Technology Short Take #35, another in my irregular series of posts that collect various articles, links and thoughts regarding data center technologies. I hope that something in here is useful to you.

## Networking

* Art Fewell takes a deeper look at the [increasingly important role of the virtual switch](http://www.networkworld.com/community/blog/battle-hypervisor-switch-and-future-networking).

* A discussion of "statefulness" brought me again to Ivan's post on [the spectrum of firewall statefulness](http://blog.ipspace.net/2013/03/the-spectrum-of-firewall-statefulness.html). It's so easy sometimes just to revert to "it's stateful" or "it's not stateful," but the reality is that it's not quite so black-and-white.

* Speaking of state, I like [this piece](http://blog.ipspace.net/2013/08/50-shades-of-statefulness.html) by Ivan as well.

* I tend not to link to TechTarget posts any more than I have to, because invariably the articles end up going behind a login requirement just to read them. Even so, this Q&A session with Martin Casado on [managing physical and virtual worlds in parallel](http://searchsdn.techtarget.com/news/2240203033/Martin-Casado-QA-Managing-physical-and-virtual-worlds-in-parallel) might be worth going through the hassle.

* [This](https://leanpub.com/the-openflow-book) looks interesting.

* VMware [introduced VMware NSX](http://blogs.vmware.com/networkvirtualization/2013/08/vmware-nsx.html) recently at VMworld 2013. Cisco shared [some thoughts](http://blogs.cisco.com/datacenter/limitations-of-a-software-only-approach-to-data-center-networking/) on what they termed a "software-only" approach; naturally, they have a different vision for data center networking (and that's OK). I was a bit surprised by some of the responses to Cisco's piece (see [here](http://simonthibaudeau.org/NetworkLayers/?p=98) and [here](http://www.networkworld.com/community/blog/response-padma-warriors-limitations-software-only-approach-data-center-networking)). In the end, though, I like Greg Ferro's [statement](http://etherealmind.com/musing-on-the-vmware-versus-cisco-thing/): "It is perfectly reasonable that both companies will 'win'." There's room for a myriad of views on how to solve today's networking challenges, and each approach has its advantages and disadvantages.

## Servers/Hardware

Nothing this time around, but I'll watch for items to include in future editions. Feel free to send me links you think would be useful to include in the future!

## Security

* I found [this write-up](http://www.geekempire.com/2013/07/virtual-security-onion-via-ubuntu-kvm.html) on using OVS port mirroring with Security Onion for intrusion detection and network security monitoring.

## Cloud Computing/Cloud Management

* Prasenjit Sarkar, who works in VMware R&D, and runs the [Stretch Cloud site](http://stretch-cloud.info/), has a write-up on [using Data Center Extension (DCE) to VMware vCloud Hybrid Service (vCHS)](http://stretch-cloud.info/2013/08/data-center-extension-to-vmware-vcloud-hybrid-service-aka-vchs/). It's a pretty detailed write-up, with step-by-step instructions and lots of screenshots. His follow-up article on [disaster recovery of DCE VM with vCHS](http://stretch-cloud.info/2013/08/disaster-recovery-of-stretched-vm-dce-in-vcloud-hybrid-service-aka-vchs/) is also pretty good. If you're considering how you might be able to use vCHS for your organization, these articles might really be worth your time.

* Chris Colotti has also been tackling some vCHS stuff. Relative to using DCE, Chris' post on [what's required for vSphere Stretch Deploy to work with vCHS](http://www.chriscolotti.us/vmware/hybrid-cloud-vmware/whats-required-for-vsphere-stretch-deploy-to-work-with-vchs/) is quite useful. Also be sure to check out his two-part series on using Stretch Deploy with vCHS ([part 1](http://www.chriscolotti.us/vmware/vcloud/how-to-setup-vsphere-stretch-deploy-to-vcloud-hybrid-service-part-1/) and [part 2](http://www.chriscolotti.us/vmware/vcloud/how-to-setup-vsphere-stretch-deploy-to-vcloud-hybrid-service-part-2/)). Finally, Chris' post on [migrating VMs to vCHS](http://www.chriscolotti.us/vmware/hybrid-cloud-vmware/how-to-migrate-vsphere-machines-to-vcloud-hybrid-service/) (the so-called "V2C" migration) is also a good article.

* I liked this write-up on the combination of [Puppet, Hiera, OpenStack, and Vagrant](http://openstack.prov12n.com/puppet-openstack-hiera-with-vagrant/).

* Kenneth Hui, formerly of VCE and now with Rackspace, does a great job of explaining how VMware vSphere fits into the OpenStack Nova architecture in [this blog post](http://www.rackspace.com/blog/architecting-vmware-vsphere-for-openstack/). He also has a series of blog posts running on his own site on OpenStack compute for vSphere admins ([part 1](http://cloudarchitectmusings.com/2013/06/24/openstack-for-vmware-admins-nova-compute-with-vsphere-part-1/), [part 2](http://cloudarchitectmusings.com/2013/06/26/openstack-for-vmware-admins-nova-compute-with-vsphere-part-2/), [part 3](http://cloudarchitectmusings.com/2013/07/09/openstack-compute-for-vsphere-admins-part-3-ha-and-vm-migration/), [part 4](http://cloudarchitectmusings.com/2013/08/05/openstack-compute-for-vsphere-admins-part-4-overcommitment-in-nova-compute/), and [part 5](http://cloudarchitectmusings.com/2013/08/22/openstack-compute-for-vsphere-admins-part-5-designing-a-multi-hypervisor-cloud/)). Very good stuff!

## Operating Systems/Applications

* In [past presentations](https://speakerdeck.com/slowe/5-thoughts-on-staying-sharp-and-relevant-boston) I've referenced the terms "snowflake servers" and "phoenix servers," which I borrowed from Martin Fowler. (I don't know if Martin coined the terms or not, but you can get more information [here](http://martinfowler.com/bliki/SnowflakeServer.html) and [here](http://martinfowler.com/bliki/PhoenixServer.html).) Recently among some of Martin's material I saw reference to yet another term: [the immutable server](http://martinfowler.com/bliki/ImmutableServer.html). It's an interesting construct: rather than managing the configuration of servers, you simply spin up new instances when you need a new configuration; existing configurations are never changed. More information on the use of the immutable server construct is also available [here](http://www.thoughtworks.com/insights/blog/rethinking-building-cloud-part-4-immutable-servers). I'd be interested to hear readers' thoughts on this idea.

## Storage

* Chris Evans [takes a took at ScaleIO](http://architecting.it/2013/08/07/scaleio-emcs-new-baby/), recently acquired by EMC, and speculates on where ScaleIO fits into the EMC family of products relative to the evolution of storage in the data center.

* While I was at VMworld 2013, I had the opportunity to talk with SanDisk's FlashSoft division about their flash caching product. It was quite an interesting discussion, so stay tuned for that update (it's almost written; expect it in the next couple of days).

## Virtualization

* The rise of new converged (or, as some vendors like to call it, "hyperconverged") architectures means that we have to consider the impact of these new architectures when designing vSphere environments that will leverage them. I found a few articles by fellow VCDX Josh Odgers that discuss the impact of Nutanix's converged architecture on vSphere designs. If you're considering the use of Nutanix, have a look at some of these articles (see [here](http://www.joshodgers.com/2013/07/02/storage-drs-and-nutanix-to-use-or-not-to-use-that-is-the-question/), [here](http://www.joshodgers.com/2013/08/07/vmware-host-isolation-response-in-a-nutanix-environment-nosan/), and [here](http://www.joshodgers.com/2013/08/07/example-architectural-decision-host-isolation-response-for-a-nutanix-environment/)).

* Jonathan Medd shows [how to clone a VM from a snapshot using PowerCLI](http://www.jonathanmedd.net/2013/07/clone-a-vm-from-a-snapshot-using-powercli.html). Also be sure to check out [this post](http://www.vmdev.info/?p=202) on the vSphere CloneVM API, which Jonathan references in his own article.

* Andre Leibovici shares [an unofficial way](http://myvirtualcloud.net/?p=4745) to disable the use of the SESparse disk format and revert to VMFS Sparse.

* Forgot the root password to your ESXi 5.x host? Here's a procedure for [resetting the root password for ESXi 5.x](http://www.vdsyn.com/resetting-the-root-password-for-esxi-5-x/) that involves booting on a Linux CD. As is pointed out in the comments, it might actually be easier to rebuild the host.

* vSphere 5.5 was all the rage at VMworld 2013, and there was a **lot** of coverage. One thing that I didn't see much discussion around was what's going on with the free version of ESXi. Vladan Seget gives a nice [update](http://www.vladan.fr/esxi-5-5-free-version-details/) on how free ESXi is changing with version 5.5.

* I am loving the micro-infrastructure series by my _VMware vSphere Design_ co-author, Forbes Guthrie. See it [here](http://www.vreference.com/2013/08/21/micro-infrastructure-server-with-openwrt-part-1/), [here](http://www.vreference.com/2013/08/22/micro-infrastructure-server-with-openwrt-part-2/), and [here](http://www.vreference.com/2013/08/23/micro-infrastructure-server-with-openwrt-part-3/).

It's time to wrap up now; I've already included more links than I normally include (although it doesn't seem like it). In any case, I hope that something I've shared here is helpful, and feel free to share your own thoughts, ideas, and feedback in the comments below. Have a great day!
