---
author: slowe
categories: Information
comments: true
date: 2026-04-03T09:00:00-05:00
tags:
- Apple
- Automation
- Docker
- Encryption
- GitHub
- GitOps
- Hardware
- Istio
- Kubernetes
- Linux
- macOS
- Networking
- Security
- Storage
- Virtualization
- Wireless
- VMware
- vSphere
title: Technology Short Take 193
url: 2026/04/03/technology-short-take-193
---

Welcome to Technology Short Take #193! I know it has only been a couple weeks since the last Tech Short Take, but I am guessing that readers won't really mind another one. Here is my latest collection of articles and posts about data center-related technologies. Enjoy!<!--more-->

## Networking

* Brian Linkletter shares his [2026 list of network simulators and emulators][link-2]. (Hat tip to Ivan P. for the link.)
* Emmanuel Vitus brings to light [a battle over IPv4 address space and the control of the regional internet registries][link-5].
* Doug Dawson discusses [the recent FCC ban on consumer-grade routers][link-26] and the potential impacts to industry and ISPs.

## Servers/Hardware

* [RIP Mac Pro][link-23]. I had a "classic Mac Pro" (2012 era) for a long time, and I loved that system. (I even ran Linux on it for a while.) It is a shame to see it go.
* I mentioned on social media (Mastodon/Bluesky) that I recently purchased all the hardware for a new PC build. It'll be part PC/part home server, as I look to expand the type and scope of services that I self-host. Don't be surprised if a few articles emerge out of this.

## Security

* Trivy's GitHub Actions were compromised by attackers, leading to the exposure of CI/CD secrets. [This post on the Socket blog][link-1] has more details, and Rose Security has a write-up that provides more details on [how a typosquatted domain and a fake version tag were involved][link-19].
* At least one other software supply chain exploit has been tracked back to the Trivy compromise: [the Python `litellm` module on PyPI steals credentials][link-13].
* [The Docker side of the Trivy compromise][link-14] is shared in detail in this Docker blog post.

## Cloud Computing/Cloud Management

* Looking for some help in migrating away from Ingress-NGINX? This [announcement of Ingress2Gateway 1.0][link-6] might be just what you are seeking.
* The FluxCD team published [a post discussing Morgan Stanley's "Stairway to GitOps" presentation][link-11], which describes the firm's five-year journey from push-based pipelines to a self-service GitOps platform.
* Although I am not an AI fanboy, I do recognize that LLMs can offer value in a variety of areas, and so exploring the utility of local LLMs is something of a side project of mine. As a result, this article on [using the Terraform MCP with a local LLM][link-17] was of interest to me.
* [InfraKitchen kind of feels like "Backstage for Terraform."][link-18]
* I've been talking about KISS versus DRY in IaC for a while now, but Rose Security [unpacks KISS versus DRY in IAC][link-20] so much more elegantly than I can.
* I am apparently behind the times, as it was just this past week that I learned about [waypoint proxies in Istio ambient mode][link-24]. John Howard has [a "waypoint proxies the hard way" tutorial][link-25] that might prove useful if you're interested in learning more.

## Operating Systems/Applications

* Akkana Peck shares how it was [an HDMI connection that was causing USB errors in the `dmesg` log][link-3].
* Terence Eden [re-affirms that is 100% OK to wait on AI][link-7].
* And while I am on the topic of AI, I enjoyed this article explaining [some of the rational reasons people may resist AI][link-10] (and other modern technologies).
* Samuel Karp shares a little bit of [the reasoning behind the move to annual LTS releases of containerd][link-8].
* [Bcachefs is out of the Linux kernel][link-9] and moving to DKMS.
* This [tale of fixing eBPF spinlock issues in the Linux kernel][link-12] is a long and detailed read---but interesting! I didn't understand all the code, but I still enjoyed reading about the journey of finding the root cause(s) and addressing them.
* William Collins [introduces `gridctl`][link-16], a tool for managing MCP configurations.
* Andre Garzia shares [why Apple lost them as a customer][link-21]. This is just a continuation of trends that have been around for some time---as far back as 2012 when I [first discussed leaving OS X][xref-1] (as macOS was known then). It is also why the majority of my time is spent on Linux now (I use Ubuntu at work and Arch Linux BTW on my personal systems).

## Storage

* Frank Denis has a good article on [options for full disk encryption on Linux][link-27].

## Programming/Development

* Markus Unterwaditzer shares [how to move from GitHub to Codeberg][link-22]. I stop relying on GitHub alone some time ago, although many of my repositories/projects are still there just for accessibility.

## Virtualization

* Looking for [a few weekend home lab experiments][link-4]?
* William Lam explains [how to create custom Virtual Machine Classes using an updated to the vSphere Automation REST API][link-15].

That's all for this time around. I hope you found something useful! I am always open to hearing from readers, so I invite you to contact me---I am available on social media ([Bluesky][link-97], [Mastodon][link-98], and [X/Twitter][link-99]), and you can find me in some Slack communities (the Kubernetes Slack instace is one notable example). My email address also is not hard to find. I'd love to hear from you!

[link-1]: https://socket.dev/blog/trivy-under-attack-again-github-actions-compromise
[link-2]: https://opensourcenetworksimulators.com/2026/02/open-source-simulator-emulator-in-2026/
[link-3]: https://shallowsky.com/blog/linux/kernel/usb-errors-hdmi.html
[link-4]: https://www.virtualizationhowto.com/2026/03/6-home-lab-experiments-worth-trying-this-weekend/
[link-5]: https://circleid.com/posts/china-afrinic-and-the-dangerous-precedent-that-could-destabilize-the-global-internet
[link-6]: https://kubernetes.io/blog/2026/03/20/ingress2gateway-1-0-release/
[link-7]: https://shkspr.mobi/blog/2026/03/im-ok-being-left-behind-thanks/
[link-8]: https://samuel.karp.dev/blog/2026/03/balancing-feature-velocity-and-stability-in-containerd/
[link-9]: https://www.linuxjournal.com/content/bcachefs-ousted-mainline-kernel-move-dkms-and-what-it-means
[link-10]: https://restofworld.org/2026/techno-negative-thomas-dekeyser-fighting-ai/
[link-11]: https://fluxcd.io/blog/2026/03/stairway-to-gitops-morgan-stanley/
[link-12]: https://rovarma.com/articles/a-tale-about-fixing-ebpf-spinlock-issues-in-the-linux-kernel/
[link-13]: https://futuresearch.ai/blog/litellm-pypi-supply-chain-attack/
[link-14]: https://www.docker.com/blog/trivy-supply-chain-compromise-what-docker-hub-users-should-know/
[link-15]: https://williamlam.com/2026/03/creating-custom-virtual-machine-classes-using-vsphere-api.html
[link-16]: https://wcollins.io/posts/2026/introducing-gridctl/
[link-17]: https://www.linkedin.com/pulse/i-got-terraform-enterprise-work-offline-local-llm-mcp-lavigne-dttfc/
[link-18]: https://opensource.electrolux.one/infrakitchen/
[link-19]: https://rosesecurity.dev/2026/03/20/typosquatting-trivy.html
[link-20]: https://rosesecurity.dev/2025/11/14/kiss-versus-dry-iac.html
[link-21]: https://andregarzia.com/2026/03/apple-just-lost-me.html
[link-22]: https://unterwaditzer.net/2025/codeberg.html
[link-23]: https://basicappleguy.com/basicappleblog/mac-pro
[link-24]: https://istio.io/latest/blog/2023/waypoint-proxy-made-simple/
[link-25]: https://blog.howardjohn.info/posts/waypoint-the-hard-way/
[link-26]: https://potsandpansbyccg.com/2026/03/30/wifi-router-ban/
[link-27]: https://00f.net/2025/02/11/luks/
[link-97]: https://bsky.app/profile/scottslowe.bsky.social
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
