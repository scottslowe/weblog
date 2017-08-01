---
author: slowe
categories: Information
comments: true
date: 2016-08-12T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OpenStack
- Ansible
- NSX
- VMware
- AWS
- Docker
- Linux
- SSH
- VMFS
- Kubernetes
- Git
title: 'Technology Short Take #70'
url: /2016/08/12/technology-short-take-70/
---

Welcome to Technology Short Take #70! In this post you'll find a collection of links to articles discussing the major data center technologies---networking, hardware, security, cloud computing, applications, virtualization...you name it! (If there's a topic you think I'm missing, I'd love to hear from you.)

## Networking

* MTU in OpenStack Neutron has been, as [this article by Sam Yaple][link-8] points out, a bit of a touchy subject. Fortunately, it looks like progress has been made on that front, so check out Sam's post for more details.
* Jason Edelman has [an article][link-9] from back in January that describes the use of Big Switch's Big Cloud Fabric (BCF) and Big Monitoring Fabric (BMF) in conjunction with Ansible (via some Ansible modules that Jason himself developed).
* Dwayne Sinclair covers the basics of SpoofGuard in NSX, and how to interact with SpoofGuard via API, in [this article][link-12].
* [This article][link-13] is a bit more OpenStack-focused, but given that it focuses pretty heavily on Neutron I thought it'd fit better here in the "Networking" section. The article talks about how to use the `--allowed_address_pairs` extension to build a highly-available proxy server instead of using LBaaS.
* Numan Siddique describes the [native DHCP support available in OVN][link-15] (Open Virtual Network).
* Thinking of using a hardware VTEP (VXLAN Tunnel Endpoint) with VMware NSX? Check out [this article][link-16] by Dmitri Kalintsev.
* Jeremy Stretch has [a good article][link-21] on using `dumpcap`, part of the Wireshark package, for long-term packet captures.
* [This][link-27] looks neat---I need to try to find time to give it a spin.

## Servers/Hardware

* VMware [recently announced Open Hardware Management Services (OHMS)][link-19], a project intended to help manage servers and switches in a software-defined data center (SDDC) context. I'm particularly encouraged by 2 things about OHMS. First, OHMS is open source (find it [here on GitHub][link-20]); second, OHMS appears to interact with/integrate with/support Redfish, a hardware-level API I first discussed [back in 2014][xref-1].
* What will happen when you combine GPUs and persistent storage? According to [this article][link-23], "It is hard to overstate what a sea change" this sort of architecture will create. It seems to me that the ever-increasing application of persistent storage technologies in lots of difference places is going to change lots and lots of things.

## Security

* Michael Endrizzi, a self-proclaimed Check Point fanatic, spent some time working with VMware NSX's security features earlier this year. I saw two articles talking about his experience: one on [redirecting NSX firewall logs into SmartLog][link-1] and a second one ranting on [how the NSX DFW isn't quite enterprise ready][link-2].
* Marco van Baggum describes his experience in working with [NSX 6.2.3 and Trend Micro's Deep Security][link-14] in this article.
* For the para...err, security conscious folks, I submit [this][link-29].

## Cloud Computing/Cloud Management

* This is a slightly older article by Casey West on [the topic of "cloud-native."][link-6] We've all see this term thrown around quite a bit, but in this article I feel that Casey does a pretty good job of breaking it down into some practical aspects: frameworks, application architectures (such as the 12 factor app), runtimes, and infrastructure automation. It's worth a read as a good "foundation" to better understanding the ideas behind cloud-native applications and cloud-native environments.
* Alex Galbraith has been doing a fair amount of blogging on Amazon AWS, so he decided to put up [an index page to Amazon AWS posts][link-11].

## Operating Systems/Applications

* Leonid Mamchenkov [builds][link-5] on my some of my articles on Ansible and SSH bastion hosts; I particularly liked the use of the "negation" in his example SSH configuration. This allows you to specify an entire domain (like `*.example.com`) but specifically exclude one host in that domain (like `bastion.example.com`). Good stuff!
* A lot of people have a hard time understanding the relationship between configuration management systems (such as Ansible, Chef, Puppet, and Salt) and Docker. After all, why would you need a configuration management system in a heavily Docker-ized environment? Well, in addition to needing to manage the Docker hosts, there may be other benefits as well. This article by Ansible is, quite naturally, biased toward Ansible but does provide some good points on [why Ansible helps make docker-compose better][link-7].
* Rajdeep Dua has written [an overview of the architecture of SwarmKit][link-22].
* Speaking of SwarmKit, Sreenivas Makam has an article [comparing Swarm, SwarmKit, and Swarm Mode][link-24] (from Docker 1.12). The article focuses more on user experience than the technical differences between the various implementations.
* Paul Bakker shares some [lessons learned after one year of using Kubernetes in production][link-25]. There's some valuable information here, in my opinion.
* Alexandre Beslic tackles the idea of [what could be next for container orchestration][link-26], talking about some topics that container orchestration systems/frameworks should address in upcoming releases.
* Here's [a fun little article][link-28] about using Ansible to provision a Raspberry Pi with the AWSCLI (command-line interface for AWS).

## Storage

* Robin Harris [takes a look at Symbolic IO's patents][link-3] in an effort to "de-hype" the marketing material.
* Mark Brookfield has an article on [recovering data from a damaged VMFS partition][link-10] that may be helpful to others, should they find themselves in a similar position.
* Is the [era of the storage admin over][link-17]? I do agree with the post that Linux skills are a good place to invest your time/energy, which is what I've been recommending for a few years now.

## Virtualization

* Version 2.0 of the HTML5 vSphere Client is here, and [here's a post on upgrading to the latest release][link-18].
* Have you checked out vSphere DSC yet? I'm more an Linux+Ansible guy myself, but for all you vSphere folks out there this is something you should _definitely_ be examining. Luc Dekens has [a great intro post][link-31] available.
* Jason Boche published [an article][link-32] describing an issue with VMware Tools and VM snapshots. The issue lies with VMware Tools, apparently; see Jason's post for full details.
* If you're new to VirtualBox as a hosted virtualization tool, then [this article on using VirtualBox VMs headless][link-33] (via `vboxheadless`) might be useful.

## Career/Soft Skills

* Want some free ebooks? No, this isn't a catch---go check out [this MSDN blog post by Eric Ligman][link-4]. I downloaded a few Azure books, since public cloud is a focus of mine this year.
* Have I mentioned what a _great_ resource [this article][link-30] is? I can't emphasize strongly enough how tools like Git are going to be useful to you moving forward.

OK folks, that's all for now! I'll keep my eyes peeled for content to add in future posts, and feel free to hit me up via social media (I'm not that hard to find) if you find anything you feel should be included in the next post. Until then, take care!



[link-1]: https://dreezman.wordpress.com/2016/02/02/redirecting-nsx-firewall-logs-into-smartlog/
[link-2]: https://dreezman.wordpress.com/2016/02/15/nsx-firewall-very-cool-enterprise-ready-toy/
[link-3]: http://storagemojo.com/2016/07/22/a-look-at-symbolic-ios-patents/
[link-4]: https://blogs.msdn.microsoft.com/mssmallbiz/2016/07/10/free-thats-right-im-giving-away-millions-of-free-microsoft-ebooks-again-including-windows-10-office-365-office-2016-power-bi-azure-windows-8-1-office-2013-sharepoint-2016-sha/
[link-5]: http://mamchenkov.net/wordpress/2016/07/24/ssh-multiplexing-and-ansible-via-bastion-host/
[link-6]: https://www.oreilly.com/ideas/the-cloud-native-future
[link-7]: https://www.ansible.com/blog/six-ways-ansible-makes-docker-compose-better
[link-8]: http://yaple.net/2016/03/22/openstack-neutron-openvswitch-and-jumbo-frames/
[link-9]: http://jedelman.com/home/big-switch-meets-ansible/
[link-10]: https://virtualhobbit.com/2016/08/01/recovering-data-from-damaged-vmfs-partitions/
[link-11]: http://tekhead.it/blog/2016/07/index-of-tekhead-it-blog-posts-on-amazon-aws/
[link-12]: http://www.beyondcli.com/301/nsx-v-spoofguard-via-api/
[link-13]: http://www.stratoscale.com/blog/compute/highly-available-lb-openstack-instead-lbaas/
[link-14]: http://www.vmbaggum.nl/2016/07/trend-micro-deep-security-and-nsx-6-2-3-issue/
[link-15]: http://blogs.rdoproject.org/7936/native-dhcp-support-in-ovn
[link-16]: https://telecomoccasionally.wordpress.com/2016/08/10/planning-deployment-of-a-hardware-vtep-with-nsx-for-vsphere/
[link-17]: http://www.sysadmintherapy.com/2016/08/storage-as-career-is-ending.html
[link-18]: http://blogs.vmware.com/vsphere/2016/08/vsphere-client-html5-v2-0-easy-upgrade.html
[link-19]: https://cto.vmware.com/announcing-open-hardware-management-services-ohms/
[link-20]: https://github.com/vmware/OHMS/
[link-21]: http://packetlife.net/blog/2011/mar/9/long-term-traffic-capture-wireshark/
[link-22]: http://containertutorials.com/swarmkit/architecture.html
[link-23]: https://semiaccurate.com/2016/07/25/amd-puts-massive-ssds-gpus-calls-ssg/
[link-24]: https://sreeninet.wordpress.com/2016/07/14/comparing-swarm-swarmkit-and-swarm-mode/
[link-25]: http://techbeacon.com/one-year-using-kubernetes-production-lessons-learned
[link-26]: http://www.abronan.com/what-could-be-next-for-container-orchestration/
[link-27]: https://github.com/CumulusNetworks/topology_converter
[link-28]: https://maxhemingway.com/2016/06/17/configuring-the-raspberry-pi-with-ansible-and-awscli/
[link-29]: https://blog.filippo.io/securing-a-travel-iphone/
[link-30]: https://codewords.recurse.com/issues/two/git-from-the-inside-out
[link-31]: http://www.lucd.info/2016/06/04/vspheredsc-intro/
[link-32]: http://www.boche.net/blog/index.php/2016/07/30/vmware-tools-causes-virtual-machine-snapshot-with-quiesce-error/
[link-33]: https://www.howtoforge.com/tutorial/running-virtual-machines-with-virtualbox-5.1-on-a-headless-ubuntu-16.04-lts-server/
[xref-1]: {{< relref "2014-09-22-thinking-about-intel-rack-scale-architecture.md" >}}