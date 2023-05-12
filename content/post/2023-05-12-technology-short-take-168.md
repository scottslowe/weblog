---
author: slowe
categories: Information
comments: true
date: 2023-05-12T10:00:00-06:00
tags:
- AWS
- Azure
- BGP
- DHCP
- Encryption
- ESXi
- Git
- Hardware
- Kong
- Microsoft
- Networking
- Pulumi
- Security
- Storage
- Virtualization
- VMware
- VPN
- vSphere
- VXLAN
- Windows
title: 'Technology Short Take 168'
url: /2023/05/12/technology-short-take-168/
---

Welcome to Technology Short Take #168! Although this weekend is (in the US, at least) celebrated as Mother's Day weekend---don't forget to call or visit your mom!---I thought you all might want some light weekend reading. I'm here to help, after all. To that end, here's the latest Technology Short Take, with links to a variety of articles in various disciplines. Enjoy!<!--more-->

## Networking

* Ivan Pepelnjak has been on a bit of a DHCP relaying kick. He's recently published articles on [DHCP relaying in VXLAN segments][link-2], [DHCP relaying in EVPN VRFs][link-3], and most recently [DHCP relaying with redundant DHCP servers][link-4].
* William Collins has a two-part series on site-to-site VPN with AWS. In part 1, Collins shows how [VPN connections directly to VPCs don't scale][link-6] (both bandwidth-wise and operationally). In part 2, Collins shows how [Transit VPC and Transit Gateway address scaling concerns][link-7].
* Daryll Swer talks about [BGP zombie routes on JunOS][link-14].
* One notable challenge with Kubernetes is handling traffic outbound from the cluster and securing (or controlling) that traffic. Aadhil Abdul Majeed with Tigera reviews how [using Calico Egress gateway and access controls][link-19] enables users/operators to secure this outbound traffic.

## Security

* Via Russ White, I came across [an article on Fully Homomorphic Encryption (FHE) as a privacy-enhancing technology (PET)][link-13]. Unfortunately, the article didn't even bother to define terms like FHE and PET, which made it quite difficult to parse. Let this be a reminder to you (and me!): not everyone immediately understands the acronyms you're using, so define them on first use, please.
* In [Technology Short Take 166][xref-1] I linked to an article about BlackLotus, an exploit capable of bypassing UEFI Secure Boot. Microsoft responded to the BlackLotus threat with this [guidance related to Secure Boot Manager changes][link-24] which address the bypass vulnerability used by BlackLotus.
* Again I ask: why [Cedar][link-26] instead of Open Policy Agent (OPA)? What problems does Cedar solve that OPA doesn't? Is it performance? Scale? Flexibility? Does AWS feel that Cedar is "more secure" or "more correct" than OPA?

## Cloud Computing/Cloud Management

* I shared this on Twitter recently, but I wanted to share it here as well. David Egbert wrote up a tutorial on [using Pulumi to deploy a production-grade static site on AWS][link-10]. What I liked about David's tutorial---in contrast to so many others---is that David didn't stop at the standard S3/CloudFront component. He went on to add Route53 entries _and_ TLS certificates via ACM. Additionally, David showed one way to break up your Pulumi code (he used Python) into multiple files. Well worth the read, in my opinion.
* Via a colleague at work, I was pointed to Jim Ferrari's article on [integrating Azure Key Vault with Azure Kubernetes Service][link-12]. (Completely unrelated side note: What a cool last name! Thankfully he didn't have the last name Lamborghini, can you imagine trying to learn to spell that as a kid in school?)
* I appreciate Nigel Poulton's clear writing on WebAssembly, containerd, and WASM. First, he has a two-part series discussing WebAssembly on Kubernetes; part 1 provides [a high-level overview of the various components][link-15], and part 2 is more of [a hands-on tutorial][link-16]. Also, Nigel has another article on [understanding containerd shims as they relate to WebAssembly][link-17].
* Yan Cui addresses concerns about [complex serverless application diagrams][link-21].
* I also found an article about a really neat Cilium feature involving integration with AWS security groups. I'm not providing a link here because it was behind a "regwall" (you had to create a free account in order to read it). I'm not a fan of such mechanisms, so...
* Eric Pauley's analysis of rising EC2 spot instance pricing prompts the question of whether we should bid [farewell to the era of cheap EC2 spot instances][link-25].

## Operating Systems/Applications

* Just when you think you're getting a handle on Git, then it all blows up in your face and you realize you have no idea how Git works. At least Nick Janetakis' recent article can show you how to [restore files to their latest commit][link-5] (along with deleting untracked files, too).
* Sven Walther discusses [disaster recovery for the Kong API Gateway][link-8].
* Jimmi Dyson provides [an overview of two very useful container image tools][link-20] (`skopeo` and `crane`).
* This is a slightly older post, but if you're looking for [an introduction to Git][link-22] this is a reasonable place to start (especially if you're not a developer, as this article was written specifically to non-developer audiences).

## Storage

* Peter Boros with Percona reviews [the performance of various EBS storage types in AWS][link-18]. The details on gp2 versus gp3 are particularly interesting, in my opinion, and show that price isn't the only dimension by which you need to measure your configuration.

## Virtualization

* Daniël Zuthof shows readers how to [fix missing NIC drivers for ESXi hosts][link-9].
* Christian Mohn shares more information---and a resolution---for [an obscure error when upgrading to vCenter 8 Update 1][link-11].

## Career/Soft Skills

* I appreciated Carly Richmond's piece on [respecting the social battery][link-1]. There are times when my social battery is depleted and I need a bit of quiet time to recharge. I also appreciated Carly's description of extroversion and introversion as a spectrum; so many discussions of this topic tend to classify folks as either one or the other, instead of recognizing that people can have tendencies of both.
* Chasing the productivity Holy Grail can be draining...and even counter-productive. Josh Mitrani's [review of the book titled _How to Calm Your Mind_][link-23] (written by former "productivity guru" Chris Bailey) talks a bit about that. I can definitely identify with some of the things mentioned here.

That's all I have for you this time around; hopefully you've found something helpful, informative, educational, or at least interesting. If you have feedback for me on this post (or _any_ post on my site), I always love hearing from readers. The best way to interact with me is via [Twitter][link-98], [Mastodon][link-99], or Slack (you can find me in the Kubernetes and Pulumi communities pretty much all the time). Feel free to reach out!

[link-1]: https://carlyrichmond.com/2023/04/05/charge-me-up/
[link-2]: https://blog.ipspace.net/2023/03/netlab-vxlan-dhcp-relay.html
[link-3]: https://blog.ipspace.net/2023/04/netlab-evpn-dhcp-relay.html
[link-4]: https://blog.ipspace.net/2023/04/dhcp-redundant-relay.html
[link-5]: https://nickjanetakis.com/blog/git-restore-all-unstaged-and-untracked-files-back-to-their-latest-commit
[link-6]: https://wcollins.io/posts/2022/evolution-of-aws-site-to-site-vpn-part-1/
[link-7]: https://wcollins.io/posts/2023/evolution-of-aws-site-to-site-vpn-part-2/
[link-8]: https://svenwal.de/blog/20230404_kong_disaster_recovery/
[link-9]: https://blog.zuthof.nl/2023/04/17/fixing-missing-nic-drivers-for-esxi-hosts/
[link-10]: https://medium.com/@dmegbert/deploying-a-production-grade-static-site-on-aws-using-route53-cloudfront-and-s3-with-pulumi-17d95f9a283a
[link-11]: https://vninja.net/2023/04/19/upgrading-vcenter8-invalid-type/
[link-12]: https://jimferrari.com/2022/02/23/integrate-azure-key-vault-with-azure-kubernetes-service/
[link-13]: https://iapp.org/news/a/the-latest-in-homomorphic-encryption-a-game-changer-shaping-up/
[link-14]: https://blog.apnic.net/2023/04/13/navigating-a-bgp-zombie-outbreak-on-juniper-routers/
[link-15]: https://nigelpoulton.com/webassembly-on-kubernetes-everything-you-need-to-know/
[link-16]: https://nigelpoulton.com/webassembly-on-kubernetes-ultimate-hands-on/
[link-17]: https://nigelpoulton.com/webassembly-and-containerd-how-it-works/
[link-18]: https://www.percona.com/blog/performance-of-various-ebs-storage-types-in-aws/
[link-19]: https://www.tigera.io/blog/using-calico-egress-gateway-and-access-controls-to-secure-traffic/
[link-20]: https://eng.d2iq.com/blog/a-tale-of-two-container-image-tools-skopeo-and-crane/
[link-21]: https://theburningmonk.com/2020/11/even-simple-serverless-applications-have-complex-architecture-diagrams-so-what/
[link-22]: https://infrastructureascode.ch/git101.html
[link-23]: https://bookthoughts.substack.com/p/how-to-calm-your-mind
[link-24]: https://msrc.microsoft.com/blog/2023/05/guidance-related-to-secure-boot-manager-changes-associated-with-cve-2023-24932/
[link-25]: https://pauley.me/post/2023/spot-price-trends/
[link-26]: https://aws.amazon.com/blogs/opensource/using-open-source-cedar-to-write-and-enforce-custom-authorization-policies/
[link-98]: https://twitter.com/scott_lowe
[link-99]: https://fosstodon.org/@scottslowe
[xref-1]: {{< relref "2023-03-10-technology-short-take-166.md" >}}
