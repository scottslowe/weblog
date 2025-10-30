---
author: slowe
categories: Information
comments: true
date: 2024-06-28T09:00:00-06:00
tags:
- AWS
- Cilium
- Docker
- EKS
- ESXi
- EVPN
- GitHub
- Hardware
- Kubernetes
- Networking
- Nginx
- Security
- Storage
- Tetragon
- Virtualization
- VMware
- VXLAN
title: 'Technology Short Take 179'
url: /2024/06/28/technology-short-take-179/
---

Welcome to Technology Short Take #179! I'm back with another set of links to articles on various data center- and IT-related topics. In the interest of full transparency, I'd like to give credit to Russ White for his "Weekend Reads" series of posts, which are similar in nature to my Technology Short Takes. If you aren't reading Russ' "Weekend Reads" posts, you're missing out on a good source of useful information. Several of the links included below are taken from recent posts by Russ. Thanks, Russ---and to all the other content creators and content curators referenced here---for your great work! Now, on to the content.<!--more-->

## Networking

* [This post about `netlab`][link-6] just reminds me that I _really_ should spend some quality time with it. Need more than 24 hours in a day...
* Timothy Ham created a GitHub Gist-based [short IPv6 guide for home IPv4 admins][link-8].
* Daniel Dib has a six-part series (so far) on Cisco vPC in a VXLAN/EVPN network. I'll only link to [part 1 of the series][link-15]; you can find links to the rest in the sidebar on his site. I haven't yet read all of them, but they're on my list to read.
* Hat tip to Ivan Pepelnjak, who shared a link to Roman Pomazanov's [article on various network lab tools][link-16].
* On the CNCF blog, William Morgan of Buoyant puts pen to page to discuss [concerns with Kubernetes' Topology Aware Routing (TAR) functionality][link-17].

## Security

* Dan Petro explains why you should [never use pixelation to redact sensitive information][link-2].
* Jimi Sebree [examines Linguistic Lumberjack][link-3], a critical memory corruption vulnerability in Fluent Bit.
* Devidas Jadhav has collected some [getting started tips for Tetragon][link-19].

## Cloud Computing/Cloud Management

* Marcus Noble provides some [recommended Kubernetes resources for newbies][link-7].
* I was doing some testing recently with `kind` and couldn't figure out why my Cilium pods were crashing and restarting continuously. I was working with Cilium's `kube-proxy` replacement functionality, and it turns out I forgot to tell Cilium how to reach the Kubernetes API (in other words, I hadn't added `k8sServiceHost` and `k8sServicePort` to my Helm values). Doh! A quick word of warning if you plan to use/test/try a similar configuration: many articles on this topic (such as ["Kind cluster with Cilium and no kube-proxy"][link-10] or ["Play with Cilium native routing in Kind cluster"][link-12]) will specify to use "kind-control-plane" as the value for `k8sServiceHost` in the Helm values for Cilium. That's true _only_ if your `kind` cluster is named "kind". In reality, the correct value is `<kind-cluster-name>-control-plane`. So, if your `kind` cluster is named "bob", then the correct value is "bob-control-plane". A minor detail, but an important one nevertheless.
* Here's a good post from my Isovalent (now part of Cisco) colleague Amit Gupta on [installing Cilium in EKS with no `kube-proxy`][link-18].

## Operating Systems/Applications

* A colleague pointed [this out][link-5], looks like it might be useful (Linux users only, sorry---if you know of a Windows or macOS equivalent, let me know!).
* There's been a fair amount of chatter regarding blocking AI bots (that are scraping sites for content to be consumed by/used in training large language models). Along those lines, I came across this tutorial by Robb Knight on [blocking bots using Nginx][link-9]. It would be useful to find a similar guide for S3/CloudFront.
* Nick Janetakis has a handy tip on [using nested variable interpolation with Docker Compose v2][link-13].

## Virtualization

* William Lam has already updated his Nested ESXi Virtual Appliance and USB Network Native Driver for vSphere 8.0U3, get more details in [his blog post][link-11].

## Career/Soft Skills

* [Calling people back to the office leads them to find new jobs?][link-1] Nah, can't be.
* I know that generative AI tools like LLMs can be useful, but articles like [this one on how ChatGPT doesn't summarize text but instead merely shortens it][link-4] are why I am very cautious about my use of such tools.
* Via Ivan Pepelnjak, I read Tom Limoncelli's post on [making two trips][link-14]. I won't spoil it here; go read it. It's worth your time, in my opinion.

That's all I have for you today! I hope you found something useful in this post. If you have any feedback for me---I _always_ love to hear from readers---I welcome to you to reach out. You can find me [on Twitter][link-99], on [the Fediverse][link-30], and in a variety of Slack channels/communities. Even my e-mail address isn't too hard to find, if you'd prefer that route! Thanks for reading, and have a great weekend!

[link-1]: https://www.theregister.com/2024/05/16/hr_say_biz_leaders_scared_rto
[link-2]: https://bishopfox.com/blog/unredacter-tool-never-pixelation
[link-3]: https://www.tenable.com/blog/linguistic-lumberjack-attacking-cloud-services-via-logging-endpoints-fluent-bit-cve-2024-4323
[link-4]: https://ea.rna.nl/2024/05/27/when-chatgpt-summarises-it-actually-does-nothing-of-the-kind/
[link-5]: https://slgobinath.github.io/SafeEyes/
[link-6]: https://blog.ipspace.net/2024/06/netlab-1-8-4-vrnetlab-cat8000.html
[link-7]: https://marcusnoble.co.uk/2024-06-24-my-recommended-kubernetes-resources-for-newbies/
[link-8]: https://gist.github.com/timothyham/dd003dbad5614b425a8325ec820fd785
[link-9]: https://rknight.me/blog/blocking-bots-with-nginx/
[link-10]: https://medium.com/@charled.breteche/kind-cluster-with-cilium-and-no-kube-proxy-c6f4d84b5a9d
[link-11]: https://williamlam.com/2024/06/nested-esxi-virtual-appliance-usb-network-native-driver-for-esxi-for-vsphere-8-0-update-3.html
[link-12]: https://medium.com/@nahelou.j/play-with-cilium-native-routing-in-kind-cluster-5a9e586a81ca
[link-13]: https://nickjanetakis.com/blog/docker-tip-98-nested-variable-interpolation-with-docker-compose-v2
[link-14]: https://queue.acm.org/detail.cfm?id=3664275
[link-15]: https://lostintransit.se/2024/04/29/cisco-vpc-in-vxlan-evpn-network-part-1-anycast-vtep/
[link-16]: https://www.linkedin.com/pulse/lets-iac-some-network-labs-roman-pomazanov-tu1pe/
[link-17]: https://www.cncf.io/blog/2024/06/17/the-trouble-with-topology-aware-routing-sacrificing-reliability-in-the-name-of-cost-savings/
[link-18]: https://medium.com/@amitmavgupta/cilium-installing-cilium-in-eks-with-no-kube-proxy-86f54a56c360
[link-19]: https://www.linkedin.com/pulse/magical-ability-peek-inside-running-kubernetes-cluster-devidas-jadhav-t4eyf/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
