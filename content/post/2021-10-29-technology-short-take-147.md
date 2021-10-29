---
author: slowe
categories: Information
comments: true
date: 2021-10-29T09:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- BGP
- Envoy
- Apple
- macOS
- Kubernetes
- CAPI
- Terraform
- Kong
- Docker
- AWS
- VMware
- vSphere
title: 'Technology Short Take 147'
url: /2021/10/29/technology-short-take-147/
---

Welcome to Technology Short Take #147! The list of articles is a bit shorter than usual this time around, but I've still got a good collection of articles and posts covering topics in networking, hardware (mostly focused on Apple's processors), cloud computing, and virtualization. There's bound to be something in here for most everyone! (At least, I hope so.) Enjoy your weekend reading!<!--more-->

## Networking

* Chris Parker shares [the reason why 65535 is not part of the private autonomous system range][link-4]. It's an interesting history lesson and explanation, even if you aren't a networking nerd.
* Dmytro Shypovalov discusses [ARP problems in EVPN][link-7]. I laughed at his comment regarding people stepping on rakes (read the post).
* Evgeny Khabarov's [part 4 in a series on using Envoy as an API gateway][link-9] talks about authentication and authorization (aka AuthN/AuthZ). In particular, Khabarov focuses on Envoy's `ext_authz` filter, which is what allows Envoy to check with an authorization service to see if a request is permitted or denied.
* I was having a bit of difficulty fully grokking the Original Destination feature in Envoy, and I found [this article][link-16] to be helpful in understanding how it works and is configured. Another very helpful resource on this topic is [this masterpiece][link-17] by Jimmy Song.
* Julia Evans shares [some tools to explore BGP][link-19].

## Servers/Hardware

* David Sparks [shares][link-12] the specs for his new 16" MacBook Pro as well as the rationale for how he arrived at that configuration. I must confess that I am intrigued by the M1 Pro and M1 Max, but having just bought a "regular" M1 a few months ago I'm not sure I can justify the purchase. And, to be honest, my "regular" M1 is plenty snappy for what I do.
* AnandTech [takes a closer look at Apple's A15 SoC][link-18] (system on a chip), which debuted in the iPhone 13.

## Security

* Senthil Raja Chermapandian [asks][link-3] the question, "Are Kubernetes Network Policies really useful?" I see their utility as being less useful now with the rising adoption of service meshes and L7-aware CNI plugins like Cilium, but I do still believe there are valid use cases for them.
* Thomas Reed with Malwarebytes Labs has a lengthy post on [how macOS attacks are evolving][link-8].

## Cloud Computing/Cloud Management

* Now that Cluster API has hit 1.0 (and v1beta1 status, indicating pretty stable APIs), the Cluster API community moves on to addressing how to manage the lifecycle of groups of Kubernetes clusters. See [this blog post][link-1] from Fabrizio Pandini for more details.
* Want to use the Kong API Gateway with Knative? Fear not, the folks from Direktiv share their knowledge on [running services with Knative and Kong][link-2].
* William Lam shares some information on [using `kube-vip`][link-10] with VMware's new Tanzu Community Edition (TCE).
* Jacob Schulz brought [this article on Terraform modules][link-11] to my attention; if you aren't already familiar with Terraform modules, you may find it helpful.
* In [this article][link-14], Alex Feiszli lays out the argument for a "wide" cluster over multiple clusters, and advocates for potentially spreading that "wide" cluster over multiple cloud providers. I have to say that, in general, I disagree with this architecture. There are a number of reasons (and I won't go into great detail), but some of them include blast radius (if your one control plane goes down, you're in for a world of hurt), lack of support for multiple cloud provider integrations (you'll have to give up most, if not all, cloud provider integrations), and increased complexity (among other things, having to add and maintain additional node labels and node selectors is additional work).
* Speaking of labels, Aviad Shikloshi wrote an article on [best practices for Kubernetes labels and annotations][link-15].
* Chris Evans [attempts to define][link-24] hybrid cloud and multi-cloud.

## Operating Systems/Applications

* Kevin Alvarez shares how to potentially speed up ARM64 build times [using Docker BuildX and AWS Graviton2 instances][link-6].

## Virtualization

* Chris Evans shares [his take on VMware's Project Capitola][link-5].
* Frank Denneman [discusses a potential gotcha][link-20] in the user interface for configuring DRS in vSphere 7.
* Jason Boche walks through [deploying EKS Anywhere on vSphere][link-21].
* William Lam [reviews][link-23] the minimum vSphere editions and features that are required for Tanzu Community Edition (TCE).

## Career/Soft Skills

* I loved [this article][link-13] about the "follow your feet" saying. I'd never heard it before, but I like the idea of applying this to your IT career. Ask the questions you think are stupid questions. Chase down the ideas that seem like they have promise, even if they aren't fully fleshed out yet. Try that new combination of technologies that you think may hold promise for your team or your company. In other words, follow your feet.
* I really enjoyed this article on [rent vs. buy in one's career][link-22].

That's all, folks! (Bonus points if you get this reference.) If you have any questions, comments, or feedback, I'd love to hear from you! Feel free to contact [me on Twitter][link-99], or find me in any of a variety of Slack communities.

[link-1]: https://kubernetes.io/blog/2021/10/08/capi-clusterclass-and-managed-topologies/
[link-2]: https://medium.com/nerd-for-tech/running-services-with-knative-kong-3135c0d94dfa
[link-3]: https://medium.com/codex/kubernetes-network-polices-are-they-really-useful-c3a153c49316
[link-4]: https://www.networkfuntimes.com/why-is-65535-not-part-of-the-private-autonomous-system-range/
[link-5]: https://www.architecting.it/blog/vmworld-2021-project-capitola/
[link-6]: https://www.docker.com/blog/speed-up-building-with-docker-buildx-and-graviton2-ec2/
[link-7]: https://routingcraft.net/arp-problems-in-evpn/
[link-8]: https://blog.malwarebytes.com/malwarebytes-news/2021/10/inside-apple-how-macos-attacks-are-evolving/
[link-9]: https://dev.ms/2021/10/envoy-as-an-api-gateway-part-iv/
[link-10]: https://williamlam.com/2021/10/quick-tip-install-kube-vip-as-service-load-balancer-with-tanzu-community-edition-tce.html
[link-11]: https://spacelift.io/blog/what-are-terraform-modules-and-how-do-they-work
[link-12]: https://www.macsparky.com/blog/2021/10/the-new-macbook-pro
[link-13]: https://adamunwin.com/2018/10/09/follow-your-feet/
[link-14]: https://itnext.io/3-reasons-to-choose-a-wide-cluster-over-multi-cluster-with-kubernetes-c923fecf4644
[link-15]: https://komodor.com/blog/best-practices-guide-for-kubernetes-labels-and-annotations/
[link-16]: https://venilnoronha.medium.com/introduction-to-original-destination-in-envoy-d8a8aa184bb6
[link-17]: https://jimmysong.io/en/blog/understanding-how-envoy-sidecar-intercept-and-route-traffic-in-istio-service-mesh/
[link-18]: https://www.anandtech.com/show/16983/the-apple-a15-soc-performance-review-faster-more-efficient
[link-19]: https://jvns.ca/blog/2021/10/05/tools-to-look-at-bgp-routes/
[link-20]: https://frankdenneman.nl/2021/10/22/drs-threshold-1-does-not-initiate-load-balancing-vmotions/
[link-21]: http://www.boche.net/blog/2021/10/13/deploying-amazon-eks-anywhere-on-vsphere/
[link-22]: https://cate.blog/2021/10/12/the-rent-versus-buy-of-career-growth/
[link-23]: https://williamlam.com/2021/10/minimum-vsphere-edition-features-for-tanzu-community-edition-tce.html
[link-24]: https://www.architecting.it/blog/defining-hybrid-multi-cloud/
[link-99]: https://twitter.com/scott_lowe
