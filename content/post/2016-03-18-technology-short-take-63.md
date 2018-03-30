---
author: slowe
categories: Information
comments: true
date: 2016-03-18T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- OpenStack
- OVS
- Microsoft
- NSX
- Docker
- VMware
- vSphere
title: 'Technology Short Take #63'
url: /2016/03/18/technology-short-take-63/
---

Welcome to Technology Short Take #63. I've managed to (mostly) get back to my Friday publishing schedule, though I'm running much later in the day this time around than usual. I'll try to correct that for the next one. In any case, here's another collection of links and articles from around the Net on the major data center technology areas. Have fun reading!

## Networking

* At DevOps Networking Forum 2016, I had the opportunity to share a presentation on some Linux networking options. If you'd like to see the presentation, it's available on [Slideshare][link-3] and [Speakerdeck][link-4]. If you'd like to re-create the demo environment, check out [the presentation's GitHub repository][link-5]. I'm also thinking of creating a video version of the presentation with some expanded content; I'd love to hear from readers if they would find that useful.
* Here's another topic that came up at the recent DevOps Networking Forum: Spotify's SDN Internet Router (SIR). Here's a two-part series ([Part 1][link-6] and [Part 2][link-7]) that discusses the SIR, the motivations for building it, the challenges they faced in building SIR, and the solutions to those challenges. It's a pretty interesting read, in my opinion.
* I recently came across a couple useful troubleshooting guides, [one for Open vSwitch (OVS) and OpenStack Neutron][link-8] and [one for VMware NSX][link-9]. Check these out if you're using any of these technologies.
* Microsoft seems to be all about shocking the world these days (see my related mention of their SQL Server announcement below). At the recent OCP Summit, Microsoft introduced Software for Open Networking in the Cloud (SONiC). Check out [Mark Russinovich's blog post on SONiC][link-18], then pop over to [Kamala Subramaniam's more in-depth SONiC post][link-19].
* Here's some information on [Nuage Networks' experimental Docker Network plugin][link-22]. (Why certain other companies that shall remain unnamed haven't done something similar is beyond me.)
* Matt Oswalt recently unveiled (and open sourced) a framework called ToDD, which stands for "Testing on Demand: Distributed". Read more about it [here][link-23].

## Servers/Hardware

Sorry, I don't have anything for you this time.

## Security

* Russell Pope at Kovarus recently wrote about [using security groups to manage the VMware NSX distributed firewall][link-20]. In talking with customers, I find that one of the things that really challenges their thinking is how to best utilize security groups to their maximum effect. Some of Anthony Burke's work here may also be useful; for example, check out his recent article on [using Log Insight to assist with achieving microsegmentation][link-21].

## Cloud Computing/Cloud Management

* Ravello Systems (now part of Oracle Cloud following their acquisition by Oracle) has a REST API that allows you to programmatically interact with their service. Jonatas Baldwin has [a good article on the Ravello REST API][link-10] that is worth reviewing if this interests you.
* Marcos Hernandez (a colleague on the NSX team at VMware) recently pointed out [this Heat template][link-14] for a multi-tier application with dynamic routing (via Quagga). Until such time as Neutron adds native dynamic routing support, this may be an option for some OpenStack deployments to consider.

## Operating Systems/Applications

* I knew Microsoft was cozying up to Linux, but I honestly didn't expect they would [port SQL Server to Linux][link-2]. Kudos to Microsoft and Microsoft's leadership for quite the culture change.
* There was some hubbub in the container world recently when Docker talked about Docker Swarm was faster than Kubernetes for scheduling containers at scale. Personally, I think it's an apples-to-oranges comparison, since Kubernetes is more than just a scheduler (think about replication controllers and services and such), but to each his own. [Here's the Docker blog post][link-15] talking about the performance study.
* Speaking of schedulers...HashiCorp is touting the performance of the latest release of [Nomad][link-17] for scheduling workloads in [this post about scheduling 1 million containers][link-16]. I haven't had the chance to really dig into Nomad yet, but a cursory glance at the project's home page tells me it might be worth my while. Now, I just need to find more time in the day...
* This is a much older article on [resource management in Docker][link-24], but still (as far as I can tell) useful and informative. You may also find this (related) article on [memory inside Linux containers][link-25] to be helpful.
* Sometimes (more often than not, I would say) terminology is important. That's why it's important, in my humble opinion, to be clear when talking about containers (in general) or Docker (a specific implementation). And, in the case of Docker, whether you're talking about the open source Docker project or the company Docker, Inc. Hence, [this post][link-26] by Massimo Re Ferre' might be helpful if you're unclear about any of these distinctions.

## Storage

* VMware recently GA'd version 6.2 of VSAN (alongside vSphere 6.0 U2). Naturally, there's been folks who have been blogging about some of the new features. Two articles in particular stood out to me: one by Christos Karamanolis on [erasure coding with VSAN 6.2][link-11], and one by my good friend Jase McCarty on [the use of sparse virtual swap files][link-12]. Both are good articles (for different reasons), and if you're thinking about deploying VSAN I'd recommend reading both. Finally, you might find the VMware [Virtual SAN 6.2 Design and Sizing Guide][link-27] helpful (thanks to [Eric Sloof for the link][link-28]).

## Virtualization

* William Lam recently blogged about [the evolution of nested ESXi][link-1] over the last few years, and gave a sneak peek at the potential evolution moving forward.
* Iwan Rahabok has a couple of posts (these are slightly older) that discuss sample architectures for a VMware SDDC deployment. The first article provides [an overview of an architecture for 500 to 2000 VMs][link-29]; the second article dives a bit deeper into [the network architecture][link-30].

## Career/Soft Skills

* Many people take the use of a home lab as a given, but Max Mortillaro contends that the choice to use a home lab should be approach as rationally as any other technical or career decision. In [this article][link-13], he lays out some questions to ask yourself as you evaluate whether a home lab is the right (best) approach for you and your career.

That's all, folks! In the interest of not taking up _too_ much of your time, I'll wrap up here. As always, I hope you found something useful here. Thanks for reading!



[link-1]: http://www.virtuallyghetto.com/2016/03/vsphere-6-0-update-2-hints-at-nested-esxi-support-for-paravirtual-scsi-pvscsi-in-the-future.html
[link-2]: https://blogs.microsoft.com/blog/2016/03/07/announcing-sql-server-on-linux/
[link-3]: http://www.slideshare.net/lowescott/an-overview-of-linux-networking-options
[link-4]: https://speakerdeck.com/slowe/an-overview-of-linux-networking-options
[link-5]: https://github.com/scottslowe/2016-dnf-materials
[link-6]: https://labs.spotify.com/2016/01/26/sdn-internet-router-part-1/
[link-7]: https://labs.spotify.com/2016/01/27/sdn-internet-router-part-2/
[link-8]: http://www.yet.org/2014/09/openvswitch-troubleshooting/
[link-9]: http://www.yet.org/2014/09/nsxv-troubleshooting/
[link-10]: http://deployeveryday.com/2016/01/25/ravello-systems-rest-api.html
[link-11]: https://blogs.vmware.com/virtualblocks/2016/02/12/the-use-of-erasure-coding-in-virtual-san-6-2/
[link-12]: http://www.jasemccarty.com/blog/vsan62-powercli-sparse-vswp/
[link-13]: http://www.kamshin.com/2016/03/a-rational-approach-to-home-labs/
[link-14]: https://github.com/nsxmarcos/vio/blob/master/heat/poc/validation_tests/3-tier_central-routing_nonat_single_quagga.yaml
[link-15]: https://blog.docker.com/2016/03/swarmweek-docker-swarm-exceeds-kubernetes-scale/
[link-16]: https://www.hashicorp.com/c1m.html
[link-17]: https://www.nomadproject.io/
[link-18]: https://azure.microsoft.com/en-us/blog/ocp-2016-building-on-community-driven-innovation/
[link-19]: https://azure.microsoft.com/en-us/blog/microsoft-showcases-%E2%80%9Csoftware-for-open-networking-in-the-cloud-sonic-%E2%80%9D/
[link-20]: http://www.kovarus.com/managing-the-nsx-distributed-firewall-via-security-groups/
[link-21]: http://networkinferno.net/achieving-micro-segmentation-with-log-insight
[link-22]: http://www.nuagenetworks.net/blog/github-docker-plugin
[link-23]: https://keepingitclassless.net/2016/03/test-driven-network-automation/
[link-24]: https://goldmann.pl/blog/2014/09/11/resource-management-in-docker/
[link-25]: http://fabiokung.com/2014/03/13/memory-inside-linux-containers/
[link-26]: http://it20.info/2016/01/why-docker-containers-and-docker-oss-docker-inc/
[link-27]: https://www.vmware.com/files/pdf/products/vsan/virtual-san-6.2-design-and-sizing-guide.pdf
[link-28]: http://www.ntpro.nl/blog/archives/3065-VMware-Virtual-SAN-6.2-Design-and-Sizing-Guide.html
[link-29]: http://virtual-red-dot.info/vmware-sddc-architecture-sample-for-500-and-2000-vm/
[link-30]: http://virtual-red-dot.info/vmware-sddc-architecture-sample-for-500-and-2000-vm-part-2/
