---
author: slowe
categories: Information
comments: true
date: 2019-05-03T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Docker
- SSH
- Kubernetes
- Ansible
- Automation
- AWS
- Intel
- HyperV
title: 'Technology Short Take 113'
url: /2019/05/03/technology-short-take-113/
---

Welcome to Technology Short Take #113! I hope the collection of links and articles I've gathered for you contains something useful for you. I think I have a pretty balanced collection this time around; there's a little bit of something for almost everyone. Who says you can't please everyone all the time?<!--more-->

## Networking

* Via [the Kubernetes blog][link-9], Box announced it has open sourced a project called `kube-iptables-tailer`, which turns packet drops from `iptables` into Kubernetes events that can be logged for easier troubleshooting. The GitHub repository for the project is [here][link-10].
* Via BlueCat Networks, John Capobianco shares his network automation journey. In [part 1][link-21], John discusses the frameworks/tooling and the goals for his network automation efforts; in [part 2][link-22], John digs into getting started with Ansible and the initial impact of his efforts.
* Diógenes Rettori has [a comparison of Istio and Linkerd][link-23] as solutions for service mesh. Personally, I could've done without the little product advertisement at the end, but that's just me.
* Here's a good article on [packets-per-second limits in EC2][link-24].

## Servers/Hardware

* AnandTech has [some coverage of the latest Intel processor announcements][link-28], which totally flew by my radar.

## Security

* I recently [read about the Evil-Twin Framework (ETF)][link-1], a tool for doing some wireless penetration testing.
* Dejan Zelic talks in some detail about [the dangers of exposing the `docker.sock` socket][link-5].
* Bogdan Popa educates folks on [using `ProxyCommand` and `ProxyJump` instead of SSH agent forwarding][link-7].
* Keybase [discusses][link-15] some challenges with encrypted chat applications and how their architecture avoids some of those challenges.

## Cloud Computing/Cloud Management

* [This][link-18] looks like a handy tool. It was pointed out to me quite some time ago, but I was lax in getting into a Technology Short Take.
* This [bare metal host management solution for Kubernetes][link-25] looks interesting, but it seems like something that should be part of the community's ClusterAPI efforts.
* This past week Microsoft and VMware announced Azure VMware Solutions, which allows customers to run the VMware software stack on Azure (an arrangement similar in nature to VMware Cloud on AWS, as I understand it). One key difference to keep in mind is that this solution is delivered by Microsoft (not VMware); with VMware Cloud on AWS, the solution is delivered by VMware. Check out [Thomas Maurer's post on the announcement][link-26].
* Henning Jacobs shares [his perspective][link-27] on many Kubernetes clusters vs. fewer Kubernetes clusters.
* This article on [edge triggering vs. level triggering in Kubernetes][link-29] is really good, and well worth reading.

## Operating Systems/Applications

* [This post][link-4] is from 2012, but is---to me---still as applicable today as ever.
* Nicolas Fränkel discussing some optimizations that might be possible by [building dependencies into your Docker base layer][link-6]. I could see this making sense in this case, since the dependencies are pretty stable.
* William Henry takes some time to [explain `podman` and `buildah`][link-8] in terms that are familiar for existing Docker users.
* I've said before that just becuase you _can_ do something doesn't mean you _should_ do it. Nathaniel Schutta takes that approach to breaking your application down into microservices, and provides [six factors to help gauge][link-12] whether a microservices architecture is the right approach.
* Rinu Gour [an article titled "Kafka for Beginners,"][link-13] but the article covers a _lot_ of ground (in my opinion)---perhaps a bit too much for true beginners to Kafka (like myself). Still, it's a useful collection of terminology and Kafka-related links.
* This article on [building DevOps pipelines with open source tools][link-14] was exactly what a beginner (to CI/CD) needed. It's high-level and not very detailed, but it outlines the basic components of a pipeline and provide options for each component. If you're new to this space as well, you may also find this article helpful.
* Lars Kellogg-Stedman has a post on [writing Ansible filter plugins][link-20].

## Storage

* It's [the end of an era][link-17]. This, plus the fact that Howard Marks is working for a vendor...what is the world coming to?
* Jim Handy ("The SSD Guy") has a four-part series (I think only three parts have been published as of this post) on the two different operating modes for Intel's Optane DIMMs (persistent memory). Start with part 1, which is [here][link-19]. Thankfully, Jim has done a good job of linking together the different parts of the series.

## Virtualization

* Here's a great post with more details on concerns over [randomness in virtual machines][link-11].
* Although written more from a security perspective (as in helping folks get started with security research), [this article on Hyper-V][link-16] provide some good foundational information that's useful for just about anyone.

## Career/Soft Skills

* This [slightly older post][link-2] by Jessie Frazelle outlines what she envisions it means to be a "distinguished engineer" (or some other high-level technical individual contributor). There are some great points here that are, in my opinion, well worth considering.
* Alice Goldfuss' article on [how to get into SRE][link-3] is also worth reading, in my opinion, even if you aren't interested in SRE specifically. Why? The guidance she offers is applicable in many cases to _any_ technical specialty you might be interested in pursuing.

[Hit me on Twitter][link-99] with any comments or feedback. Otherwise, enjoy catching up on some technical reading!

[link-1]: https://opensource.com/article/19/1/evil-twin-framework
[link-2]: https://blog.jessfraz.com/post/defining-a-distinguished-engineer/
[link-3]: https://blog.alicegoldfuss.com/how-to-get-into-sre/
[link-4]: https://mikehadlow.blogspot.com/2012/05/configuration-complexity-clock.html
[link-5]: https://dejandayoff.com/the-danger-of-exposing-docker.sock/
[link-6]: https://blog.frankel.ch/layering-docker-images-dependencies/
[link-7]: https://defn.io/2019/04/12/ssh-forwarding/
[link-8]: https://developers.redhat.com/blog/2019/02/21/podman-and-buildah-for-docker-users/
[link-9]: https://kubernetes.io/blog/2019/04/19/introducing-kube-iptables-tailer/
[link-10]: https://github.com/box/kube-iptables-tailer
[link-11]: https://blogs.gentoo.org/marecki/2018/01/23/randomness-in-virtual-machines/
[link-12]: https://content.pivotal.io/blog/should-that-be-a-microservice-keep-these-six-factors-in-mind
[link-13]: https://medium.com/@rinu.gour123/kafka-for-beginners-74ec101bc82d
[link-14]: https://opensource.com/article/19/4/devops-pipeline
[link-15]: https://keybase.io/blog/chat-apps-softer-than-tofu
[link-16]: https://blogs.technet.microsoft.com/srd/2018/12/10/first-steps-in-hyper-v-research/
[link-17]: https://storagemojo.com/2019/04/23/storagemojo-on-hiatus/
[link-18]: https://github.com/jeanlouisferey/aws-securitygroup-grapher
[link-19]: https://thessdguy.com/intels-optane-two-confusing-modes-part-1-overview/
[link-20]: http://blog.oddbit.com/2019/04/25/writing-ansible-filter-plugins/
[link-21]: https://www.bluecatnetworks.com/blog/my-network-automation-journey-part-1-frameworks-and-goals/
[link-22]: https://www.bluecatnetworks.com/blog/my-automation-journey-part-2-building-in-ansible-and-initial-impact/
[link-23]: https://medium.com/@rettori/linkerd-or-istio-6fcd2aad6e42
[link-24]: https://stressgrid.com/blog/pps_limits_in_ec2/
[link-25]: https://github.com/metal3-io/metal3-docs
[link-26]: https://www.thomasmaurer.ch/2019/04/run-your-vmware-natively-on-azure-with-azure-vmware-solutions/
[link-27]: https://srcco.de/posts/many-kubernetes-clusters.html
[link-28]: https://www.anandtech.com/show/14256/intel-9th-gen-core-processors-all-the-desktop-and-mobile-45w-cpus-announced
[link-29]: https://hackernoon.com/level-triggering-and-reconciliation-in-kubernetes-1f17fe30333d
[link-99]: https://twitter.com/scott_lowe
