---
author: slowe
categories: Information
comments: true
date: 2016-02-12T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- CoreOS
- Docker
- Automation
- Cisco
- UCS
- OpenStack
- Photon
- macOS
- VMware
- vSphere
title: 'Technology Short Take #61'
url: /2016/02/12/technology-short-take-61/
---

Welcome to Technology Short Take #61. I've tried to find a nice range of articles and links, so here's hoping I was successful and that you find something useful here. Enjoy!

## Networking

* Along with the release of rkt 1.0 (see below in the "Operating Systems/Applications" section), Metaswitch has [announced][link-4] a Container Networking Interface (CNI) plugin for Project Calico. (This appears to be primarily through the [Calico plugin for Kubernetes][link-5] and Kubernetes' support for CNI.)
* Matt Oswalt (my guest on [Episode 2 of the Full Stack Journey Podcast][xref-1]) has been on a tear recently with some really great articles (in my opinion). First up, Matt goes on [a bit of a rant regarding SDN and network automation][link-11]. His key point is that SDN and network automation are different things, and the latter is far more approachable and offers a great deal of benefits to organizations of _any_ size. In his post on [the unspoken benefits of open networking][link-12], Matt presents a similar argument: that the rise of open networking is something that offers benefits to all organizations, not just the web-scale companies. (Both these articles came out of [Network Field Day 11][link-13], by the way.)
* [Via Ivan Pepelnjak][link-15], I found this article on [the need for user-space network ring buffers][link-14]. Don't know what those are? No worries---go read the article. It's very well-written, and explains what user-space network ring buffers are _and_ why the author believes they are essential for networking with a modern OS. (The article also goes on a bit of a rant against OpenBSD, but that's a discussion for a different day.)
* Only a year late, I found [this article][link-18] about Cisco IOS XRv (as in "virtual"). This could be handy for testing and/or experimenting with various network automation techniques or tools.

## Servers/Hardware

* Colin Lynch gives [a "first look" at the Cisco UCS Generation 3 hardware][link-28], which may be of interest if you're a UCS customer or looking at purchasing UCS. In my opinion, it's notable that the Gen3 fabric interconnects lack ACI integration, which (according to Colin) isn't expected until Gen4 interconnects are released.

## Security

* I've been watching Keybase, but [this][link-17] really makes me want to get selected to participate.
* Brian Krebs takes on security in the IoT world in this post [about smart devices with dumb defaults][link-27].

## Cloud Computing/Cloud Management

* I recently came across [an article on the Azure Docker Extension][link-1]. This is a Azure-specific tool that enables you to do things like install Docker Engine, deploy containers, configure TLS certificates, etc., on Azure VMs. This enables you to handle many Docker-related tasks using only Azure tools, which may be preferable for some customers.
* Steve Flanders wrote up a post [announcing the release of Log Insight 3.3][link-24], also announcing that vCenter Server users are also entitled to access to a limited-license version of Log Insight as well.
* Is OpenStack turning into a niche product? That's the question Ken Hui asks in his discussion about [OpenStack being caught between a rock and a hard place][link-25]. Ken's points are absolutely valid and spot-on---I definitely recommend reading this post.
* Look at [this][link-26]. Need I say anything further?

## Operating Systems/Applications

* CoreOS recently announced the 1.0 release of `rkt`, its alternative container runtime to Docker Engine. I hope to be able to spend a bit of time with it soon, but in the meantime you can read [the announcement blog post][link-2]. CoreOS also has [a "getting started" blog post][link-3] that may help.
* Juan Manuel Rey has been spending some quality time with VMware's Photon OS; in particular, how to configure networking on Photon OS. (Given that Photon OS uses systemd, it's more like he's spending time with systemd, but now we're just splitting hairs.) In any case, he's got a couple blog posts you may find useful: one on [network configuration with Photon OS][link-6], and one on [troubleshooting systemd-networkd on Photon OS][link-7]. Keep 'em coming, Juan!
* This one is specific to OS X, but given that OS X has a lot of users in the IT community I felt it was still appropriate. Fournova Software, makers of the OS X-specific Git client [Tower][link-10], have an article listing [5 tips to be more productive with Dash][link-8]. [Dash][link-9], if you weren't aware, is an OS X app designed to provide easy access to various documentation sets. (It's a really handy app, by the way---I use it constantly.)
* The past couple of weeks saw the release of Docker 1.10, Docker Compose 1.6, and Docker Machine 0.6.0. The Docker blog has a couple of posts---one [on Docker 1.10's new features][link-19], and another [on Compose 1.6][link-20]---that might help bring you up to speed on the changes in the latest releases.
* This article on The New Stack about [containers in the telecommunications space][link-29] is a reminder that containers aren't the solution (by themselves) for all problems. While containers solve some problems very elegantly, they don't necessarily solve other problems quite as well, and this article dives into one such area (VoIP architectures).

## Storage

* If you're a newcomer to OS X (or other UNIX-like operating systems) and NFS, you may find this article by Stephen Foskett on [using NFS to share data between UNIX and Mac OS X][link-16] to be helpful. As usual, Stephen clearly lays out the foundational concepts that are involved and explains how to make a setup like this work.
* VMware [recently announced VSAN 6.2][link-21], and along with the new release is plenty of coverage from the VMware community. I'd definitely recommend reading Cormac Hogan's [coverage of the new release][link-22], but also take some time to check out Duncan Epping's [discussion on VSAN 6.2][link-23]. One thing that really strikes me is that many of the "coolest" features of VSAN 6.2 are _only_ available in all-flash configurations---this just underscores the growing importance/role of flash in modern data centers.
* Chris Evans [reflects][link-36] on how Amazon S3 appears to have become the _de facto_ object storage API.

## Virtualization

* Frank Denneman shares some [insights into CPU and memory configurations of ESXi hosts][link-30], based on data gathered by PernixData Cloud. In some respects, the results look (mostly) similar to the data I used to see when I was doing vSphere installations---dual-socket servers seem to still be the sweet spot for many environments, though the number of cores per socket is higher now than in years past, as is the amount of RAM.
* Frank has also launched [the vSphere Design Pocketbook v3][link-31], which (in his words) is "a platform for all the virtualization community members to broadcast their knowledge."
* Rick Scherer has a post up on [how to change the ESXi hostname on an EVO:RAIL appliance][link-32].
* Have you heard about [the new edition of the PowerCLI book][link-33]? I've long thought that this book didn't receive the attention it merits; I'm hoping this new edition is a smash success. It deserves it.
* Granted, this use case is probably a bit rare, but in the event you need it: William Lam has a post on [how to limit the number of physical CPU sockets in an ESXi host][link-34]. William, as usual, has been churning out an _insane_ number of very useful blog posts; be sure to check [his site][link-35] for the latest.
* [Whether or not VMware should open source ESXi][link-37] is a question that has been tossed around a lot over the years, and the climate/economy/situation is different now than it was in previous years. Is now the time? Is it the right move? I honestly don't know.

## Career/Soft Skills

Nothing here this time around, but rest assured I'll be watching for content to include next time.

Despite the fact that I'm trying to publish these Short Takes more frequently, I'm finding it difficult to keep the length manageable. So, before this thing gets any longer, let's wrap it up for this time around. Stay tuned for the next Technology Short Take very soon!


[link-1]: https://ahmetalpbalkan.com/blog/azure-docker-extension/
[link-2]: https://coreos.com/blog/rkt-hits-1.0.html
[link-3]: https://coreos.com/blog/getting-started-with-rkt-1.0.html
[link-4]: http://www.projectcalico.org/a-rocket-reaches-orbit/
[link-5]: http://www.projectcalico.org/announcing-1-0-calico-cni-integration-for-kubernetes/
[link-6]: http://blog.jreypo.io/cloud-native/devops/vmware/sysadmin/linux/network-configuration-in-photon-os/
[link-7]: http://blog.jreypo.io/cloud-native/devops/vmware/sysadmin/linux/quick-tip-troubleshooting-networkd/
[link-8]: https://www.git-tower.com/blog/tips-for-dash/
[link-9]: https://kapeli.com/dash
[link-10]: https://www.git-tower.com/
[link-11]: https://keepingitclassless.net/2016/01/sdn-network-automation-splitting-hairs/
[link-12]: https://keepingitclassless.net/2016/01/unspoken-benefits-open-networking/
[link-13]: http://techfieldday.com/event/nfd11/
[link-14]: http://blog.erratasec.com/2016/01/net-ring-buffers-are-essential-to-os.html
[link-15]: http://blog.ipspace.net/2016/01/quick-link-user-space-network-io-on-x86.html
[link-16]: http://blog.fosketts.net/2015/03/20/using-nfs-to-share-data-between-unix-and-mac-os-x/
[link-17]: https://keybase.io/introducing-the-keybase-filesystem
[link-18]: http://www.fryguy.net/2014/02/08/cisco-ios-xrv-v-as-in-virtual/
[link-19]: http://blog.docker.com/2016/02/docker-1-10/
[link-20]: https://blog.docker.com/2016/02/compose-1-6/
[link-21]: http://www.vmware.com/company/news/releases/vmw-newsfeed/VMware-Introduces-Next-Generation-Hyper-Converged-Software-Enabling-Simple,-High-Performance-Infrastructure-for-the-Software-Defined-Data-Center/2029641
[link-22]: http://cormachogan.com/2016/02/10/vsan-6-2-an-overview-of-the-new-virtual-san-6-2-features/
[link-23]: http://www.yellow-bricks.com/2016/02/10/whats-new-for-virtual-san-6-2/
[link-24]: http://blogs.vmware.com/management/2016/02/whats-new-log-insight-3-3.html
[link-25]: http://cloudarchitectmusings.com/2016/02/08/between-a-rock-and-a-hard-place-will-openstack-become-niche/
[link-26]: https://www.stickermule.com/marketplace/3442-there-is-no-cloud
[link-27]: http://krebsonsecurity.com/2016/02/iot-reality-smart-devices-dumb-defaults/
[link-28]: http://ucsguru.com/2016/02/02/cisco-ucs-generation-3-first-look/
[link-29]: http://thenewstack.io/web-scale-isnt-enough-containers-telecommunications-space/
[link-30]: http://frankdenneman.nl/2015/12/23/insights-into-cpu-and-memory-configuration-of-esxi-hosts/
[link-31]: http://frankdenneman.nl/2016/01/06/new-book-project-vsphere-design-pocketbook-v3/
[link-32]: http://vmwaretips.com/wp/2016/01/13/changing-esxi-hostname-on-an-evorail/
[link-33]: http://blogs.vmware.com/PowerCLI/2016/02/updated-book-powercli-book-2nd-edition.html
[link-34]: http://www.virtuallyghetto.com/2016/02/how-to-limit-the-number-physical-cpu-sockets-in-esxi.html
[link-35]: http://www.virtuallyghetto.com/
[link-36]: https://blog.architecting.it/2016/01/12/has-s3-become-the-de-facto-api-standard/
[link-37]: https://blog.architecting.it/2016/02/02/is-it-time-for-vmware-to-open-source-the-esx-hypervisor/

[xref-1]: {{< relref "2016-02-03-full-stack-journey-ep002.md" >}}
