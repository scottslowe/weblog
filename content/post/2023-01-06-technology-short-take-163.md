---
author: slowe
categories: Information
comments: true
date: 2023-01-06T06:30:00-06:00
tags:
- AWS
- ESXi
- Hardware
- Kubernetes
- Linux
- macOS
- Microsoft
- Networking
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- VMware
- vSphere
- Windows
title: 'Technology Short Take 163'
url: /2023/01/06/technology-short-take-163/
---

Welcome to Technology Short Take #163, the first of 2023! If you're new to this site, the Technology Short Takes are essentially "link lists"---I collect links and articles about various technologies and I share them about every 3-4 weeks (sometimes more frequently). I'll often add a bit of commentary here and there, but the real focus is the information in the linked articles. But enough of this, let's get on with it! Here's hoping you find something useful here.<!--more-->

## Networking

* Yuchen Wu and Andrew Hauck [share the story of Pingora][link-2], a Rust-based HTTP proxy developed by and currently in use at Cloudflare.
* Ivan Velichko has been publishing some great articles on container-focused networking concepts; the latest (that I'd seen as of the writing of this article) is this one on [publishing the port of a running container][link-3].
* Numerous Networks has some information on [optimizing Wi-Fi 6E networks for Apple devices][link-24].

## Servers/Hardware

* Back during the AWS re:Invent 2022 timeframe, I came across [this newsletter][link-1] focused on AWS custom chips (Graviton, Trainium, Inferencia). If staying up-to-date with this topic is important for your role, then subscribing is probably a good idea. (I did.)
* I enjoyed [this story][link-22] on the mass extinction of UNIX workstations and the trials and travails of trying to run your own UNIX workstation.

## Security

* This could be bad---a wormable vulnerability that could allow attackers to remotely execute code by exploiting potentially _any_ Windows application protocol that provides authentication, including (potentially) SMTP or HTTP. Ouch. Get more details in [this article][link-14].
* Nigel Douglas shows [how to mitigate DoS (Denial of Service) attacks in Kubernetes][link-16]. The article title led me to believe that both Falco and Calico would be used; although they are both discussed (Falco for detection and Calico for prevention), the bulk of the work falls to Calico.
* Now that a fix has been supplied, Microsoft [publicly discusses Achilles][link-18], a vulnerability they discovered in the macOS Gatekeeper security mechanism.
* Let's hope [this][link-19] doesn't turn out to be a significant issue for folks.

## Cloud Computing/Cloud Management

* Using Visual Studio Code's Remote Containers extension is a handy way to keep your development environment contained---no pun intended!---away from the host system. This includes using the functionality when working with infrastructure as code: here's a walkthrough on [doing it with Terraform][link-4], and [an equivalent version for Pulumi][link-5].
* Here's [a "lightweight dockerized" AWS CLI container][link-6].
* I stumbled across [this site][link-11] by Michael Friedrich; looks like there is also a monthly newsletter.
* If you're looking to expand your serverless knowledge, [this site][link-12] may have some useful resources.
* The idea of [a "cloud-oriented programming" language][link-15] is interesting. I'll be watching closely to see how things play out.

## Operating Systems/Applications

* I already knew about (and use) many of the Rust CLI tools in [this article][link-10], but I still found a couple new ones I hadn't seen before.
* Here's [a list of advanced macOS command-line tools][link-17]. You may know about most of these already, although I learned about the `sips` command when I read this article.
* With all the turmoil going on at Twitter, Mastodon has garnered the attention of many folks, especially considering it can be self-hosted. Andreas Wittig has a write-up on [hosting your own Mastodon instance on AWS][link-20], while Micah Walter takes readers through [how much it costs to host Mastodon on AWS][link-21]. If you're thinking of running your own Mastodon instance, these articles are likely well worth your time to read.
* Here's a guide to [setting up Nix on macOS without Homebrew][link-23].

## Virtualization

* Daniël Zuthof writes about [using ESXi 8 on the Maxtang NUC][link-7].
* Sherif Alghali has an article that discusses [common issues when migrating VMs from on-premises VMware vSphere environments to Azure][link-8].

## Career/Soft Skills

* Dewan Ahmed writes about why developer advocacy is a technical role and is not an entry-level role. While I can't argue with any of his points, his article is definitely not helping my imposter syndrome (many of you probably know [I recently transitioned into DevRel when I moved to Pulumi][xref-1]).
* I know quite a few folks are exploring Mastodon (I have an account on the Fosstodon instance), and this article shows [how to post automatically from an RSS feed][link-9]. This is something I'll be exploring to share articles from this site on Mastodon.
* Also on the topic of Mastodon: here's a tip for [verifying your GitHub profile on Mastodon][link-13].

That's all for now! As always, I _love_ to hear from readers, so please feel free to engage with me through a variety of channels: you can engage with [me on Twitter][link-99], I'm also [on Mastodon][link-30] (via the excellent Fosstodon instance), and you can find me in a variety of Slack communities (the [Kubernetes][link-28] and [Pulumi][link-29] communities are great examples). Thanks for reading!

[link-1]: https://awsgravitonweekly.com/
[link-2]: https://blog.cloudflare.com/how-we-built-pingora-the-proxy-that-connects-cloudflare-to-the-internet/
[link-3]: https://iximiuz.com/en/posts/docker-publish-port-of-running-container/
[link-4]: https://medium.com/geekculture/working-with-terraform-and-aws-within-a-container-on-visual-studio-code-8f6f1a0f2a2a
[link-5]: https://www.pulumi.com/blog/vscode-devcontainers/
[link-6]: https://github.com/richarvey/aws-docker-toolkit
[link-7]: https://blog.zuthof.nl/2022/12/12/esxi-8-on-the-maxtang-nuc/
[link-8]: https://sherifalghali.com/2022/12/09/common-issues-when-migrating-vms-from-vmware-vsphere-on-prem-to-azure/
[link-9]: https://lukas.io/autoposting-rss-to-mastodon
[link-10]: https://dev.to/deepu105/rust-easy-modern-cross-platform-command-line-tools-to-supercharge-your-terminal-4dd3
[link-11]: https://opsindev.news
[link-12]: https://serverlessland.com
[link-13]: https://til.simonwillison.net/mastodon/verifying-github-on-mastodon
[link-14]: https://securityintelligence.com/posts/critical-remote-code-execution-vulnerability-spnego-extended-negotiation-security-mechanism/
[link-15]: https://medium.com/@hackingonstuff/cloud-why-so-difficult-%EF%B8%8F-4e9ef1446a64
[link-16]: https://sysdig.com/blog/denial-of-service-kubernetes-calico-falco/
[link-17]: https://saurabhs.org/advanced-macos-commands
[link-18]: https://www.microsoft.com/en-us/security/blog/2022/12/19/gatekeepers-achilles-heel-unearthing-a-macos-vulnerability/
[link-19]: https://www.bleepingcomputer.com/news/security/oktas-source-code-stolen-after-github-repositories-hacked/
[link-20]: https://cloudonaut.io/mastodon-on-aws/
[link-21]: https://www.micahwalter.com/how-much-ive-spent-so-far-running-my-own-mastodon-server-on-aws/
[link-22]: https://www.osnews.com/story/135605/the-mass-extinction-of-unix-workstations/
[link-23]: https://wickedchicken.github.io/post/macos-nix-setup/
[link-24]: https://www.numerousnetworks.co.uk/noversight/optimising-wi-fi-6e-networks-for-apple-devices/
[link-28]: https://kubernetes.slack.com
[link-29]: https://pulumi-community.slack.com
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2022-08-15-jumping-off-cliffs.md" >}}
