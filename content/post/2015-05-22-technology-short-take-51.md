---
author: slowe
categories: Information
comments: true
date: 2015-05-22T12:15:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OVN
- OpenStack
- VMware
- Automation
- Docker
- Linux
- Ubuntu
- Vagrant
- HyperV
title: 'Technology Short Take #51'
url: /2015/05/22/technology-short-take-51/
---

Welcome to Technology Short Take #51, another collection of posts and links about key data center technologies like networking, virtualization, cloud management, and applications/operating systems. Here's hoping you find something useful in this collection!

## Networking

* I'm not sure if this falls here or into the "Cloud Computing/Cloud Computing" category, but Shannon McFarland---fellow co-conspirator with the Denver OpenStack Meetup group---has a nice article describing some [design and deployment considerations for IPv6 in the OpenStack Kilo release][link-5].
* I'm pretty sure I've mentioned Open Virtual Network (OVN) here before, as I'm pretty jazzed about the work going on with this project. If you're unfamiliar with OVN, Gal Sagie has a couple of articles that might help. I'd start with the later of the two articles, which provides [an introduction to OVN][link-1], before moving on to Gal's discussion of [OVN and the distributed controller][link-2] and his article on [OVN and containers][link-3].
* Speaking of OVN, Russell Bryant has a detailed description of [using OVN with OpenStack Neutron][link-29] (via DevStack).
* Using Jinja2 templates for automating network device configuration is a topic that's getting a fair amount of attention (there were at least two sessions discussing this technique while I was at Interop). Rick Sherman has an article on [using Jinja2 templates for network automation][link-6], including a practical example.
* If you'd like to manually wire up a quick GRE tunnel-based network across multiple Docker hosts, here's a walk-through on [a multi-host Docker network][link-15].
* VXLAN overlay networks between AWS and GCE? Sure! [Here's one way][link-16], using Ravello Systems.
* Flavio Leitner has a nice article [comparing OVS internal ports with Linux veth devices][link-18]. This topic is the subject of some debate given that some claim OVS internal ports perform much better than Linux veth devices. Flavio's research shows the performance differential appears to be negligible overall.
* MAC learning on OVS under OpenStack? [Here you go][link-21], thanks to Anthony Burke.
* Dave Tucker---formerly of Socketplane, now of Docker---has [a post outlining the direction of Docker networking][link-22]. This is a good read for those of you who are interested in understanding where networking with Docker containers is headed.

## Servers/Hardware

* I suppose this could go under "Networking", but it's really focused on hardware---specifically, the [NoviSwitch line of OpenFlow-optimized switches][link-31] from NoviFlow.

## Security

* There's a lot of talk about container security, but now there's something you can actually _do_ about it. Along with Docker, VMware worked with the Center for Internet Security (CIS) to prepare a security benchmark for Docker 1.6 (available [here][link-13]), and has updated VMware vRealize Configuration Manager to support assessing containerized environments against this benchmark. More details, along with screenshots and explanations of some of the CIS benchmark settings, are found in [this VMware blog post][link-14].
* Cody Bunch (re-)posted [a hardening script for Ubuntu][link-27] that is intended to be supplied as userdata (of some sort) when deploying to a cloud platform. Also from Cody is [this post][link-28] talking about using the CIS Ansible role to apply the CIS benchmark against CentOS/RHEL systems.

## Cloud Computing/Cloud Management

* The topic of HA (high availability) in cloud computing environments, particularly OpenStack environments, is a topic with many different aspects. Russell Bryant explores some of [the different facts of OpenStack HA][link-8] in a comprehensive and (in my opinion) well-written article.
* Interesting in building your own NSX lab, but don't have the equipment in your own home lab? Here's an article on how one person used Ravello Systems' new functionality supporting nested ESXi to [build an NSX lab on AWS][link-11].
* Juan Manuel Rey walks you through [applying a patch to your VMware Integrated OpenStack (VIO) environment][link-12]. Pretty painless, to be honest.
* Darryl Cauldwell has a write-up on [using VMware Log Insight with VMware Integrated OpenStack][link-24].

## Operating Systems/Applications

* In late March there was a "Docker in Production" meetup, and a summary post from that meetup---with links to videos of the presentations---is available [via the Heavybit blog][link-7]. Companies sharing their Docker production stories included Iron.io, ClusterHQ, RelateIQ, and Docker Inc.
* A new release of Docker Compose---version 1.2---happened in mid-April, with some new features. The new `extends` feature is perhaps the most notable one, which allows for sharing configurations between services and environments. Have a look at [this blog post][link-9] for all the details.
* Frank Hinek has a basic---but very thorough and very useful---post on [maintaining DNS records in BIND][link-10]. It's a great resource for both BIND newbies as well as those who don't muck around enough in BIND to have this stuff memorized (like me).
* Here's a guide to [deploying a DNS server using Docker][link-17].
* I keep waffling between Ansible and SaltStack as my "next-generation" configuration management solution. In the event you're in the same boat, here's something to tip the scales in the SaltStack direction: [a Vagrant environment to build a SaltStack multi-minion setup][link-20].
* Eric Gray has a nice write-up on [using Lightwave for authentication with Photon][link-23].
* Google shook up the container world recently when it announced integration between Kubernetes and AppC. Some took that as a knock against Docker; Google was quick to respond, via [this blog post][link-25], to say that Docker is still important to Google and to containers in general.
* This is pretty cool: a _cross-platform_ (ARM and x86_64) hybrid cloud built on top of Docker. Get all the details [here][link-26].
* Cool---[a Docker plugin for vRealize Orchestrator][link-30].

## Storage

Nothing this time around, but I'll keep my eyes peeled for content to include in future posts.

## Virtualization

* Based on [this report][link-4], it looks like the next revision of Microsoft Hyper-V will support nested virtualization. VMware fans have been able to take advantage of nested virtualization for a while, and with the addition of this functionality now Microsoft fans will be able to do the same (hat tip to William Lam).
* This list of [VMware and Vagrant performance hacks][link-19] is quite handy. Thanks Cody!

I still have so much more to share with you, but I'd better wrap this up now before it gets any longer. Otherwise, I'll have to start calling these posts "Technology Long Takes"!



[link-1]: http://galsagie.github.io/sdn/openstack/ovs/2015/04/20/ovn-1/
[link-2]: http://galsagie.github.io/ovs/virtualization/2015/01/16/ovn-distributed-controller/
[link-3]: http://galsagie.github.io/sdn/openstack/ovs/2015/04/26/ovn-containers/
[link-4]: http://www.thomasmaurer.ch/2015/05/hyper-v-vnext-is-going-to-support-nested-virtualization/
[link-5]: http://www.debug-all.com/?p=52
[link-6]: http://vulgarpython.com/jinja2-templates-for-network-automation/
[link-7]: http://blog.heavybit.com/blog/2015/3/23/dockermeetup
[link-8]: http://blog.russellbryant.net/2015/03/10/the-different-facets-of-openstack-ha/
[link-9]: http://blog.docker.com/2015/04/easily-configure-apps-for-multiple-environments-with-compose-1-2-and-much-more/
[link-10]: http://frankhinek.com/maintaining-bind-dns-records/
[link-11]: http://www.ravellosystems.com/blog/vmware-nsx-lab-on-aws/
[link-12]: https://jreypo.wordpress.com/2015/04/29/how-to-patch-your-vio-environment/
[link-13]: http://benchmarks.cisecurity.org/downloads/show-single/index.cfm?file=docker16.100
[link-14]: http://blogs.vmware.com/security/2015/05/vmware-releases-security-compliance-solution-docker-containers.html
[link-15]: http://wiredcraft.com/blog/multi-host-docker-network/
[link-16]: http://blog.mwpreston.net/2015/04/27/vxlan-on-ravello-between-google-and-amazon-ec2/
[link-17]: http://www.damagehead.com/blog/2015/04/28/deploying-a-dns-server-using-docker/
[link-18]: http://flavioleitner.blogspot.com.br/2015/05/open-vswitch-internal-ports-and-linux.html
[link-19]: http://blog.codybunch.com/2015/04/28/VMware--Vagrant-Performance-Hacks/
[link-20]: http://blog.codybunch.com/2015/04/28/SaltStack-and-Multi-Minions-with-Vagrant/
[link-21]: http://networkinferno.net/enable-mac-learning-on-ovs-under-openstack
[link-22]: https://blog.docker.com/2015/04/docker-networking-takes-a-step-in-the-right-direction-2/
[link-23]: http://www.vcritical.com/2015/05/use-lightwave-to-authenticate-ssh-logins-to-photon/
[link-24]: http://darrylcauldwell.com/vrealize-log-insight-for-vmware-integrated-openstack/
[link-25]: http://blog.kubernetes.io/2015/05/docker-and-kubernetes-and-appc.html
[link-26]: http://java.dzone.com/articles/cross-platform-hybrid-cloud
[link-27]: http://blog.codybunch.com/2015/05/12/Update-Userdata-Hardening-Script/
[link-28]: http://blog.codybunch.com/2015/05/15/Using-the-CIS-Ansible-Role-against-CentOSRHEL/
[link-29]: http://blog.russellbryant.net/2015/05/14/an-ez-bake-ovn-for-openstack/
[link-30]: https://github.com/m451/coopto
[link-31]: http://noviflow.com/products/noviswitch/
