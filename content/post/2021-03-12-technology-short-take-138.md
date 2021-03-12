---
author: slowe
categories: Information
comments: true
date: 2021-03-12T09:50:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Cilium
- Kubernetes
- Apple
- Messaging
- AWS
- Fedora
- VMware
- vSphere
- VirtualBox
- KVM
title: 'Technology Short Take 138'
url: /2021/03/12/technology-short-take-138/
---

Welcome to Technology Short Take #138. I have what I hope is an interesting and useful set of links to share with everyone this time around. I didn't do so well on storage links; apologies to my storage-focused friends! However, there should be something for most everyone else. Enjoy!<!--more-->

## Networking

* I've been interested in learning more about gRPC, so this guide on [analyzing gRPC messages using Wireshark][link-8] may be useful.
* Isovalent, the folks behind Cilium, [recently unveiled][link-12] the Network Policy Editor, a graphical way of editing Kubernetes Network Policies.
* Ivan Pepelnjak, the font of all networking knowledge, has been discussing cloud networking in some detail for a good while now. The latest series of posts (found [here][link-18] and [here][link-19]) are, in my opinion, just outstanding. I want to be like Ivan when I grow up. `#BeLikeIvan`
* If you work with TextFSM templates (see [here][link-21] for more information), then you might also like this post on [writing a `vim` syntax plugin][link-20] for TextFSM templates.
* Want/need to better understand IPv6? Denise Fishburne [has you covered][link-23]. Denise also has you covered if you need [BGP knowledge][link-30].

## Security

* The Google Project Zero blog takes [a look at iMessage in iOS 14][link-1].
* I recently came across [this series on AWS security][link-3] by ScaleSec. There's a lot here!
* Snort is a product I haven't heard about in ages (so it seems), but I recently saw [this news that Snort 3.x was now available][link-4].
* Using a package manager like NPM, PyPi, or others? You may want to have a look at [this software supply chain attack][link-11] that takes advantage of the way these package managers handle dependencies.
* [Ouch][link-22].
* [Intentionally leaking AWS keys][link-26]? No thanks!

## Cloud Computing/Cloud Management

* It took me a while to get around to reading it, but this post from Massimo Re Ferre on [the role of AWS Fargate in the container world][link-2] was useful in helping me wrap my head around this technology and its place. 
* [This blog post by Alex DeBrie on AWS API Gateway access logs][link-5] is a real _tour de force_. This is truly an information-dense article, and if working with the AWS API Gateway falls into your job responsibilities then this is probably a good post to read.
* If you're interested in doing some customization of Tanzu Basic, [this post][link-7] may prove useful.
* William Lam shares [a few Kubernetes tips and tricks][link-13].
* Here's Chip Zoller's [comparison of Gatekeper and Kyverno][link-14].
* Nic Cope has [an interesting post][link-15] discussing Crossplane and some design/implementation considerations.
* Katie Gamanji shares [some information on passing the CKAD][link-17].
* [This article][link-25] by Vlad Ionescu on EKS DNS at scale is a great read, well worth your time if you work with Kubernetes.

## Operating Systems/Applications

* Alex Gurbych [discusses][link-16] a microservices architecture, including adapting the concepts to actual services and offerings from AWS.
* Ricardo Ferreira explores [using an Elgato Stream Deck with Fedora][link-28].

## Virtualization

* Here's one users guide to building [a vSphere 7 home lab][link-6].
* Via TecMint, James Kiarie explains [how to use VirtualBox VMs on KVM in Linux][link-27]. Unfortunately, the article only really focuses on disk conversion/migration and only has a passing reference to creating the rest of the VM configuration.

## Career/Soft Skills

* I came across a couple "my home office setup" posts over the last month that I thought I'd share here. First up is [this post][link-9] by Clint Wyckoff; I must say I do like the ambient backlighting and I appreciate that Clint included a comprehensive parts list. Next up is [a rather lengthy post by Falko Banaszak][link-10], in which he shares a pretty comprehensive view of what he uses in his home office.
* Nick Korte discusses [the importance of intentional practice][link-24], something I think is important in all sorts/types of careers. In IT, where change is a constant, it's particularly important.
* I found [this article on depletion][link-29] to be meaningful to me on a personal level.

That's all for this time around! I hope that I've managed to include something useful or helpful to my readers. As always, I welcome all constructive feedback and I love hearing from readers, so feel free to reach out to [me on Twitter][link-99]. Thank you for reading!

[link-1]: https://googleprojectzero.blogspot.com/2021/01/a-look-at-imessage-in-ios-14.html
[link-2]: https://aws.amazon.com/blogs/containers/the-role-of-aws-fargate-in-the-container-world/
[link-3]: https://scalesec.com/aws-series/
[link-4]: https://9to5linux.com/snort-3-open-source-intrusion-prevention-system-released-with-major-new-features
[link-5]: https://www.alexdebrie.com/posts/api-gateway-access-logs/
[link-6]: https://vkasaert.wordpress.com/2021/02/07/my-vsphere-7-homelab/
[link-7]: https://vdan.niceneasy.ch/vmware-tanzu-basic-customizing/
[link-8]: https://grpc.io/blog/wireshark/
[link-9]: https://cdubhub.us/2020/07/12/my-home-office-setup/
[link-10]: https://www.virtualhome.blog/2021/01/08/my-home-office-upgrade-and-setup/
[link-11]: https://www.bleepingcomputer.com/news/security/researcher-hacks-over-35-tech-firms-in-novel-supply-chain-attack/
[link-12]: https://cilium.io/blog/2021/02/10/network-policy-editor
[link-13]: https://www.virtuallyghetto.com/2021/02/useful-kubernetes-tricks-tools.html
[link-14]: https://neonmirrors.net/post/2021-02/kubernetes-policy-comparison-opa-gatekeeper-vs-kyverno/
[link-15]: https://blog.crossplane.io/crossplane-vs-cloud-infrastructure-addons/
[link-16]: https://levelup.gitconnected.com/microservices-architecture-74c26df8688
[link-17]: https://kgamanji.medium.com/how-i-passed-my-ckad-with-97-6b54dcffa72f
[link-18]: https://blog.ipspace.net/2021/02/public-cloud-regions-availability-zones.html
[link-19]: https://blog.ipspace.net/2021/02/vpc-subnets-aws-azure-gcp.html
[link-20]: https://www.oasys.net/posts/writing-a-vim-syntax-plugin/
[link-21]: https://github.com/google/textfsm
[link-22]: https://github.com/brandongalbraith/endgame
[link-23]: https://www.networkingwithfish.com/understanding-ipv6-7-part-series/
[link-24]: https://blog.thenetworknerd.com/2021/02/20/when-practice-doesnt-make-perfect/
[link-25]: https://www.vladionescu.me/posts/eks-dns/
[link-26]: https://brokenco.de//2021/01/15/leaking-aws-keys.html
[link-27]: https://www.tecmint.com/migrate-virtualbox-vms-into-kvm-vms/
[link-28]: https://riferrei.com/2021/02/07/using-stream-deck-with-fedora/
[link-29]: https://sonjablignaut.medium.com/on-depletion-5be42595611d
[link-30]: https://www.networkingwithfish.com/bgp-show-and-tell-beginners/
[link-99]: https://twitter.com/scott_lowe
