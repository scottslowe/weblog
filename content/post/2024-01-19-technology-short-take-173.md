---
author: slowe
categories: Information
comments: true
date: 2024-01-19T09:00:00-06:00
tags:
- AWS
- Cilium
- CLI
- Docker
- Encryption
- ESXi
- Git
- Hardware
- Intel
- Kubernetes
- Linux
- Networking
- Security
- Storage
- Terraform
- TLS
- Virtualization
- VPN
- WireGuard
title: 'Technology Short Take 173'
url: /2024/01/19/technology-short-take-173/
---

Welcome to Technology Short Take #173! After a lull in links to share last time around, it looks like things have rebounded and folks are in full swing writing new content for me to share with you. I think I have a decent round-up of links for you; hopefully you can find something useful here. Enjoy!<!--more-->

## Networking

* This article on [running WireGuard in Docker][link-8] may prove useful if that's an approach I decide to adopt for my AWS lab infrastructure. 
* Natalie Marek [educates readers on VPC endpoints][link-13].
* Russ White [laments some of the issues][link-19] facing network engineering.

## Servers/Hardware

* Alex Ellis provides some details on his workflow for [booting Raspberry Pi 5 from NVMe][link-10].
* Tom Hummel [finds himself veering back][link-11] into a hardware-based home lab (instead of a cloud-based lab).

## Security

* Rory McCune shares some information about [a change in `kubeadm` version 1.29 pertaining to administrative credentials][link-4].
* Quintessence Anx of SPIRL shares some guidance on [how to construct SPIFFE IDs][link-5].
* A set of vulnerabilities in the open source reference implementation of the UEFI specification has been uncovered. The flaws, referred to as PixieFail, specifically affect the PXE network boot process. BleepingComputer has [more details][link-22].

## Cloud Computing/Cloud Management

* Dean has published information on [migrating your Red Hat OpenShift clusters to Cilium][link-6] (from one of the "default" networking solutions).
* I think I've linked to Ricardo Sueiras' "AWS open source newsletter" before; it's such a useful resource. In [edition 184][link-14], Ricardo shares some links to some useful posts on EKS; the one on using Istio with EKS to improve the user experience caught my eye. (Check out the newsletter to get the link to the Istio article.)
* Ian McKay [digs into the details][link-16] of the recently-announced support for HTTPS Endpoints in AWS Step Functions.
* Matt Gowie of MasterPoint [explains `terraform-null-label`][link-24] and its use in providing more consistent naming and tagging of cloud resources.

## Operating Systems/Applications

* Julia Evans has [a lovely article on Git commits][link-1] that is well worth reading. Further, Julia's article links over to this substantial article on [Git packfiles][link-2] by Aditya Mukerjee. Julia has a bunch of other Git-related articles published recently; if you'd like to better understand Git, they're a good resource.
* Kyle Galbraith talks about [using the Docker build cache][link-3].
* Samuel Karp [discusses some deprecation warnings][link-12] in containerd while on the road to version 2.0.
* Jacob Gillespie [decodes some of the basics of OCI container image layers][link-15].
* Vivek Gite aka nixCraft explains [how to check the expiration date of a TLS/SSL certificate from the command line][link-17].
* [This looks interesting][link-18].
* While doing some research on GPG keys, I came across [this tool for monitoring GPG key expiration][link-21].

## Programming/Development

* Jamie Tanna explains [how to represent][link-23] a JSON field in Go that could be absent, `null`, or have a value.

## Virtualization

* Eric Sloof relates his experience [setting up ESXi ARM on a Raspberry Pi 5][link-7].
* William Lam has [published the results][link-20] of some experiments with ESXi CPU affinity and Intel Hybrid CPU cores.

## Career/Soft Skills

* Anyone thinking of getting [a GitHub certification][link-9]?

It's time to wrap up now; as always, I'd love to hear from readers about what you find useful (or not useful!) about the Technology Short Takes---or any of the posts on my site. Feel free to reach out to me via social media; you can find [me on Twitter][link-99] as well as [on the Fediverse][link-30]. I also tend to frequent a few different Slack communities, so you're welcome to DM me there. Finally, if you'd like to drop me an e-mail, my address isn't too hard to find. Thanks for reading!

[link-1]: https://jvns.ca/blog/2024/01/05/do-we-think-of-git-commits-as-diffs--snapshots--or-histories/
[link-2]: https://codewords.recurse.com/issues/three/unpacking-git-packfiles
[link-3]: https://depot.dev/blog/faster-builds-with-docker-caching
[link-4]: https://raesene.github.io/blog/2024/01/06/when-is-admin-not-admin/
[link-5]: https://www.spirl.com/blog/how-to-construct-spiffe-ids/
[link-6]: https://veducate.co.uk/migrate-red-hat-openshiftsdn-ovn-kubernetes-cilium/
[link-7]: https://www.ntpro.nl/blog/archives/3752-Setting-Up-ESXi-ARM-on-the-Raspberry-Pi-5.html
[link-8]: https://www.nikitakazakov.com/wireguard-vpn-in-docker
[link-9]: https://github.blog/2024-01-08-github-certifications-are-generally-available/
[link-10]: https://blog.alexellis.io/booting-the-raspberry-pi-5-from-nvme/
[link-11]: https://tomhummel.com/posts/homelab-2023/
[link-12]: https://samuel.karp.dev/blog/2024/01/deprecation-warnings-in-containerd-getting-ready-for-2.0/
[link-13]: https://dev.to/aws-builders/lets-talk-about-aws-vpc-endpoints-2bj
[link-14]: https://community.aws/content/2arO7cYup4ShOVguSMZfHt9WgJa/aws-open-source-newsletter-184
[link-15]: https://depot.dev/blog/building-container-layers-from-scratch
[link-16]: https://onecloudplease.com/blog/https-endpoints-and-more-tricks-with-aws-step-functions
[link-17]: https://www.cyberciti.biz/faq/find-check-tls-ssl-certificate-expiry-date-from-linux-unix/
[link-18]: https://klog.jotaen.net/
[link-19]: https://rule11.tech/making-networking-cool-again-2/
[link-20]: https://williamlam.com/2024/01/experimenting-with-esxi-cpu-affinity-and-intel-hybrid-cpu-cores.html
[link-21]: https://jasonaowen.net/blog/2021/Jan/04/monitoring-gpg-key-expiration/
[link-22]: https://www.bleepingcomputer.com/news/security/pixiefail-flaws-impact-pxe-network-boot-in-enterprise-systems/
[link-23]: https://www.jvt.me/posts/2024/01/09/go-json-nullable/
[link-24]: https://masterpoint.io/updates/terraform-null-label/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
