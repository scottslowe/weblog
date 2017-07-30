---
author: slowe
categories: Information
comments: true
date: 2016-06-30T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OVS
- Cisco
- VMware
- NSX
- OpenStack
- Linux
- Docker
- Kubernetes
- Apple
- KVM
- Vagrant
- Microsoft
- Windows
title: 'Technology Short Take #68'
url: /2016/06/30/technology-short-take-68/
---

Welcome to Technology Short Take #68, my erratically-published collection of links, articles, and posts from around the web---all focused on today's major data center technologies. I've been trying to stick to a schedule that has these posts published on a Friday, but given the pending holiday weekend I wanted to get this out a bit early. As always, I hope that something I've included here proves useful to you.

## Networking

* Brent Salisbury retweeted a ton of MACVLAN- and IPVLAN-related posts _right_ after I published the last Technology Short Take, so they had to get bumped to this one. First, we have [this post][link-1] on HiCube, which covers MACVLAN vs. IPVLAN. Next, we have a pair of articles by Sreenivas Makam; the first covers [MACVLAN and IPVLAN basics][link-2], while the second tackles [the Docker MACVLAN and IPVLAN network plugins][link-3]. Both articles are useful, IMHO, especially considering that MACVLAN is no longer experimental as of the Docker 1.12 release.
* I recently came across [this mention of in-band network telemetry (INT)][link-4], which looks _really_ powerful. I think this will be something worth watching.
* Oh, and while we're discussing P4: Diego Dompe has [an article talking about OpenSwitch and P4][link-7] that might be useful to read. I haven't played with OpenSwitch (or the OpenSwitch simulator), so I can't provide any feedback on the stuff Diego describes. (As a side note, almost every time I see "OpenSwitch" I have to do a double-take because I think it says "Open vSwitch".)
* Patrick Ogenstad outlines what's involved with [managing Cisco IOS upgrades with Ansible][link-21].
* If you like geeking out over the hardware side of networking, you may find [this Ars Technica article][link-27] on the physical infrastructure of the global Internet to be an interesting read. (I did.)
* Hannes Gredler has a post [speculating on the end of the router][link-28], where he discusses how new technologies may have dramatic effects on routers as we know them. As you read it, keep in mind that Hannes founded a company (RtBrick) to explore some of the things he talks about in the article.
* Here's [a good article][link-33] from my friend Ivan Pepelnjak on whether OVSDB is a management plane or control plane protocol. I won't spoil Ivan's conclusion; go read the article!

## Servers/Hardware

Nothing this time around, but I'll stay tuned for items to include in future posts.

## Security

* [Sysdig Falco][link-11], a behavioral security tool with support for containers (can run in a container and can monitor containers) looks like it could be a useful addition to your security toolset. It's early yet (Falco is only at version 0.1.0), so keep that in mind---[the blog post announcing Falco][link-12] specifically calls out performance as something they'll target in upcoming releases.
* I was also recently introduced to a company called [HexaTier][link-22] (formerly GreenSQL), which provides a database security product supporting both on-premises deployments as well as cloud-based deployments, and supports public cloud database-as-a-service (DBaaS) offerings. I plan to do a more in-depth write-up on HexaTier soon, after I've had a bit of time to do more research.
* In the event you accidentally locked yourself out of vCenter using NSX's distributed firewall, [this post by Roie Ben Haim][link-25] provides a workaround for getting yourself out of this pickle. Angel Villar Garea also has [a post on the same topic][link-29].

## Cloud Computing/Cloud Management

* Andrew Beekhof tackles some issues around [the evolving OpenStack HA architecture][link-32]; in particular, how and when Pacemaker should be used as more and more OpenStack services become able to operate in active/active configurations.

## Operating Systems/Applications

* How about [a Salt plugin for vRealize Orchestrator][link-8]?
* It seems like container management solutions are a dime a dozen these days, with more popping up constantly. The latest I've found: [Kontena][link-13].
* This is a _highly_ technical [article on scheduling in the Linux kernel][link-14], but it's well worth reading. One of the key takeaways, for me, was this phrase: "Scheduling, as in dividing CPU cycles among threads was thought to be a solved problem. We show that this is not the case."
* If you're trying to keep up with this "serverless" stuff that's happening, you can count on Massimo to keep you up to speed. His [write-up from the ServerlessConf][link-15] is very useful.
* Here's a quick post on [optimizing the size of Docker containers][link-16] made from just about any image.
* VMware's Photon OS recently hit 1.0; here's [a blog post][link-17] about the release.
* Consul, a distributed key-value store that sees use in a lot of Docker environments, is now available with its own official Docker image. More details are available in [this blog post][link-19].
* Jerome Petazzo has [an article][link-20] on bind-mounting the control socket inside another container, enabling you to control Docker Engine from within a container.
* [This post][link-23] takes a look at SwarmKit, the code that powers the "swarm mode" available in the Docker 1.12 release. Keep in mind the post is several weeks old as of this post, so things may have changed slightly since that time.
* Anand Patel describes a mechanism for [distributing Docker cache across hosts][link-24] using `docker save` and `docker load` between Docker Engine instances.
* I recently came across [KubeWeekly][link-26], a weekly aggregation of Kubernetes-related news. If Kubernetes is something in your wheelhouse, you may find this site useful.

## Storage

* Have you heard about [the Docker Volume driver for vSphere][link-18]?
* I enjoyed Adam Leventhal's [evaluation of APFS][link-30], the new file system announced at the most recent Apple WWDC.

## Virtualization

* It's not uncommon for folks to use a tool like VirtualBox to run Linux VMs on which they run/test/develop Docker containers. The problem comes when you go to expose the services being provided by those Docker containers to the local network, and it's here that a bit more work is generally needed. [This post][link-5] outlines the process for configuring VirtualBox's NAT rules to expose services provided by Docker containers.
* Folks new to the Linux virtualization space can often get confused by the relationship between QEMU and KVM, as described in [this post by Ronald Bradford][link-6]. Ronald does a great job of showing the various ways to verify whether or not KVM acceleration is being used, but regardless of KVM acceleration QEMU is still in the picture. (The long and short of it: KVM only handles the CPU acceleration piece, QEMU does everything else.)
* Eric Wright brought to my attention [a Vagrant plugin that keeps VirtualBox Guest Additions up-to-date][link-9]. Nifty. Hey, VMware Fusion/Workstation team---want to know why people seem to prefer VB over your technically-superior product? This would be one reason.
* This Microsoft TechNet thread discusses [an issue running Windows 10 under KVM][link-10]; some manual editing of the VM configuration is required to workaround the issue.

## Career/Soft Skills

* Here's Eric Shanks' thoughts on [being a full stack engineer][link-31]. Mayhaps I should get Eric on Full Stack Journey podcast?

I have _so much_ more content that I'd like to include, but it will have to wait until the next Technology Short Take. (This one is already too long!) Until then, enjoy and never stop learning!



[link-1]: http://hicu.be/macvlan-vs-ipvlan
[link-2]: https://sreeninet.wordpress.com/2016/05/29/macvlan-and-ipvlan/
[link-3]: https://sreeninet.wordpress.com/2016/05/29/docker-macvlan-and-ipvlan-network-plugins/
[link-4]: http://p4.org/p4/inband-network-telemetry/
[link-5]: https://blog.ouseful.info/2016/05/22/exposing-services-running-in-a-docker-container-running-in-virtualbox-to-other-computers-on-a-local-network/
[link-6]: http://ronaldbradford.com/blog/are-you-running-kvm-or-qemu-launched-instances-2016-05-19/
[link-7]: https://opennetgeek.wordpress.com/2016/05/09/openswitch-meets-p4/
[link-8]: https://solutionexchange.vmware.com/store/products/vrealize-orchestrator-salt-plugin
[link-9]: http://discoposse.com/2016/05/23/autoupdating-virtualbox-guest-additions-with-vagrant-vbguest/
[link-10]: https://social.technet.microsoft.com/Forums/en-US/695c8997-52cf-4c30-a3f7-f26a40dc703a/failed-install-of-build-10041-in-the-kvm-virtual-machine-system-thread-exception-not-handled?forum=WinPreview2014Setup
[link-11]: http://www.sysdig.org/falco/
[link-12]: https://sysdig.com/blog/sysdig-falco/
[link-13]: https://www.kontena.io
[link-14]: https://blog.acolyer.org/2016/04/26/the-linux-scheduler-a-decade-of-wasted-cores/
[link-15]: http://www.it20.info/2016/06/serverlessconf-2016-new-york-city-a-personal-report/
[link-16]: http://blog.xebia.com/how-to-create-the-smallest-possible-docker-container-of-any-image/
[link-17]: http://blogs.vmware.com/cloudnative/vmwares-photon-os-1-0-now-available/
[link-18]: https://github.com/vmware/docker-volume-vsphere
[link-19]: https://www.hashicorp.com/blog/official-consul-docker-image.html
[link-20]: http://jpetazzo.github.io/2016/04/03/one-container-to-rule-them-all/
[link-21]: https://networklore.com/ansible-ios-upgrade/
[link-22]: http://www.hexatier.com/
[link-23]: https://blog.replicated.com/2016/06/08/first-look-at-swarmkit/
[link-24]: http://blog.runnable.com/post/145362675491/distributing-docker-cache-across-hosts
[link-25]: http://www.routetocloud.com/2014/05/create-firewall-rules-that-blocked-your-own-vc/
[link-26]: https://kubeweekly.com
[link-27]: http://arstechnica.com/information-technology/2016/05/how-the-internet-works-submarine-cables-data-centres-last-mile/1/
[link-28]: https://medium.com/hyperscale-routing/the-end-of-the-router-e4d769aea60f#.6f8w4f9p6
[link-29]: https://avillargarea.wordpress.com/2016/06/17/recovering-access-to-vcenter-after-blocking-it-with-nsx-dfw/
[link-30]: http://arstechnica.com/apple/2016/06/a-zfs-developers-analysis-of-the-good-and-bad-in-apples-new-apfs-file-system/1/
[link-31]: http://theithollow.com/2016/05/31/wanna-full-stack-engineer/
[link-32]: http://blog.clusterlabs.org/blog/2016/next-openstack-ha-arch
[link-33]: http://blog.ipspace.net/2016/06/is-ovsdb-control-or-management-plane.html
