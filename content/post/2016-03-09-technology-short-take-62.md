---
author: slowe
categories: Information
comments: true
date: 2016-03-09T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- NSX
- Cisco
- Cumulus
- Linux
- OpenFlow
- Microsoft
- Intel
- OpenStack
- Docker
- CoreOS
- Ansible
title: 'Technology Short Take #62'
url: /2016/03/09/technology-short-take-62/
---

Welcome to Technology Short Take #62. Sorry for the long delay since the last TST; some global travel has really thrown my schedule off. In any case, I didn't want to wait until Friday (the day I normally publish these) so I'm putting this out there sooner. Enjoy!

## Networking

* PowerNSX is pretty cool---it's a set of PowerShell cmdlets for administrators of VMware NSX running in vSphere environments. Check out Anthony Burke's [introductory post on PowerNSX][link-1] for more details.
* Seen [this][link-9]? (Cue the round of folks claiming that this is why proprietary network operating systems [NOSes] are the route the networking industry should be taking.)
* Tony Sangha has a nice article providing step-by-step instructions [on setting up a site-to-site IPSec VPN between VMware NSX and a Cisco CSR 1000v][link-10].
* Roie Ben Haim has an article on [how to improve the NSX GUI user experience][link-19].
* Cumulus Networks recently shifted their pricing and licensing model toward perpetual licenses; [this article][link-20] has more information and a comparison of the old vs. new models.
* [This article][link-21] by Red Hat provides a reasonable overview of various networking options available in OpenStack (specifically, Red Hat's OpenStack distribution) to support Network Functions Virtualization (NFV). The article specifically calls out SR-IOV as well as Intel Data Plane Development Kit (DPDK) support within Open vSwitch (OVS).
* Eric Chou's [tutorial on OpenFlow with POX][link-24] underscores what is, I believe, OpenFlow's strength as well as its weakness. OpenFlow is enormously flexible (its strength), but with that flexibility comes a great deal of complexity (which is its weakness). This is not a knock against OpenFlow; the same could be said for any number of (relatively) low-level tools.
* By the way, if you're looking for a decent OpenFlow primer you could do worse than [Matt Oswalt's deep dive from July 2014][link-26].

## Servers/Hardware

* VCE---er, or is it EMC Converged Platforms Division?---recently announced VxRail, a new hyper-converged infrastructure appliance (HCIA). I'm sure there are tons of blog posts out there; here's [one][link-3].

## Security

* [This article][link-17] is an interesting read on some work being done by Microsoft Research to leverage Intel SGX to address some security concerns with today's cloud-heavy architectures/usage models. Recall from [Technology Short Take #45][xref-1] that I predicted SGX would be _huge_. I think this is just the beginning of how we'll see this functionality put to use.

## Cloud Computing/Cloud Management

* Via Jeff Barr on Twitter, I saw [this new tool from GiantSwarm called Kocho][link-4]. It's used to spin up CoreOS clusters on AWS. I plan to give it a try soon, so look for a blog post (at least) about it.
* I haven't spent any significant time using SaltStack (I use Ansible for almost all of my automation/configuration management needs), but here's an article on [using Salt Cloud to deploy EC2 instances][link-5] (Salt Cloud is part of SaltStack, as I understand it). This might be a useful resource if you're an AWS customer also using SaltStack.
* If you're considering OpenStack certification, [this article][link-7] breaks down some of the available certification options.
* Mesos is getting a lot of attention these days, so I found it really useful to read someone's thoughts on the [actual usability of Mesos from a developer's perspective][link-8]. There are some good (naturally), and also some bad (of course). At least with this article you're somewhat informed going into it as to whether it's the right solution for you and/or your organization.
* Trevor Roberts recently posted an article discussing [Windows images in your OpenStack cloud][link-18]. The article is a bit specific to VMware Integrated OpenStack (VIO), but some of the concepts---like the use of cloudbase-init---could apply to any OpenStack installation.
* You probably keep hearing people talk about "infrastructure as code," but may not fully understand what that means. Martin Fowler has [a good article][link-33] that lays it out pretty clearly.

## Operating Systems/Applications

* Arun Gupta has [a post][link-2] describing how to work around issues using the Docker REST API when working with Docker Machine on OS X.
* I guess all I can really say about [this][link-11] is...oops.
* There's been a fair amount of noise regarding moving the Docker official image library from Ubuntu to Alpine Linux (see Solomon's post [here][link-12]; this is the only "official" mention I've been able to find). Some [think this is great][link-13]; others, [not so much][link-14].
* Since we are talking about Docker images: here's a post on [creating a good, secure Docker base image][link-16]. The author does recommend using Alpine, but also recognizes that it may be necessary to add glibc in some cases to ease application compatibility. He provides a good example in the article by using Java.
* This article is about 18 months old, but I just found it a couple weeks ago. It lays out a way to [manage CoreOS instances using Ansible][link-23]. The trick here---and the reason that a post like this is needed---is that CoreOS doesn't ship with a Python interpreter installed, so you have jump through a couple of hoops to make Ansible work (Ansible requires a Python interpreter).
* Are you a developer? (Probably not, since this site isn't exactly tailored toward developers.) In any case, Fournova Software---the company behind the OS X Git client [Tower][link-35] (which I use regularly)---has [a developer survey here][link-34]. Might be worth a few minutes, especially since they're giving away free stuff.

## Storage

* [ZFS will be in the next Ubuntu Linux LTS release][link-6]. Sweet. (That's assuming all the legal junk can be resolved, which isn't a foregone conclusion.)
* Kenneth Hui has a couple of great articles on persistent storage with application containers (first one [here][link-27], then [this follow-up post][link-28]).
* Eric Sloof [points][link-30] readers toward a new white paper on VSAN 6.2 space efficiency technologies, like compression, deduplication, and erasure coding.

## Virtualization

* Alan Renouf has been publishing some videos on using PowerCLI; his latest shows how to manage the entire virtual machine lifecycle using PowerCLI. Check out the blog post (and the video) [here][link-29].
* William Lam has a write-up on [the future of the ESXi Embedded Host Client][link-31].
* Tom Howarth has [a write-up on the announcement of Horizon 7][link-32] that happened in early February (yes, I know I'm behind the times---sorry). If you're a VDI user or looking for a VDI solution, I'd recommend checking out this article as part of your research.

## Career/Soft Skills

* Anthony Spiteri takes a (wholly good-natured) [jab][link-15] at the seemingly-constant pressure for IT folks to evolve. I don't disagree with some of Anthony's statements---change _is_ hard, and gaining the expertise you've gained took hard work, lots of time, and plenty of blood/sweat/tears. For me, evolving _doesn't_ mean giving up expertise or knowledge; it means expanding that knowledge, building upon it, using it to propel you to new heights and new opportunities. That's just my viewpoint, though, so take it for what it's worth. Thanks for contributing to the conversation, Anthony!
* Want to get a job writing Python code? Read [this][link-22].
* If you're having trouble figuring out _why_ containers and microservices and the like are getting such attention, read Massimo's article on [how your shopping experience is related to Docker][link-25]. It's not extremely technical, and that is exactly why it's useful.

That's a wrap, folks! I fear if I include anything more I might cause some heads to explode, and we wouldn't want that. I hope you found something useful here!


[link-1]: http://networkinferno.net/powernsx
[link-2]: http://blog.couchbase.com/2016/february/enabling-docker-remote-api-docker-machine-mac-osx
[link-3]: http://www.inthedc.com/wp/moving-the-build-vs-buy-line-with-vxrail/
[link-4]: https://blog.giantswarm.io/kocho-bootstrapping-coreos-clusters-on-aws/
[link-5]: https://blog.jixee.me/saltstack-how-to-deploy-ec2-instances-with-salt-cloud/
[link-6]: http://blog.dustinkirkland.com/2016/02/zfs-is-fs-for-containers-in-ubuntu-1604.html
[link-7]: http://www.stratoscale.com/blog/it-leadership/openstack-certification-the-where-the-how-and-why/
[link-8]: http://container-solutions.com/mesos-usability-a-developers-perspective/
[link-9]: https://medium.com/vijay-pandurangan/linux-kernel-bug-delivers-corrupt-tcp-ip-data-to-mesos-kubernetes-docker-containers-4986f88f7a19#.c44ym7679
[link-10]: https://tonysangha.com/2015/09/14/nsx-edge-site-to-site-ipsec-vpn/
[link-11]: https://derflounder.wordpress.com/2016/02/28/apple-security-update-blocks-apple-ethernet-drivers-on-el-capitan/
[link-12]: https://news.ycombinator.com/item?id=11000827
[link-13]: https://www.brianchristner.io/docker-is-moving-to-alpine-linux/
[link-14]: http://technosophos.com/2016/02/25/the-alpine-mistake.html
[link-15]: http://anthonyspiteri.net/the-change-message-is-on-repeat-i-reckon-evolve/
[link-16]: http://heiber.im/post/creating-a-solid-docker-base-image/
[link-17]: http://www.esecurityplanet.com/network-security/microsoft-wants-to-fix-cloud-securitys-trust-problem.html
[link-18]: http://blogs.vmware.com/openstack/windows-images-in-your-openstack-cloud/
[link-19]: http://www.routetocloud.com/2014/08/improving-nsx-gui-user-experience/
[link-20]: https://bm-switch.com/index.php/blog/cumulus-linux-perpetual-model
[link-21]: http://redhatstackblog.redhat.com/2016/02/10/boosting-the-nfv-datapath-with-rhel-openstack-platform/
[link-22]: http://sedimental.org/getting_a_python_job.html
[link-23]: https://coreos.com/blog/managing-coreos-with-ansible/
[link-24]: http://blog.pythonicneteng.com/2013/02/openflow-tutorial-with-pox.html
[link-25]: http://it20.info/2016/01/how-is-your-shopping-experience-related-to-docker/
[link-26]: https://keepingitclassless.net/2014/07/sdn-protocols-2-openflow-deep-dive/
[link-27]: http://cloudarchitectmusings.com/2016/01/25/is-persistent-storage-good-for-containers/
[link-28]: http://cloudarchitectmusings.com/2016/02/10/follow-up-post-using-persistent-storage-with-containers/
[link-29]: http://blogs.vmware.com/PowerCLI/2016/03/managing-virtual-machine-lifecycle-powercli.html
[link-30]: http://www.ntpro.nl/blog/archives/3056-VMware-Virtual-SAN-6.2-Space-Efficiency-Technologies.html
[link-31]: http://www.virtuallyghetto.com/2016/03/the-future-of-the-esxi-embedded-host-client.html
[link-32]: https://www.virtualizationpractice.com/vmware-announces-horizon-view-7-37174/
[link-33]: http://martinfowler.com/bliki/InfrastructureAsCode.html
[link-34]: http://www.git-tower.com/p/mac-dev-survey
[link-35]: http://www.git-tower.com/
[xref-1]: {{< relref "2014-10-08-technology-short-take-45.md" >}}
