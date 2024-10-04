---
author: slowe
categories: Information
comments: true
date: 2024-10-04T09:00:00-06:00
tags:
- Apple
- AWS
- eBPF
- Hardware
- Istio
- Kubernetes
- Linux
- macOS
- Networking
- OPA
- Security
- Storage
- Virtualization
- VMware
- vSphere
title: 'Technology Short Take 183'
url: /2024/10/04/technology-short-take-183/
---

Welcome to Technology Short Take #183! Fall is in the air; the nights and mornings are cooler and the leaves are turning (or have already turned in some areas!). I've got a slightly smaller collection of links for you this time around, but I do hope that you'll find something shared here useful. Enjoy!<!--more-->

## Networking

* Network World provides some coverage of the recent [2024 eBPF Summit][link-10], discussing [why eBPF is critical and how it's getting better][link-5].
* I first learned of [Kmesh][link-18] via [this article by Jimmy Song][link-17]. There's no doubt eBPF is impacting lots of different areas of networking; this is just one more example.
* ARM-based architectures continue to see expanded usage in lots of different areas; here's just [one more example][link-21].

## Security

* [This article][link-1] is a fascinating look into a series of misconfigurations and security flaws that culminates with full cluster admin privileges and access to internal software repositories. Wow! Remember, kids: [outbound sidecars are not secure enforcement points][link-2].
* A group of individuals being able to [accidentally become the admins of a top-level domain (TLD)][link-4] underscores just how fragile many parts of the Internet are today.
* Ricky Mondello (who works on Apple's new Passwords app) advises people to [consider slowing down when switching password managers][link-11].
* James Sheard discusses a (now patched) [security flaw in AWS Transit Gateway Peering Attachments][link-14].
* [This article][link-16] discusses a security vulnerability in the Arc browser; more specifically, in the Arc browser's companion online services.
* A botnet of up to [a quarter million devices][link-20]?

## Cloud Computing/Cloud Management

* Spurred on by a comment from a colleague that in turn pointed me to a random GitHub issue, I've learned about CEL (Common Expression Language). You can learn more about CEL via [the CEL web site][link-7]. Why does this matter? This is notable for [its inclusion in Kubernetes][link-8], and [the use of CEL in Validating Admission Policies][link-9] (which are GA as of Kubernetes 1.30). I do wonder about the future of other tools used for admission control (top of mind for me is OPA/Gatekeeper) and how CEL will affect them.
* In the event you aren't familiar with the structure of a Kubeconfig file (for connecting to a Kubernetes cluster), [this article][link-22] is somewhat helpful.

## Operating Systems/Applications

* [Minderbinder][link-6] is a tool that injects failures into running processes via eBPF. I could be reading this wrong, but it looks like Minderbinder is (currently) focused around injecting network-centric failures.
* I haven't tried [this][link-12] out yet, but it looks interesting/useful.
* Thinking of trying Arch Linux? Here's [a walkthrough of setting it up on a laptop][link-13].
* I am absolutely in love with [`kubecolor`][link-19].

## Programming/Development

* Here's a good article on [rate limiting, cells, and GCRA][link-15].

## Virtualization

* Gina Minks [mourns the loss of the vCommunity][link-3] after attending VMware Explore (formerly VMworld) in Las Vegas. While we might disagree whether the Broadcom acquisition was a good thing or not, and while we might disagree about the future of VMware, I think we _can_ agree that the VMware Community of days past is on its way out (some might say it's already gone). Gina rightfully calls out just how unique the VMware community was during its heyday---I am thankful to have been a small part of it.
* Eric Sloof recently [shared a link][link-23] to a set of performance best practices for vSphere 8.0 Update 3, if that's what you're using.

That's all for now, folks! Thanks for reading; I appreciate the opportunity to share information with you. If you have any feedback for me---or if you just want to say hi---feel free to reach out to me [on Twitter][link-99], on [the Fediverse][link-30], in one of the Slack communities I frequent, or by dropping me an e-mail (my address isn't hard to find). I'd love to hear from you!

[link-1]: https://www.wiz.io/blog/sapwned-sap-ai-vulnerabilities-ai-security
[link-2]: https://blog.howardjohn.info/posts/bypass-egress/
[link-3]: https://digitalsunshinesolutions.com/mourning-our-vcommunity-navigating-grief-after-the-broadcom-acquisition/
[link-4]: https://labs.watchtowr.com/we-spent-20-to-achieve-rce-and-accidentally-became-the-admins-of-mobi/
[link-5]: https://www.networkworld.com/article/3518212/why-ebpf-is-critical-and-how-its-getting-better.html
[link-6]: https://blog.scottgerring.com/introducing-minderbinder/
[link-7]: https://cel.dev
[link-8]: https://kubernetes.io/docs/reference/using-api/cel/
[link-9]: https://medium.com/google-cloud/validating-admission-policies-with-gke-1-26-ed1321bcf739
[link-10]: https://ebpf.io/summit-2024/
[link-11]: https://rmondello.com/2024/09/19/consider-slowing-down-when-switching-password-managers/
[link-12]: https://github.com/intermezzio/weffe
[link-13]: https://giacomo.coletto.io/blog/arch-amd/
[link-14]: https://engineering.doit.com/aws-transit-gateway-peering-exploit-a1715edd4c8a
[link-15]: https://brandur.org/rate-limiting
[link-16]: https://kibty.town/blog/arc/
[link-17]: https://jimmysong.io/en/blog/introducing-kmesh-kernel-native-service-mesh/
[link-18]: https://kmesh.net/en/
[link-19]: https://kubecolor.github.io/
[link-20]: https://www.bleepingcomputer.com/news/security/flax-typhoon-hackers-infect-260-000-routers-ip-cameras-with-botnet-malware/
[link-21]: https://blog.ipspace.net/2024/09/srlinux-arm-apple-silicon/
[link-22]: https://pixelrobots.co.uk/2024/09/understanding-the-kubeconfig-file-insights-from-building-kubetidy/
[link-23]: https://www.ntpro.nl/blog/archives/3770-Performance-Best-Practices-for-VMware-vSphere-8.0-Update-3.html
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
