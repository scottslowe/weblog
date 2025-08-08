---
author: slowe
categories: Information
comments: true
date: 2025-08-08T09:00:00-04:00
tags:
- Apple
- AWS
- BGP
- Cisco
- CLI
- EKS
- Git
- Hardware
- IaC
- IPv6
- Kubernetes
- Networking
- OPA
- Security
- Storage
- Terraform
- Virtualization
- VMware
- vSAN
title: 'Technology Short Take 187'
url: /2025/08/08/technology-short-take-187/
---

Welcome to Technology Short Take #187! In this Technology Short Take, I have a curated collection of links on topics ranging from BGP to blade server hardware to writing notes using a "zettelkasten"-style approach, along with a few other topics thrown in here and there for fun. I hope you find something useful!<!--more-->

## Networking

* Jason Gintert justifies the value behind [using IRRs and RPSL to keep your public BGP routing working well][link-6].
* IPv6 has a new documentation prefix, as outlined [by Ivan Pepelnjak in his blog post][link-10] as well as [by Nick Buraglio in his blog post][link-11].

## Servers/Hardware

* Kevin Houston takes [a look at the latest blade server offerings][link-20] from Cisco and HPE.

## Security

* This is an older article dating back to the start of 2025, but it [outlines a potential security flaw][link-5] that allowed hackers to hijack Subaru cars in the US and Canada.
* Rory McCune [tackles PKI in this post of his series on Kubernetes security fundamentals][link-14].
* Daniel Stenberg (of `curl` fame) explains [why the `curl` project doesn't use CVSS scores][link-16] and describes some of the problems that result from this decision.

## Cloud Computing/Cloud Management

* I've spoken about Cedar before here on this site. The first mention of Cedar was in [TST 165][xref-1]; later, in [TST 168][xref-2], I pondered why AWS went through the effort of creating and open-sourcing Cedar. In a paper on Cedar (linked to via [this post][link-8]), I learned that consistent performance at scale was one benefit of Cedar when compared to other policy solutions, including Open Policy Agent (OPA). Good to know!
* Also on the topic of Cedar: here's [an implementation of Kubernetes RBAC in Cedar][link-9].
* This blog post provides some insight on [how Amazon scaled EKS to support up to 100,000 nodes][link-13].
* The Infisical team [provides a case for using Terraform modules][link-17].
* Looking for [a list of useful, IaC-adjacent tools][link-18]? Look no further.

## Operating Systems/Applications

* Marcus Hutchins [explains his stance on AI][link-1]. It's a bit of a long read, but he goes into great detail for all the reasons why he's opposed to AI in its current form (LLMs).
* Julian Hofer [discusses][link-3] CLI tools (`gh` and `glab`, for GitHub and GitLab, respectively) and how they enhance the `git` CLI experience when working with these online services (which he calls "Git forges").
* The Register [clues its readers in][link-4] on some security changes Apple is supposedly making in iOS and macOS, leveraging a feature called "exclaves".
* Stig Brautaset [describes YAML's exponent problem][link-15].

## Storage

* Ben Dicken of PlanetScale has a good primer on [IO devices and latency][link-2].

## Virtualization

* William Lam takes readers through the steps necessary to [rename a vSAN virtual machine and its files][link-19].
* I've been thinking recently that I need to give [Incus][link-22] a deeper look. This article on [Incus installation and use (focused on storage pools and networking)][link-21] will likely come in handy.

## Career/Soft Skills

* Ruben Schade explains why [calling a technology "boring" is actually a compliment][link-7].
* Scott Young provides some guidance to know [when focusing on your strengths isn't the best approach][link-12].
* Over the past year or two, I've been experimenting with a "zettelkasten" style of note taking. Accordingly, I've been reading various articles and resources about this approach, what makes it work, and how to use it effectively. Among other articles, this article by Bob Doto [on "bottom up" versus "top down" in note-taking][link-23] was helpful.

That's all for now. I welcome all feedback as long as it is constructive; feel free to reach out and tell me what I could improve! There are a variety of ways to get in touch with me; you can reach me via various social media sites ([Twitter][link-99], [Mastodon][link-30], and [Bluesky][link-29]), via Slack (I frequent [the Kubernetes Slack community][link-28], among others), or via email (my address is on this site and it's not too hard to guess). I'd love to hear from you!

[link-1]: https://malwaretech.com/2025/08/every-reason-why-i-hate-ai.html
[link-2]: https://planetscale.com/blog/io-devices-and-latency
[link-3]: https://julianhofer.eu/blog/03-git-forges/
[link-4]: https://www.theregister.com/2025/03/08/kernel_sanders_apple_rearranges_xnu/
[link-5]: https://www.bleepingcomputer.com/news/security/subaru-starlink-flaw-let-hackers-hijack-cars-in-us-and-canada/
[link-6]: https://www.bitsinflight.com/keeping-public-bgp-routing-in-check-with-irr-and-rpsl/
[link-7]: https://rubenerd.com/boring-tech-is-mature-not-old/
[link-8]: https://muratbuffalo.blogspot.com/2025/03/cedar-new-language-for-expressive-fast.html
[link-9]: https://github.com/cedar-policy/cedar-access-control-for-k8s
[link-10]: https://blog.ipspace.net/2025/01/rfc9637-ipv6-documentation-prefix/
[link-11]: https://forwardingplane.net/post/2024-12-21-new-ipv6-doc-space-rfc9637/
[link-12]: https://www.scotthyoung.com/blog/2025/07/08/strengths/
[link-13]: https://aws.amazon.com/blogs/containers/under-the-hood-amazon-eks-ultra-scale-clusters/
[link-14]: https://securitylabs.datadoghq.com/articles/kubernetes-security-fundamentals-part-7/
[link-15]: https://www.brautaset.org/posts/yaml-exponent-problem.html
[link-16]: https://daniel.haxx.se/blog/2025/01/23/cvss-is-dead-to-us/
[link-17]: https://infisical.com/blog/terraform-modules-organization-scaling
[link-18]: https://nubis.ma/blog/terraform_infrastructure_as_code_essential_tools_for_clean_maintainable_production_environments/
[link-19]: https://williamlam.com/2025/08/quick-tip-how-to-rename-a-vsan-virtual-machine-and-its-files.html
[link-20]: https://bladesmadesimple.com/2025/07/taking-a-look-at-the-newest-blade-server-offerings/
[link-21]: http://serverascode.com//2024/10/19/incus-installation-and-use.html
[link-22]: https://linuxcontainers.org/incus/docs/main/
[link-23]: https://writing.bobdoto.computer/what-do-we-mean-when-we-say-bottom-up/
[link-28]: https://kubernetes.slack.com/
[link-29]: https://bsky.app/profile/scottslowe.bsky.social
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2023-02-17-technology-short-take-165.md" >}}
[xref-2]: {{< relref "2023-05-12-technology-short-take-168.md" >}}
