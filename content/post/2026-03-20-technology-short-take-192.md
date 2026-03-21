---
author: slowe
categories: Information
comments: true
date: 2026-03-20T09:00:00-05:00
tags:
- Apple
- AWS
- CLI
- GitHub
- Go
- Google
- Hardware
- iOS
- Kubernetes
- Linux
- macOS
- Networking
- Security
- Storage
- Virtualization
title: Technology Short Take 192
url: 2026/03/20/technology-short-take-192
---

Welcome to Technology Short Take #192! Who's interested in some links to data center technology-related articles and posts? If that's you, you're in the right place. Here's hoping you find something useful!<!--more-->

## Networking

* Interested in [an autonomous network engineering agent powered by Claude][link-6]? (Hat tip to Russ S. for sending this my way.)

## Security

* Google's Threat Intelligence Group recently [published some details around Coruna][link-8], an iOS exploit kit targeting a range of iOS versions and equipped with a number of potential exploits.
* As an example of one of the challenges around "default" secuity settings, Rory McCune [highlights the fact that MicroK8s does not enable role-based access control][link-19].
* Bryce Kerley [explains Web PKI][link-22]---the infrastructure that powers secure connections on the modern Web.
* Mike Masnick writes about [the massive surveillance stack hiding inside the "age verification" checks][link-23].

## Cloud Computing/Cloud Management

* I've had this article on [AWS Lambda for the containers developer][link-5] sitting in my read queue since first publication in 2023. (Sorry, Massimo.) I finally got around to reading it---really reading it, not just skimming it---and I found it to be helpful in helping me get a better grasp on Lambda.
* The AWS Load Balancer Controller recently [gained support for Kubernetes Gateway API][link-17]. This allows Kubernetes administrators to use AWS ALBs or NLBs for Gateway resources, and eliminates annotation-based configuration in favor of Custom Resource Definitions (CRDs).

## Operating Systems/Applications

* While using containers is certainly a step forward in securing autonomous agents---such as what [NanoClaw][link-7] is doing---I don't think it's enough on its own. Helpful, yes, but there is still more to do. Am I wrong?
* Does [Gentoo's migration from GitHub to Codeberg][link-9]---obstensibly due to Microsoft's AI emphasis---signal a turning point?
* The recent [announcement of a partnership between Motorola and GrapheneOS][link-10] opens the possibility for the privacy-focused smartphone OS to arrive on new hardware platforms.
* Although some sites are sensationalizing early news about Windows 12 (in particular, how much of the OS will be subscription-based), the [PC World article][link-11] that appears to be the source for these sites paints a different picture.
* Tara Tarakiyee says [it's not about "the year of the Linux desktop," it's about the decade of Linux][link-12].
* KubeSimplify [introduces `ing-switch`][link-15], a tool to assist in migrating away from Ingress NGINX to Traefik or Gateway API.
* The `fmd` command-line tool provides [an way to find Markdown files based on metadata][link-16]. I've been looking for something like this!
* Nick Janetakis has [a shell alias for getting outdated packages][link-18] across distributions (Arch, Debian, and macOS+Homebrew).

## Programming/Development

* I was recently introduced to `ast-grep` ([website][link-1], [GitHub repository][link-2]), a CLI tool that leverages the abstract syntax tree (AST) of code to allow users to find (and potentially change) values in code without great effort and at scale. I am not a programmer by trade so I do not have any immediate use cases, but there is one use case that I am exploring (if it works out, it will be the topic of a future blog post). Here is [one example of what you can do with `ast-grep`][link-3].
* Datadog reviews [how they reduced the size of their Agent Go binaries by 77%][link-13].
* Andrew Nesbitt makes the assertion that [package registries are more than just infrastructure---they are also governance][link-14].
* Via Chris Short, I noted the OSI [has adopted SPDX IDs for open source license URLs][link-20]. (By the way, Chris' [DevOps'ish newsletter][link-21] is outstanding---highly recommended.)
* David Anderson [shares some thoughts on LLM coding][link-24]. One quote really stood out to me: "The psychology of LLMs use worries me deeply. It’s a system that could not be better designed to encourage harmful behaviors if you tried, because it mixes a couple of very potent psychological traps. And it’s further being pushed by a large amount of venture money, and being co-opted by grifters with vested interests."
* Leonardo de Moura [argues for the need for formal verification of AI-written software][link-25].

## Career/Soft Skills

* A colleague (thanks James L.!) shared [this rather long essay on generative AI][link-4] with me. While the article is a lengthy read, it really resonated with me. I am including it here in this situation because so much of the discusion in the article relates to our careers and how we view our work.
* These two articles ([part 1][link-26], [part 2][link-27]) paint a not-rosy picture of the future should AI have the impact people are saying (hoping?) it will have. (Hat tip to Michael K for the link.)

That's all I have for now. I love to hear from readers, so feel free to reach out and say hi! You can find me on [X/Twitter][link-97], [Mastodon][link-98], and [Bluesky][link-99]. My email address isn't hard to find, so you also have the option of sending me a message that way. Thanks for reading!

[link-1]: https://ast-grep.github.io/
[link-2]: https://github.com/ast-grep/ast-grep
[link-3]: https://www.darricheng.com/posts/first-use-of-ast-grep/
[link-4]: https://www.scottsmitelli.com/articles/you-dont-have-to/
[link-5]: https://aws.amazon.com/blogs/containers/aws-lambda-for-the-containers-developer/
[link-6]: https://github.com/automateyournetwork/netclaw
[link-7]: https://nanoclaw.dev/
[link-8]: https://cloud.google.com/blog/topics/threat-intelligence/coruna-powerful-ios-exploit-kit/
[link-9]: https://fossforce.com/2026/03/gentoo-starts-exit-from-githubs-microsoft-and-copilot-infested-walled-garden/
[link-10]: https://itsfoss.com/news/motorola-grapheneos-team-up/
[link-11]: https://www.pcworld.com/article/3068331/windows-12-rumors-features-pricing-everything-we-know-so-far.html
[link-12]: https://tarakiyee.com/for-the-love-of-the-linux-desktop/
[link-13]: https://www.datadoghq.com/blog/engineering/agent-go-binaries/
[link-14]: https://nesbitt.io/2025/12/22/package-registries-are-governance-as-a-service.html
[link-15]: https://blog.kubesimplify.com/ing-switch-migrate-from-ingress-nginx-to-traefik-or-gateway-api-in-minutes-not-days
[link-16]: https://github.com/zhouer/fmd
[link-17]: https://aws.amazon.com/blogs/networking-and-content-delivery/aws-load-balancer-controller-adds-general-availability-support-for-kubernetes-gateway-api/
[link-18]: https://nickjanetakis.com/blog/a-shell-alias-for-getting-outdated-packages-in-arch-debian-and-macos
[link-19]: https://raesene.github.io/blog/2026/03/11/microk8s-rbac-default/
[link-20]: https://opensource.org/blog/osi-adopts-spdx-ids-for-license-urls
[link-21]: https://buttondown.com/devopsish
[link-22]: https://blog.brycekerley.net/2026/03/08/webpki-and-you.html
[link-23]: https://www.techdirt.com/2026/02/25/hackers-expose-the-massive-surveillance-stack-hiding-inside-your-age-verification-check/
[link-24]: https://blog.dave.tf/post/coding-agents/
[link-25]: https://leodemoura.github.io/blog/2026-2-28-when-ai-writes-the-worlds-software-who-verifies-it/
[link-26]: https://alapshah1.substack.com/p/the-global-intelligence-crisis
[link-27]: https://www.citriniresearch.com/p/2028gic
[link-97]: https://twitter.com/scott_lowe
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://bsky.app/profile/scottslowe.bsky.social
