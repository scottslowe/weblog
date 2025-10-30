---
author: slowe
categories: Information
comments: true
date: 2024-08-16T09:00:00-06:00
tags:
- Ansible
- BGP
- Cilium
- CLI
- Cloud
- EKS
- EVPN
- Git
- GitHub
- Google
- Hardware
- Intel
- Linux
- macOS
- Networking
- Security
- Storage
- Tetragon
- Terraform
- Virtualization
- VXLAN
title: 'Technology Short Take 181'
url: /2024/08/16/technology-short-take-181/
---

Welcome to Technology Short Take #181! The summer of 2024 is nearly over, and Labor Day rapidly approaches. Take heart, though; here is some reading material for your weekend. From networking to security and from hardware to the cloud, there's something in here for just about everyone. Enjoy!<!--more-->

## Networking

* Leon Adato has a great piece on [transitioning from being a network engineer to being a cloud engineer][link-2]. The post links to some good resources, and Leon provides three quick "lessons" to help folks get started.
* Geoff Huston discusses [DNS and UDP truncation][link-6].
* Ivan Pepelnjak examines [when you might need to use BGP, EVPN, VXLAN, or SRv6][link-9].

## Servers/Hardware

* Permanent damage? No recall? Ouch! Sean Hollister [discusses][link-10] Intel's responses to questions asked about instabilities in their 13th and 14th Gen Intel Core desktop processors.
* Chaim Gartenberg shares a look back at [10 years of Google's AI-specialized chips][link-13] (the Tensor Processing Units, or TPUs).

## Security

* I enjoy Ben Bornholm's article showing [how to use Cilium and Tetragon to secure a vulnerable web application][link-8]. Practical examples like this are useful. Thanks for sharing, Ben!
* Writing for The Hacker News, Ravie Lakshmanan [discusses CRYSTALRAY and a recent surge of infections][link-4] aiming "to harvest and sell credentials, deploy cryptocurrency miners, and maintain persistence in victim environments."
* Also via The Hacker News, I saw this article about [Singapore banks phasing out OTPs (one-time passwords)][link-5]. I was a bit surprised, to be honest, but the news in the article of sophisticated toolkits aimed at harvesting OTPs provides some explanation why this is happening.
* While patches mitigating the underlying vulnerability are available (the article is from April 2024), this post on [how folks can use Tetragon to detect exploits like the one used by XZ Utils/CVE 2024-3094][link-21] is (to me) informative.

## Cloud Computing/Cloud Management

* It's an older post, sir, but it checks out: Seth Miles shares his experience from 2021 in [building a Cilium-powered EKS cluster using Bottlerocket][link-1]. Seth uses `eksctl` in his post, but I verified it using Pulumi (and I hope to be able to share the Pulumi program I used shortly).
* Matt Gowie of Masterpoint shares some experience as [an early adopter of OpenTofu][link-3].
* Piotr Jastrzebski shares a quick but effective way to [optimize GitHub Actions usage][link-7].
* Jim Bugwadia shares some techniques for [applying the DRY principle to Kyverno policies][link-12]. I believe a trade-off between DRY and readability exists; emphasizing DRY over readability has the potential for some issues. I am not an expert, though, so feel free to disagree. (Although it would be awesome if you reached out to me to explain _why_ you disagree!)
* Eleni Grosdouli shares a way of [deploying and controlling Ciilum on an EKS cluster using Sveltos][link-18]. Before finding this article, I was not aware of [Sveltos][link-19].
* Running Oracle Kubernetes Engine (OKE)? Here's [a write-up on enhancing OKE security with Cilium Network Policy][link-20].

## Operating Systems/Applications

* Bhushan Shah shares some ["lessons learned" using `git bisect`][link-14] to do some Linux kernel troubleshooting.
* Run macOS software on Linux? I saw [Darling][link-15] described on social media as "WINE, but for macOS". Anyway, it's intended to let you run macOS software in a "Darwin emulation/translation" layer on Linux. Nice!
* Andreas Scherbaum reviews [a recent change in some Arch Linux packaging][link-22] around the replacement of BDB with LMDB in the Postfix.
* Kien Nguyen-Tuan reviews [testing Ansible with Molecule][link-23].
* Chris Wood shares their experience [using Linux as a UX designer][link-24].

## Storage

* William Lam tries to get folks excited about [NVMe tiering in vSphere 8 Update 3][link-17]. I'll admit this looks like a pretty cool feature.

## Virtualization

* Stine Elise Larsen [writes about an easily-missed requirement][link-11] for port 9087 to be open between vCenter Server and ESXi hosts when using vSphere Lifecycle Manager.

## Career/Soft Skills

* This doesn't really pertain to a "soft skill," exactly, but the focus of the article is a career skill that many folks feel is important: accounting. Yes, you read that right. It's important to know basic accounting principles, especially for developers building products that move and track money. Check out [this guide to accounting for developers][link-16].

That's all for now, folks! I love hearing from readers, so if you have thoughts about any of the links I've shared---or maybe you have a link or two of your own you think I should include in the next Technology Short Take---drop me a line! You can reach me via e-mail (my address is on this site, not too terribly hard to find to be honest), you can contact [me on Twitter][link-99], you can reach [me on the Fediverse][link-30], or you can hit me up in one of the Slack communities I regularly visit.

[link-1]: https://miles-seth.medium.com/eks-unchained-with-ebpf-and-bottlerocket-1639b011a36a
[link-2]: https://www.kentik.com/blog/calm-down-clouds-not-that-different/
[link-3]: https://masterpoint.io/updates/opentofu-early-adopters/
[link-4]: https://thehackernews.com/2024/07/crystalray-hackers-infect-over-1500.html
[link-5]: https://thehackernews.com/2024/07/singapore-banks-to-phase-out-otps-for.html
[link-6]: https://www.potaroo.net/ispcol/2024-07/truncation.html
[link-7]: https://turso.tech/blog/simple-trick-to-save-environment-and-money-when-using-github-actions
[link-8]: https://holdmybeersecurity.com/2024/07/24/making-damn-vulnerable-web-application-dvwa-almost-unhackable-with-cilium-and-tetragon/
[link-9]: https://blog.ipspace.net/2024/07/bgp-evpn-vxlan-srv6/
[link-10]: https://www.theverge.com/2024/7/26/24206529/intel-13th-14th-gen-crashing-instability-cpu-voltage-q-a
[link-11]: https://vninja.net/2024/07/30/vcenter-upgrade-8.0u3a-vsphere-lifecycle-manager-port-issues/
[link-12]: https://www.cncf.io/blog/2024/07/31/applying-the-dry-principle-to-kyverno-policies/
[link-13]: https://cloud.google.com/transform/ai-specialized-chips-tpu-history-gen-ai
[link-14]: https://blog.bshah.in/2024/08/04/git-bisecting-kernel-some-pitfalls/
[link-15]: https://www.darlinghq.org/
[link-16]: https://www.moderntreasury.com/journal/accounting-for-developers-part-i
[link-17]: https://williamlam.com/2024/08/nvme-tiering-in-vsphere-8-0-update-3-is-a-homelab-game-changer.html
[link-18]: https://egrosdou01.github.io/personal-blog/blog/cilium-eks-sveltos
[link-19]: https://github.com/projectsveltos
[link-20]: https://umashankar-s.medium.com/enhancing-oke-security-with-cilium-network-policy-9535bca0bcc0
[link-21]: https://isovalent.com/blog/post/ebpf-tetragon-xz-utils-cve-policy/
[link-22]: https://andreas.scherbaum.la/post/2024-08-07_warning-btree-var-lib-postfix-smtp_scache-is-unavailable-unsupported-dictionary-type-btree/
[link-23]: https://ntk148v.github.io/posts/testing-ansible-with-molecule/
[link-24]: https://www.chris-wood.design/resources/linux-for-ux-designers
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
