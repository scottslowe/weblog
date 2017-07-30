---
author: slowe
categories: Information
comments: true
date: 2013-03-05T18:17:43Z
slug: technology-short-take-30
tags:
- KVM
- Linux
- Networking
- OpenFlow
- OpenStack
- Puppet
- RedHat
- SDN
- Security
- Storage
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take #30'
url: /2013/03/05/technology-short-take-30/
wordpress_id: 3107
---

Welcome to Technology Short Take #30. This Technology Short Take is a bit heavy on the networking side, but I suppose that's understandable given my recent job change. Enjoy!

## Networking

* Ben Cherian, Chief Strategy Officer for Midokura, helps make a [case for network virtualization](http://allthingsd.com/20130124/making-a-case-for-network-virtualization/). (Note: Midokura makes a network virtualization solution.) If you're wondering about network virtualization and why there is a focus on it, this post might help shed some light. Given that it was written by a network virtualization vendor, it might seem a bit rah-rah, so keep that in mind.

* Brent Salisbury has a _fantastic_ series on OpenFlow. It's so good I wish I'd written it. He starts out by discussing [proactive vs. reactive flows](http://networkstatic.net/openflow-proactive-vs-reactive-flows/), in which Brent explains that OpenFlow performance is less about OpenFlow and more about how flows are inserted into the hardware. Next, he tackles the concerns over the scale of flow-based forwarding in his post on [coarse vs. fine flows](http://networkstatic.net/openflow-coarse-vs-fine-flows/). I love this quote from that article: "The second misnomer is, flow based forwarding does not scale. Bad designs are what do not scale." Great statement! The third post in the series tackles what Brent calls [hybrid SDN deployment strategies](http://networkstatic.net/openflow-sdn-hybrid-deployment-strategies), and Brent provides some great design considerations for organizations looking to deploy an SDN solution. I'm looking forward to the fourth and final article in the series!

* Also, if you're looking for some additional context to the TCAM considerations that Brent discusses in his OpenFlow series, check out this Packet Pushers blog post on [OpenFlow switching performance](http://packetpushers.net/openflow-switching-performance-not-all-tcam-is-created-equal/).

* Another one from Brent, this time on [Provider Bridging and Provider Backbone Bridging](http://networkstatic.net/putting-together-provider-bridging-provider-backbone-bridging-s-tags-and-c-tags/). Good explanation---it certainly helped me.

* [This article](http://www.securityweek.com/software-defined-networking-new-network-weakness) by Avi Chesla points out a potential security weakness in SDN, in the form of a DoS (Denial of Service) attack where many switching nodes request many flows from the central controller. It appears to me that this would only be an issue for networks using fine-grained, reactive flows. Am I wrong?

* Scott Hogg has a nice list of [9 common Spanning Tree mistakes](http://www.networkworld.com/community/blog/9-common-spanning-tree-mistakes) you shouldn't make.

* Schuberg Philis has a nice write-up of their CloudStack+NVP deployment [here](http://www.cupfighter.net/index.php/2013/01/schuberg-philis-cloud-l2-l3-use-case/).

## Servers/Hardware

* Alex Galbraith recently posted a two-part series on what he calls the "NanoLab," a home lab built on the Intel NUC ("Next Unit of Computing"). It's a good read for those of you looking for some very quiet and very small home lab equipment, and Alex does a good job of providing all the details. Check out [part 1 here](http://www.tekhead.org/blog/2013/01/nanolab-running-vmware-vsphere-on-intel-nuc-part-1/) and [part 2 here](http://www.tekhead.org/blog/2013/01/nanolab-running-vmware-vsphere-on-intel-nuc-part-2-2/).

* At first, I thought this article was written from a sarcastic point of view, but it turns out that Kevin Houston's post on [5 reasons why you may not want blade servers](http://bladesmadesimple.com/2013/02/5-reasons-you-may-not-want-blade-servers/) is the real deal. It's nice to see someone who focuses on blade servers opening up about why they aren't necessarily the best fit for all situations.

## Security

* Nick Buraglio has a good post on [the potential impact of Arista's new DANZ functionality](http://www.forwardingplane.net/2013/02/watch-out-gigamon-and-others-arista-is-bringing-their-a-game/) on tap aggregation solutions in the security market. It will be interesting to see how this shapes up. BTW, Nick's writing some pretty good content, so if you're not subscribed to his blog I'd reconsider.

## Cloud Computing/Cloud Management

* Although this post is a bit older (it's from September of last year), it's still an interesting [comparison of both OpenStack and CloudStack](http://www.mirantis.com/blog/an-openstack-guy-takes-cloudstack-for-a-test-drive/). Note that the author apparently works for Mirantis, which is a company that provides OpenStack consulting services. In spite of that fact, he manages to provide a reasonably balanced approach to comparing the two cloud management platforms. Both of them (I believe) have had releases since this time, so some of the points may not be valid any longer.

* Are you a CloudStack fan? If so, you should probably check out [this collection of links](http://www.aarondelp.com/2012/12/links-to-everything-cloudstack.html) from Aaron Delp. Aaron's focused a lot more on CloudStack now that he's at Citrix, so he might be a good resource if that is your cloud management platform of choice.

## Operating Systems/Applications

* If you're just now getting into the whole configuration management scene where tools like Puppet, Chef, and others play, you might find [this article](http://spin.atomicobject.com/2012/09/13/from-imperative-to-declarative-system-configuration-with-puppet/) helpful. It walks through the difference between configuring a system imperatively and configuring a system declaratively (hint: Puppet, Chef, and others are declarative). It does presume a small bit of programming knowledge in the examples, but even as a non-programmer I found it useful.

* Here's a three-part series on beginning Puppet that you might find helpful as well ([Part 1](http://justfewtuts.blogspot.com/2012/05/puppet-beginners-concept-guide-part-1.html), [Part 2](http://justfewtuts.blogspot.com/2012/07/puppet-beginners-concept-guide-part-2.html), and [Part 3](http://justfewtuts.blogspot.com/2012/08/puppet-beginners-concept-guide-part-3.html)).

* If you're a developer-type person, I would first ask why you're reading my site, then I'd point you to [this post on the AMQP, MQTT, and STOMP messaging protocols](http://blogs.vmware.com/vfabric/2013/02/choosing-your-messaging-protocol-amqp-mqtt-or-stomp.html).

## Storage

* There's a good post here by Manish Patel on [verifying VMFS heartbeat region corruption](http://virtualpatel.blogspot.com/2013/01/how-to-verify-vmfs-heartbeat-region.html).

* Jason Boche has a great, great post on [VAAI and the "unlimited VMs per datastore" urban myth](http://www.boche.net/blog/index.php/2013/02/28/vaai-and-the-unlimited-vms-per-datastore-urban-myth/). Jason clearly and logically lays out the benefits of VAAI (specifically, he focuses most on hardware-assisted locking) and how those benefits don't address other design considerations. Overall, an excellent article, and one I highly recommend.

## Virtualization

* Although these posts are storage-related, the real focus is on how the storage stack is implemented in a virtualization solution, which is why I'm putting them in this section. Cormac Hogan has a series going titled "Pluggable Storage Architecture (PSA) Deep Dive" ([part 1 here](http://cormachogan.com/2013/02/04/pluggable-storage-architecture-psa-deep-dive-part-1/), [part 2 here](http://cormachogan.com/2013/02/05/pluggable-storage-architecture-psa-deep-dive-part-2/), [part 3 here](http://cormachogan.com/2013/02/07/pluggable-storage-architecture-psa-deep-dive-part-3/)). If you want more PSA information, you'd be hard-pressed to find a better source. Well worth reading for VMware admins and architects.

* Chris Colotti shares information on a little-known vSwitch advanced setting that helps resolve an issue with multicast traffic and NICs in promiscuous mode in [this post](http://www.chriscolotti.us/vmware/vsphere/interesting-vmware-vswitch-advanced-setting/).

* Frank Denneman reminds everyone in [this post](http://frankdenneman.nl/2012/12/18/designing-your-vmotion-network/) that the concurrent vMotion limit only goes to 8 concurrent vMotions when vSphere detects the NIC speed at 10Gbps. Anything less causes the concurrent limit to remain at 4. For those of you using solutions like HP VirtualConnect or similar that allow you to slice and dice a 10Gb link into smaller links, this is a design consideration you'll want to be sure to incorporate. Good post Frank!

* Interested in some OpenStack inception? See [here](http://openstack.prov12n.com/nesting-openstack-in-openstack/). How about some oVirt inception? See [here](http://blog.jebpages.com/archives/ovirt-on-ovirt-nested-kvm-fu/). What's that? Not familiar with oVirt? No problem---see [here](http://blog.jebpages.com/archives/up-and-running-with-ovirt-3-1-edition/).

* [Windows Backup has native Hyper-V support in Windows Server 2012](http://blogs.msdn.com/b/virtual_pc_guy/archive/2013/02/18/windows-backup-and-hyper-v-in-server-2012.aspx). That's cool, but are you surprised? I'm not.

* Red Hat and IBM put out a [press release](http://www.redhat.com/about/news/archive/2013/2/red-hat-and-ibm-achieve-leading-performance-benchmark-results) today on improved I/O performance with RHEL 6.4 and KVM. The press release claims that a single KVM guest on RHEL 6.4 can support up to 1.5 million IOPS. (Cue timer until next virtualization vendor ups the ante)

I guess I should wrap things up now, even though I still have more articles that I'd love to share with readers. Perhaps a "mini-TST"

In any event, courteous comments are always welcome, so feel free to speak up below. Thanks for reading and I hope you've found something useful!
