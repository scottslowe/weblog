---
author: slowe
categories: Information
comments: true
date: 2024-11-27T09:00:00-06:00
tags:
- Automation
- AWS
- Docker
- eBPF
- Encryption
- Hardware
- JSON
- Kubernetes
- Kustomize
- Linux
- Networking
- PGP
- Pulumi
- Security
- Storage
- Virtualization
- WireGuard
- YAML
title: 'Technology Short Take 184'
url: /2024/11/27/technology-short-take-184/
---

Welcome to Technology Short Take #184! This Tech Short Take is a bit shorter than the usual ones, but then again this week---at least in the US---is a bit shorter than most weeks due to the Thanksgiving holiday. Even so, I hope that I've managed to include some information that folks find useful. Also, thanks to some feedback from readers, I've tried hard to ensure that links are more descriptive and informative than they've sometimes been in the past; let me know how I did. Now, on to the content!<!--more-->

## Networking

* I love reading geeky blog posts like this one that [combines Linux network namespaces with WireGuard for VPN split tunneling][link-5]. Neat stuff.
* Scott Laird shares his experience [using Pulumi to connect UniFi and Netbox][link-12] together in his home network.

## Security

* Ofek Itach and Yakir Kadkoda [discuss a security vulnerability in AWS CDK][link-1] that could---under the right conditions---allow an attacker to gain administrative access to an AWS account.
* Carlos Mora [reviews a (now-patched) confused deputy vulnerability in Amazon DataZone][link-2].

## Cloud Computing/Cloud Management

* Luc van Donkersgoed explains how he [optimized caching for his AWS News site to improve performance][link-6].
* Rory McCune takes a moment [to explain the many IP addresses of Kubernetes][link-7] (like node addresses, pod addresses, and service addresses).
* [SkyScalpel][link-9] is an open source tool that "obfuscates, deobfuscates, and detects obfuscated JSON documents". It makes perfect sense now, but before reading about SkyScalpel I hadn't considered the idea of obfuscating JSON documents in policy statements (such as an AWS IAM policy) to hide the policy's true intent. Sneaky hackers!
* Adriana Villela [reviews troubleshooting an issue with GitHub Codespaces and KinD][link-10] (Kubernetes-in-Docker).
* The AWS open source newsletter is _always_ a great source of information; check out [issue #204][link-19].
* Similarly, the AWS Cloud Security weekly newsletter is another great source of information. [Here's a link to issue 71][link-20].

## Operating Systems/Applications

* Soatok provides [some guidance on what to use instead of PGP][link-3].
* Announced recently at KubeCon North America, the eBPF community has recently released an eBPF Security Threat Model and an eBPF verifier code audit. More details are available in [this blog post from the eBPF Foundation][link-4].
* [This blog post lays out a slightly different reason][link-8] for choosing Linux as your primary operating system.
* Chainguard [recently released kernel-independent FIPS images][link-11].
* I like `kustomize` but it seems like the functionality the tool offers---as well as the syntax behind the YAML files it uses---is constantly changing. Here's a case in point: Nick Janetakis [describes the replacement functionality for `commonLabels`, `patchesJson6902`, and `patchesStrategicMerge`][link-18]. I genuinely believe that one of the reasons `kustomize` hasn't seen greater adoption is that users are having a hard time keeping up with all the changes required to use it.

## Storage

* Stephen Foskett discusses [how to migrate from Docker Volumes to external storage][link-15].

## Career/Soft Skills

* Bluesky has been taking off recently as an alternative/replacement for X/Twitter; you can even find [yours truly on Bluesky][link-17]. Bret Fisher has [a good "introduction"-type post on Bluesky][link-13], and [this treatise is helpful if you're concerned/wondering about Bluesky and decentralization][link-14].
* Tom Hollingsworth [extols the virtues of being flexible][link-16] by invoking Gumby, that lovable childhood toy.

That's all for this week! To my readers in the US, I hope that you have a safe and enjoyable Thanksgiving holiday. In spite of whatever may be happening in the world, there are many things for which to be thankful! For my readers outside the US, I hope that you have an enjoyable rest of the week. Be thankful for the decrease in email from your US-based colleagues! Finally, feel free to follow me or interact with me on social media; I'm available [on Twitter][link-99], [on the Fediverse][link-30] (via Mastodon), and [on Bluesky][link-17]. I'd love to hear from you!

[link-1]: https://www.aquasec.com/blog/aws-cdk-risk-exploiting-a-missing-s3-bucket-allowed-account-takeover/
[link-2]: https://trustoncloud.com/blog/confused-deputy-vulnerability-in-amazon-datazone/
[link-3]: https://soatok.blog/2024/11/15/what-to-use-instead-of-pgp/
[link-4]: https://ebpf.foundation/threat-model-and-independent-verifier-audit-examine-the-security-of-ebpf/
[link-5]: https://blog.thea.codes/nordvpn-wireguard-namespaces/
[link-6]: https://lucvandonkersgoed.com/2024/11/16/making-aws-news-stupid-fast-with-smart-caching/
[link-7]: https://raesene.github.io/blog/2024/11/01/The-Many-IP-Addresses-Of-Kubernetes/
[link-8]: https://www.rugu.dev/en/blog/linux-asceticism/
[link-9]: https://permiso.io/blog/introducing-sky-scalpel-open-source-tool
[link-10]: https://geekingoutpodcast.substack.com/p/that-time-when-kind-stopped-working
[link-11]: https://www.chainguard.dev/unchained/kernel-independent-fips-images
[link-12]: https://scottstuff.net/posts/2024/11/18/unifi-netbox-and-pulumi/
[link-13]: https://www.bretfisher.com/cloud-native-is-hot-on-bluesky-cndo-68-2/
[link-14]: https://dustycloud.org/blog/how-decentralized-is-bluesky/
[link-15]: https://blog.fosketts.net/2024/11/01/how-to-migrate-from-docker-volumes-to-external-storage/
[link-16]: https://networkingnerd.net/2024/09/24/semper-gumby/
[link-17]: https://bsky.app/profile/scottslowe.bsky.social
[link-18]: https://nickjanetakis.com/blog/replace-kustomize-commonlabels-patchesstrategicmerge-and-patchesjson6902
[link-19]: https://dev.to/aws/aws-open-source-newsletter-204-4pgg
[link-20]: https://aws-cloudsec.com/p/issue-71
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
