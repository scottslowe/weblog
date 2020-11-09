---
author: slowe
categories: Information
comments: true
date: 2016-01-29T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Arista
- OSS
- VXLAN
- NSX
- VMware
- Docker
- OpenStack
- CoreOS
- macOS
- FibreChannel
- LXC
- Fusion
title: 'Technology Short Take #60'
url: /2016/01/29/technology-short-take-60/
---

Welcome to Technology Short Take #60. As usual, I've gathered what I hope to be a useful but varied collection of articles and links on key data center technologies. I hope something I've included here will be helpful---enjoy!

## Networking

* The rise of Linux in importance in the networking industry continues, with Arista announcing enhancements to Arista EOS to support---among other things---Docker containers. (See [this SDx Central article][link-2] or [this Network World article][link-3] for more details.)
* As a partial counterpoint to the increased use/visibility of Linux in the networking industry, check out [Ivan's take on Dell's OS10 announcement][link-17].
* And while we are on the topic of Linux and open source in the networking industry...read [this entertaining _and_ informative article][link-18] on the importance of (truly) open source libraries in networking.
* NIC offloads can make a noticeable difference in performance with VXLAN-based overlay networks, so it's worth some time making sure that VXLAN offloads are supported _and_ enabled. Dale Coghlan has an article on [the Broadcom bnx2x driver and VXLAN offload][link-7] that may save you some time in this area.
* This post on [automating NSX security rules using Splunk][link-9] (and some custom code) is pretty cool. Granted, it's a VMware post, so it may gloss over some details, but it's still a decent look at what's possible when you take the abstractions and APIs offered by a solution like NSX.
* Elver Sena (great guy, by the way) has an article [discussing NSX Manager backup and restore][link-29].

## Servers/Hardware

I don't have anything this time around, but I'll stay alert for content to include in future posts.

## Security

* Have you heard about the DoD Security Technical Implementation Guide (STIG) ESXi VIB? Here's [a brief write-up][link-19] by Jason Scanga.

## Cloud Computing/Cloud Management

* Though I haven't had much opportunity to work with Cloud Foundry (yet), I still try (my best) to keep track of what's happening in that space. As such, I recently came across [the announcement of CloudFoundry-Mesos][link-13], an open source project aimed at running Cloud Foundry directly on Mesos (and thus the Mesosphere DCOS). The project itself is open source and [hosted on GitHub][link-12]. Per the `README.md` in the repo, it's still early days yet, but this looks quite interesting.
* Ben Snape has a post [that introduces the idea of a `Terrafile`][link-16], which is a list of all the Terraform modules and versions used by/in a particular Terraform configuration. Although I haven't worked too much with Terraform modules, I can definitely see where Ben is coming from with this idea. Here's hoping that Hashicorp picks up this concept and runs with it.
* Jon Langemak has an article describing [how he built his OpenStack home lab][link-23], starting (in this article) with the networking. Jon was used nested virtualization in his home lab, so seeing how he mapped out the interplay between VLANs and virtual network may be helpful for others who are considering a similar setup.

## Operating Systems/Applications

* CoreOS `rkt` is coming along nicely; version 0.15.0 was recently introduced an includes a new `rkt fly` feature that aims to provide better support for software that needs more privileges than a typical container (the Kubernetes _kubelet_ is one example). [This blog post][link-1] has more details.
* This probably falls more into the "interesting" than "useful" category, but it may prove helpful to someone out there. As you know, the core of Docker Engine is morphing into `runc` as part of the Open Containers Initiative (OCI). Continuing her legacy of doing some unique things with containers on the desktop, Jessie Frazelle recently blogged about [`runc` containers on the desktop][link-8].
* In response to my post on my current productivity setup, the author of this blog post sent me a link to his article on [converting files before moving to Linux][link-14]. Good information, thanks!
* [Run Windows containers on the Mac][link-15]. 'Nuff said.
* With [the recent acquisition of Unikernel Systems by Docker Inc.][link-20], you've probably suddenly got unikernels on your radar. (Bart Smith gave you the heads-up [in Episode 1 of the Full Stack Journey Podcast][xref-1], so you shouldn't have been surprised.) In any case, [here's an article][link-21] that provides a overview of unikernels. After you've finished reading that, pop over to the Joyent site to read Bryan Cantrill's take on [why unikernels are unfit for production][link-22]. A lot of folks criticized Bryan's viewpoint on unikernels, but he's right---there is (right now) a dearth of operational tools for unikernels. At best, that makes them an interesting research/innovation opportunity, but certainly not ready for production.

## Storage

* This is cool---Scott Harney talks about [using Netmiko to automate FC SAN zoning][link-5], and he points to [an article by Matt Oswalt on a similar topic][link-6] that I don't recall seeing. It would be very cool if Scott were to share the source to the Python scripts he talks about in the article. (GitHub would be a good way. Hint, hint.)

## Virtualization

* The Fusion team at VMware recently blogged about [an issue with port forwarding on NAT networks][link-4]; the blog post has a workaround for the issue. (The future of VMware Fusion---and VMware Workstation---as a result of recent VMware layoffs is [a topic I leave others to discuss][link-30].)
* William Lam shares [how to bootstrap the VCSA using the ESXi Embedded Host Client (EHC)][link-10]. William also has [a great "cheat sheet" to the AppCatalyst API][link-24], in case you're experimenting with that product (project?).
* Here's a post on [how to change the geometry of a VMDK][link-25]. (Personally, I'm interested in reading the article that describes the where/why the author had to use this process.)
* According to [this post by Dustin Kirkland][link-28], LXD (which I first discussed in [this introductory post][xref-2]) is part of Ubuntu Server 15.10 by default (and that will continue to be the case with 16.04, the next LTS release). LXC-based Linux containers---which are what LXD helps manage/manipulate/orchestrate---have a different architecture and different value prop than Docker containers, but too often get overlooked.

## Career/Soft Skills/Productivity

* I like this post by Massimo Re Ferre' on [DevOps for Dummies][link-11]. The term "DevOps" has been thrown around by so many different people in so many different contexts that it's now incredibly difficult to understand what is meant when someone says "DevOps". Massimo does a good job (as usual!) of breaking it down and making it consumable. 
* Following up on the IRC vs. mailing list vs. whatever discussion from previous Technology Short Takes, here's an article on [setting up a ZNC bouncer][link-26] (which is a way to improve your reachability on IRC). [This post][link-27] by Sean Dague (upon which the first post I referenced is also based) may also be useful.

That's enough for this time around. See you next time around! Until then, feel free to [contact me on Twitter][link-31] if you have any questions, comments, or thoughts.

[link-1]: https://coreos.com/blog/rkt-0.15.0-introduces-rkt-fly.html
[link-2]: https://www.sdxcentral.com/articles/news/arista-outfits-eos-for-containers-hybrid-clouds/2016/01/
[link-3]: http://www.networkworld.com/article/3023848/cloud-computing/arista-networks-pops-next-gen-os.html
[link-4]: https://blogs.vmware.com/teamfusion/2016/01/workaround-of-nat-port-forwarding-issue-in-fusion-8-1.html
[link-5]: http://www.scottharney.com/using-python-and-netmiko-to-automate-san-zoning/
[link-6]: http://keepingitclassless.net/2014/04/san-config-automation-ansible/
[link-7]: http://www.sneaku.com/2015/05/26/broadcom-bnx2x-driver-and-vxlan-offload/
[link-8]: https://blog.jessfraz.com/post/runc-containers-on-the-desktop/
[link-9]: https://blogs.vmware.com/networkvirtualization/2016/01/vmware-nsx-security-splunk.html
[link-10]: http://www.virtuallyghetto.com/2015/12/how-to-bootstrap-the-vcsa-using-the-esxi-embedded-host-client.html
[link-11]: http://it20.info/2015/12/devops-for-dummies/
[link-12]: https://github.com/mesos/cloudfoundry-mesos
[link-13]: https://mesosphere.com/blog/2015/12/15/cloud-foundry-dcos/
[link-14]: https://blog.iefdev.se/2015/12/converting-files-before-mv2linux/
[link-15]: https://github.com/nomisbeme/flotsam/blob/master/windowsdockeronthemac.md
[link-16]: http://bensnape.com/2016/01/14/terraform-design-patterns-the-terrafile/
[link-17]: http://blog.ipspace.net/2016/01/dell-os10-and-cumulus-linux.html
[link-18]: http://packetpushers.net/industry-needs-open-source-framework-switching-silicon/
[link-19]: http://www.vm5280.com/2016/01/dod-security-technical-implementation.html
[link-20]: https://www.docker.com/press-release-01212016docker-acquires-unikernel-systems-extend-breadth-docker-platfrom
[link-21]: https://ma.ttias.be/what-is-a-unikernel/
[link-22]: https://www.joyent.com/blog/unikernels-are-unfit-for-production
[link-23]: http://www.dasblinkenlichten.com/building-an-openstack-home-lab-the-lab/
[link-24]: http://www.virtuallyghetto.com/2016/01/cheatsheet-for-the-entire-vmware-appcatalyst-api-using-curl.html
[link-25]: https://michaelryom.dk/change-vmdk-geometry/
[link-26]: https://developer.ibm.com/opentech/2016/01/21/openstack-development-tips-setting-up-a-znc-bouncer/
[link-27]: https://dague.net/2014/09/13/my-irc-proxy-setup/
[link-28]: http://blog.dustinkirkland.com/2015/11/lxd-in-sky-with-diamonds.html
[link-29]: http://blog.senasosa.com/2015/12/nsx-manager-backup-and-restore.html
[link-30]: http://planetvm.net/blog/?p=2952
[link-31]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-01-08-full-stack-journey-ep001.md" >}}
[xref-2]: {{< relref "2015-05-06-quick-intro-lxd.md" >}}
