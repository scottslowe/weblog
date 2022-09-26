---
author: slowe
categories: Information
comments: true
date: 2022-09-23T09:00:00-06:00
tags:
- AWS
- Azure
- CLI
- GCP
- Hardware
- IaC
- Istio
- Kubernetes
- macOS
- Networking
- OPA
- Packer
- Security
- Storage
- Virtualization
title: 'Technology Short Take 160'
url: /2022/09/23/technology-short-take-160/
---

Welcome to Technology Short Take #160! This time around, my list of links and articles is a tad skewed toward cloud computing/cloud management, but I've still managed to pull together some links on other topics that readers will hopefully find useful. For example, did you know about the secret macOS network quality tool? You didn't? Lucky for you there's a link to an article about it below. Read on to get all the details!<!--more-->

## Networking

* Ivan Pepelnjak [tackles][link-16] the "infrastructure-as-code is scary" mindset. (Related: see the first bullet in the "Career/Soft Skills" section below.)
* Larry Peterson [reflects on the evolution of TCP][link-18].
* Vikas Choudhary [discusses][link-26] Istio's Secure Naming; that is, the name given to services and the Subject Alternative Name (SAN) put on the X.509 certificates used in mTLS.

## Servers/Hardware

* Timothy Prickett Morgan [takes a look at Tesla's "Dojo" supercomputer][link-4].

## Security

* Rory McCune has a round-up of [what's new for security in Kubernetes 1.25][link-19].
* Ars Technica [has details][link-22] on another recent software supply chain attack.
* Mac Chaffee shares some [under-documented Kubernetes security tips][link-29].

## Cloud Computing/Cloud Management

* Not familiar with the AWS CDK? [This post][link-2] should help.
* Barry Browne writes about his [journey to becoming an AWS Solutions Architect][link-8].
* With plenty of warnings that this is---using the author's own words---a "Bad Idea" and an "Extremely Inappropriate Use", Kevin Kutcha guides readers through [building a URL shortener with _only_ AWS Lambda][link-9].
* Need an introduction/overview to Google Cloud Platform? Anthony Heddings has [such a post][link-10], although it is _really_ high-level (perfect for true beginners). What I'd like to find---but haven't yet---is a good, in-depth comparison of fundamental concepts between AWS, Azure, and GCP.
* Maarten Van Driessen takes readers through [using an Azure DevOps pipeline with Packer to build server templates][link-12].
* Jacob Martin recently published [an introduction to Open Policy Agent (OPA)'s Rego language][link-21] and its role in "policy as code."
* Need some help learning serverless? Paul Johnston's [recent post][link-23] might be helpful.
* Nic Cope writes about some of the challenges (and solutions) seen when [scaling Kubernetes to thousands of CRDs][link-25].
* Nicolas Fränkel takes [a quick glance at the Kubernetes Gateway API][link-27].
* Kumar Harsh has [a tutorial on using `kubectl` proxy][link-28].
* Pavan Gudiwada _almost_ convinces me to switch tools for managing Kubernetes contexts based on [this review][link-31], but ultimately I'm sticking with my fork of `ktx` (an old Heptio Labs tool).

## Operating Systems/Applications

* I loved [this article][link-1] by Lawrence Jones about how his organization tested and prepared for the impact of latency on their application. There's some really good stuff in here.
* Phil Stokes shares a few [macOS "power tricks" for security pros][link-3].
* [Devbox][link-6] looks insanely useful. I haven't tried it yet, but do plan to try it out in the very near future.
* Jean Yang [opines][link-13] that traffic-based tools leveraging eBPF are going to shake up the entire developer tools industry.
* I should add [this][link-14] to my standard SSH bastion host OS image. (Also, I loved the story about the origin of the name.)
* Here's [a walkthrough][link-17] on using DNSCrypt on macOS.
* Dan Petrov writes about [the secret macOS network quality tool][link-24].
* [This article][link-30] by Cecelia Martinez almost has me convinced I need to give the GitHub CLI a try (even though that is not the article's primary purpose).

## Programming

* Nathan Peck has a good article that walks readers through both the "how" and the "why" of [using a container for a NodeJS application][link-7] (including deploying it to AWS).
* Daniel Lemire discusses [escaping strings faster with AVX-512][link-11].

## Virtualization

* Drew DeVault speaks out [in praise of QEMU][link-5].
* Cormac Hogan provides a way to perform [distributed switch to standard switch migrations][link-20].
* Niels Hagoort discusses some [vSphere 8 vMotion improvements][link-34].

## Career/Soft Skills

* Lydia Leong observes that [many companies are struggling to fill the cloud skills gap][link-15]. If you're not actively embracing cloud-related (cloud native?) skills, you are missing out on what could be a tremendous opportunity (I think).

That's it for this time around! If you have feedback for me---you want to suggest a new blog I should follow or check, or you have some constructive criticism that will help me make this site better---I'd love to hear from you. Feel free to reach out to [me on Twitter][link-99], or find me in one of a number of different Slack communities (the [Kubernetes][link-32] and [Pulumi][link-33] Slack communities are good choices). Thanks for reading!

[link-1]: https://blog.lawrencejones.dev/latency/
[link-2]: https://bobbyhadz.com/blog/what-is-aws-cdk
[link-3]: https://www.sentinelone.com/blog/15-macos-power-tricks-for-security-pros/
[link-4]: https://www.nextplatform.com/2022/08/23/inside-teslas-innovative-and-homegrown-dojo-ai-supercomputer/
[link-5]: https://drewdevault.com/2022/09/02/2022-09-02-In-praise-of-qemu.html
[link-6]: https://www.jetpack.io/blog/devbox/
[link-7]: https://nathanpeck.com/your-first-nodejs-container-on-aws/
[link-8]: https://mybrokencomputer.net/t/my-journey-to-becoming-an-aws-solutions-architect/169
[link-9]: https://kevinkuchta.com/2018/03/lambda-only-url-shortener/
[link-10]: https://www.howtogeek.com/devops/a-beginners-guide-to-google-cloud-platform-gcp/
[link-11]: https://lemire.me/blog/2022/09/14/escaping-strings-faster-with-avx-512/
[link-12]: https://www.brisk-it.net/automated-template-builds-with-packer/
[link-13]: https://www.akitasoftware.com/blog-posts/every-call-you-make-why-watching-traffic-and-ebpf-is-the-future-of-developer-tools
[link-14]: https://www.cyberciti.biz/hardware/how-to-protects-linux-and-unix-machines-from-accidental-shutdownsreboots-with-molly-guard/
[link-15]: https://cloudpundit.com/2022/09/12/cloud-adoption-will-fail-because-of-the-skills-gap/
[link-16]: https://blog.ipspace.net/2022/09/infrastructure-as-code-sounds-scary.html
[link-17]: https://blog.sweetpproductions.com/post/2022-09-10-improve-your-privacy-with-dnscrypt/
[link-18]: https://systemsapproach.substack.com/p/tcp-the-p-is-for-platform
[link-19]: https://securitylabs.datadoghq.com/articles/whats-new-for-security-in-kubernetes-125/
[link-20]: https://cormachogan.com/2022/09/13/distributed-switch-to-standard-switch-migrations/
[link-21]: https://spacelift.io/blog/open-policy-agent-rego
[link-22]: https://arstechnica.com/information-technology/2022/09/breach-of-software-maker-used-to-backdoor-as-many-as-200000-servers/
[link-23]: https://pauldjohnston.medium.com/learning-serverless-and-why-it-is-hard-4a53b390c63d
[link-24]: https://danpetrov.xyz/macos/2021/11/14/analysing-network-quality-macos.html
[link-25]: https://blog.upbound.io/scaling-kubernetes-to-thousands-of-crds/
[link-26]: https://vikaschoudhary16.com/2022/05/26/secure-naming-in-istio/
[link-27]: https://blog.frankel.ch/kubernetes-gateway-api/
[link-28]: https://www.containiq.com/post/kubectl-proxy
[link-29]: https://www.macchaffee.com/blog/2022/k8s-under-documented-security-tips/
[link-30]: https://dev.to/ceceliacreates/mac-web-developer-setup-terminal-zsh-git-node-vs-code-homebrew-and-github-cli-1p5b
[link-31]: https://home.robusta.dev/blog/switching-kubernets-context/
[link-32]: https://kubernetes.slack.com
[link-33]: https://pulumi-community.slack.com
[link-34]: https://nielshagoort.com/2022/09/20/vsphere-8-vmotion-improvements/
[link-99]: https://twitter.com/scott_lowe
