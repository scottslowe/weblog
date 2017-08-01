---
author: slowe
categories: Information
comments: true
date: 2015-01-07T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Cloud
- Virtualization
- Storage
- HyperV
- VMware
- Linux
- Automation
- OpenStack
- OSS
- NSX
- OVS
title: 'Technology Short Take #47'
url: /2015/01/07/technology-short-take-47/
---

Welcome to Technology Short Take #47! This is the first Technology Short Take for 2015 and the first to be published on the new blog platform. I have quite a bit of information to share this time around, so buckle up and let's get started!

## Networking

* Michael Webster isn't a name that normally pops up here in the Networking section of my Technology Short Takes, but he recently wrote an article on [installing Cumulus Linux from a MacBook Pro][link-2] that I thought might be handy. I'm particularly jealous that Michael was able to get his hands on a Cumulus-supported switch while here I am---with a full NSX installation just ready to integrate with Cumulus---not making any progress on that front.
* Speaking of Cumulus Linux, here's a write-up on [using Cumulus Linux on Dell Networking switches][link-16]; in particular, this article describes how to install Cumulus Linux on a Dell S6000-ON. I spoke to some folks at Dell a while ago about getting my hands on a Cumulus-compatible switch, but never heard back. Sure would be nice...(hint, hint).
* The folks over at Weave (who are building a lightweight overlay networking solution for Docker containers) recently posted some thoughts on [life and Docker networking][link-3]. If you want to keep up on the various developments in Docker networking, there are some useful links in this post.
* Jason Edelman has a great post on [automated network diagrams with Schprokits and AutoNetkit][link-8]. I love how Jason---among others---is really pushing forward with network automation. Good stuff.
* OVS developers Ben Pfaff, Justin Pettit, and Ethan Jackson recently posted an article to Network Heresy talking about [improvements in Open vSwitch (OVS) performance][link-14]. This post is definitely worth reading if you're interested in the improvements that have been made to OVS over the last several versions.
* Interesting in giving OpenContrail a spin? Here's an article on [how to use Vagrant to spin up a single node test environment][link-27]. It would be cool if VMware would/could do the same with VMware NSX.
* Here's [a 2-D diagram of the Facebook Altoona network architecture][link-29] created by Jason Edelman, who thought the 2-D diagram was more readable and easier to understand (for him, at least).
* Open Network Linux (main website [here][link-31]; GitHub repo [here][link-32]) was recently brought to my attention. Big Switch's web site confirms that Open Network Linux is the foundation for their Switch Light OS; does anyone know if Cumulus Linux is also based on Open Network Linux?

## Servers/Hardware

* Juniper recently announced the OCX1100-48SX, a new hardware switching platform built to the Open Compute Project (OCP) specifications (more information [here][link-24]). Normally, I'd put this in the Networking section, but I'm highlighting it here because not only does it support Juniper's JunOS but also supports other ONIE-compatible network operating systems as well (Cumulus Linux, anyone?). The interesting part in this announcement is the hardware---does this mark the rise of "brite-box" ("brite-box" = branded whitebox) switching? How long before other major networking hardware vendors will have to follow suit? Or will this be a flash in the pan, with no lasting impact on the networking hardware industry? Time will tell. (I think it _will_ make a difference.)

## Security

* Did you catch the news about HyTrust making DataControl (their data encryption product) [available via the AWS Marketplace][link-9]? This is a smart move, in my opinion---data security and data integrity are often cited as potential concerns for using public cloud services, and so HyTrust responds with a solution designed for that very purpose. I'm curious to see the uptake of this solution.
* This news is a few weeks old now, but I wanted to be sure to include it here in order to ensure that it gets the broadest distribution possible. All versions of Docker prior to version 1.3.2 have a security vulnerability that could allow host privilege escalation; more details [here][link-30]. Be sure to upgrade to close this potential security hole.

## Cloud Computing/Cloud Management

* Did you see the big announcement from AWS re:Invent about EC2 Container Service? More details are available [here][link-4]. Also, Aaron Delp has a nice series of live blogs from re:Invent, so check out his site [here][link-5].
* Ben Kepes also has [a nice write-up of some of the announcements][link-6] from re:Invent.
* Mark Shuttleworth (of Canonical) [shares his thoughts here][link-12] on what constitutes OpenStack "core."
* Puppet Labs recently announced a Puppet module to provision AWS infrastructure. The blog post announcing the module is [here][link-18]; the module's GitHub page is [here][link-19].
* Although the title talks about deploying OpenStack with multiple hypervisors, [this article][link-22] focuses more on setting up VMware vSphere with OpenStack. Fortunately it does bring to light one thing that often gets overlooked---the need to manage multiple Glance images when using multiple hypervisors.
* Congress---the policy framework project in which I'm involved---is starting to get more attention, which is definitely a good thing. I recently came across [this post on Congress][link-23], which provides an overview of Congress and supplies some links to useful resources on Congress. Keep up the Congress coverage, Melissa!
* Eran Gampel has a three-part series on the Neutron Distributed Virtual Router (DVR) functionality in OpenStack Juno that might be worth reading ([part 1][link-33], [part 2][link-34], and [part 3][link-35]). BTW, there's plenty of other good content on Eran's site as well, so feel free to check it out.

## Operating Systems/Applications

* Is Docker ready for production? Frederic de Villamil shares his thoughts [after 2 weeks of hands-on time with Docker][link-7]. The end result: in his view, Docker isn't production-ready in a complex environment because it adds too much complexity. Some of the areas Frederic identifies as issues include logging, network management, and process monitoring. That's not to say that Docker isn't useful, just that it might not be the "be all end all" that some are claiming it is.
* Maybe it's just because I know Tom Howarth, but [his post on "Containers: The Emperor's New Clothes"][link-13] comes across a bit like a crotchety old man yelling "Get off my lawn!" (No offense, Tom!) However, Tom does bring up some great points regarding the claimed benefits of containers over VMs. In the end, the onus is on the user to evaluate these solutions in the context of the underlying business need, and then pick the technology that provides the most effective and most efficient solution to the problem. On that point, I think Tom and I are in violent agreement.
* CoreOS rocked (no pun intended) the container world recently with [their announcement of Rocket][link-26], their own container runtime. Some say [this split was predictable][link-28]; others were caught off guard. It's too early to tell what will become of the "Docker vs. Rocket" debate, but the open discussion that's being generated by the announcement certainly doesn't hurt anyone.

## Storage

* If you, like me, have been seeing "NVMe" around a lot but weren't quite sure what the story was, this [beginner's guide to NVMe][link-1] by J Metz is _definitely_ worth reading.
* Is optimizing storage traffic a "killer application" for OpenFlow? Matt Oswalt has [a write-up][link-10] on how Coho uses OpenFlow to intelligently steer traffic to and from various nodes in their storage array.
* I'm not really sure of the purpose of running a file system as a set of Docker containers except to say "Look what I did," but here's a post on [running XtreemFS in Docker containers][link-15].

## Virtualization

* Niklas Askerlund has a post describing [the hot add/remove memory feature in the Hyper-V technical preview][link-11]. There are some limitations to how/when it can be used, but that is inline with the corresponding feature in vSphere.
* Darren O'Connor has [a write-up on an ESXi white box server][link-17] he recently built to run multiple VMs and virtual routers.
* James Thorne shows [a multi-machine Vagrantfile using JSON and loops][link-20] that is a variant of the YAML-powered version I shared [here][xref-1].
* Lots of announcements from DockerCon EU recently, including Docker Machine. Did anyone happen to notice [VMware's early support for Docker Machine][link-25]?

It's time to wrap up now; this post has already gotten too long. (What can I say? There's just so much good information out there!) I hope you were able to find something useful here.


[link-1]: http://sniaesfblog.org/?p=368
[link-2]: http://longwhiteclouds.com/2014/11/13/installing-cumulus-linux-from-a-macbook-pro/
[link-3]: http://weaveblog.com/2014/11/13/life-and-docker-networking/
[link-4]: https://aws.amazon.com/blogs/aws/cloud-container-management/
[link-5]: http://www.aarondelp.com
[link-6]: http://www.forbes.com/sites/benkepes/2014/11/13/more-amazonian-announcements-aws-anoints-docker-and-makes-a-nod-towards-star-wars/
[link-7]: https://t37.net/is-docker-ready-for-production-feedbacks-of-a-2-weeks-hands-on.html
[link-8]: http://www.jedelman.com/home/automated-network-diagrams-with-schprokits-autonetkit
[link-9]: http://www.businesswire.com/news/home/20141111005354/en/HyTrust-HyTrust-DataControl™-AWS-Marketplace#.VGI19PnF_14
[link-10]: http://keepingitclassless.net/2014/11/openflow-based-storage-traffic-steering-coho-data/
[link-11]: http://vniklas.djungeln.se/2014/11/11/hot-addremove-memory-on-a-hyper-v-vm-in-technical-preview/
[link-12]: http://www.markshuttleworth.com/archives/1428
[link-13]: http://www.virtualizationpractice.com/containers-emperors-new-clothes-29439/
[link-14]: http://networkheresy.com/2014/11/13/accelerating-open-vswitch-to-ludicrous-speed/
[link-15]: http://xtreemfs.blogspot.jp/2014/10/xtreemfs-in-docker-containers.html
[link-16]: http://humairahmed.com/blog/?p=7820
[link-17]: https://mellowd.co.uk/ccie/?p=5746
[link-18]: http://puppetlabs.com/blog/provision-aws-infrastructure-using-puppet
[link-19]: https://github.com/puppetlabs/puppetlabs-aws
[link-20]: http://thornelabs.net/2014/11/13/multi-machine-vagrantfile-with-shorter-cleaner-syntax-using-json-and-loops.html
[link-22]: http://www.cloudenablers.com/blog/deploying-openstack-with-multi-hypervisor-environment/
[link-23]: http://vmiss.net/openstack/openstack-congress-policy-for-your-cloud/
[link-24]: http://forums.juniper.net/t5/Data-Center-Technologists/Juniper-OCX1100-48SX-Technical-Deep-Dive/ba-p/265370
[link-25]: https://github.com/cloudnativeapps/machine/releases/tag/vmw_tech_preview
[link-26]: https://coreos.com/blog/rocket/
[link-27]: http://www.opencontrail.org/use-vagrant-to-bring-up-a-test-only-single-node-opencontrail-1-20-system/
[link-28]: http://danielcompton.net/2014/12/02/modular-integrated-docker-coreos
[link-29]: http://www.jedelman.com/home/facebook-altoona-network-diagram-in-2-d
[link-30]: http://www.openwall.com/lists/oss-security/2014/11/24/5
[link-31]: http://opennetlinux.org/
[link-32]: https://github.com/opennetworklinux/ONL
[link-33]: http://blog.gampel.net/2014/12/openstack-neutron-distributed-virtual.html
[link-34]: http://blog.gampel.net/2014/12/openstack-dvr2-floating-ips.html
[link-35]: http://blog.gampel.net/2015/01/openstack-DVR-SNAT.html
[xref-1]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
