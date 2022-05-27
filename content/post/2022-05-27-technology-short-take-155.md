---
author: slowe
categories: Information
comments: true
date: 2022-05-27T08:45:00-06:00
tags:
- AWS
- Kubernetes
- Go
- Hardware
- JSON
- Linux
- macOS
- Networking
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- WireGuard
- YAML
title: 'Technology Short Take 155'
url: /2022/05/27/technology-short-take-155/
---

Welcome to Technology Short Take #155, just in time for the 2022 Memorial Day holiday weekend! (Here in the US, at least.) I mean, don't you want to spend this weekend catching up on some technology-related articles instead of cooking on the grill and gathering with friends and family? I certainly hope not! Still, for those who need a little technology fix over the weekend, hopefully I've included something useful in the list of articles below. Enjoy!<!--more-->

## Networking

* Isovalent---the company behind the Cilium project---has been talking a lot about how the use of eBPF will transform things, including the architecture of a service mesh. Along those lines, one of their latest articles discusses [how to achieve identity-based mutual authentication leveraging eBPF][link-13]. If I'm understanding the article correctly (and feel free to correct me if I am mistaken) it looks as if Cilium Service Mesh will leverage/does leverage a combination of certificate-based mTLS for identity at the workload level and node-based transport encryption (via WireGuard) for data confidentiality. Even though I know that the underlying mechanisms are different, subjectively this _feels_ a lot like using tunnels to connect workloads on different compute nodes (i.e., network virtualization). Is the relationship between network virtualization and service mesh closer than some folks might wish to admit?

## Servers/Hardware

* Researchers have uncovered a potential security flaw in Apple Silicon CPUs; more details in [this 9to5Mac article][link-2]. I'm not sure how I feel about security researchers calling this flaw "not that bad."
* Colin Percival shares some benchmarks using [FreeBSD on the Graviton 3][link-26].

## Security

* An earlier security vulnerability that exposed APC Smart-UPS devices (see details [here][link-18]---attackers could even _destroy_ the UPS!) now has [a follow-up exploit][link-3] that exposes Aruba and Avaya network switches to remote code execution. In both cases, it's a problem with the TLS implementation in a library called NanoSSL.
* Steven J. Vaughan-Nichols writes about [the first malware discovered running on AWS Lambda][link-10].
* Attacks that [can affect iPhones when they're turned off][link-20]? Yikes. Fortunately, such an attack vector isn't very straightforward.
* [Via Teri Radichel][link-27], I saw this article from Google Project Zero about [zero-click security vulnerabilities in Zoom][link-28]. Fortunately, it looks like these vulnerabilities have been patched, so be sure to update your Zoom client. I know it's a pain, but it's one of those things you just need to do.

## Cloud Computing/Cloud Management

* Kubernetes 1.24 marks the first release of the open source container orchestration platform that is signed using Sigstore (more details [here][link-6]). As I understand it, this is the culmination of an effort launched about a year ago [when Google started signing the "distroless" images][link-7].
* Kat Cosgrove provides [some historical context][link-5] for the removal of dockershim in Kubernetes 1.24.
* Although a bit older (from June 2021), this article on [unknown values in Terraform][link-9] feels like a direct response to the rise of tools leveraging general purpose programming languages for declarative infrastructure management (think tools like Pulumi, AWS CDK, etc.).
* Manoj Bhagwat provides [a high-level overview of Kiali and how to install it][link-17].
* Lee Briggs [talks about the YAML][link-16] (he's discussing the recent addition of YAML support in Pulumi).
* Viktor van den Berg shares his [CKAD exam experience and some tips on how to prepare][link-15].
* Here's [a user's perspective][link-14] on using `infracost` to bring cloud spend into your Terraform worfklows.

## Operating Systems/Applications

* The new utility `zq` claims to be [an easier and faster alternative to `jq`][link-1].

## Programming

* [This article][link-4] helped me better understand the relationship(s) between SLSA and SBOMs.
* Bashayr Alabdullah provides an example of [building your own admission controllers in Kubernetes using Go][link-8].
* John Breen's article on [patterns with promises and asynchronous programming in JavaScript][link-22] provides some practical advice on understanding these concepts. (Hat tip to Corey Quinn for sharing this article via Twitter.)
* Alex Edwards' post on [Golang interfaces explained][link-24] was helpful.

## Storage

* For relevant storage news, I'd recommend having a look at J Metz' [Storage Short Take 42][link-23].

## Virtualization

* The state of virtualization on Apple Silicon hardware has seen a few developments in recent days and weeks. One project that caught my attention was [Tart][link-19], a CLI-driven tool that leverages the virtualization support present in macOS to run virtual macOS instances. This will become even more useful, in my opinion, when Linux support is added. (The possibility of a Vagrant provider just seals the deal, in my opinion.)
* The world of virtualization---nay, more than just virtualization---will be forever changed with [the announcement of Broadcom's acquisition of VMware][link-29].

## Career/Soft Skills

* Mike McQuaid shares some details on [how he gets things done][link-11].
* Ashley Janssen discusses [how time-blocking may help improve productivity][link-12]. This is not a technique I've generally used, so I'd be curious to hear from readers who may have used or are currently using techniques like this.
* Here's [some advice][link-21] on "starting your diagramming career" (let's be real, many IT folk need to create diagrams on a regular basis, so tips on creating diagrams might prove really useful).

## Other

* I've seen a _lot_ of work-from-home desk setups, but [this one][link-25] stood out as actually having some budget-conscious selections (which is often not the case). The use of 3M Command strips to affix stuff under the desk is also so blindingly obvious that I'm surprised I hadn't thought of it already.

It's time to wrap up. I hope you all have a wonderful weekend! Feel free to reach out to me if you have questions, comments, suggestions for improvement, or if you just want to say hi. The easiest way to contact me is [via Twitter][link-99]. Thanks for reading!

[link-1]: https://www.brimdata.io/blog/introducing-zq/
[link-2]: https://9to5mac.com/2022/05/02/augury-apple-silicon-vulnerability/
[link-3]: https://www.helpnetsecurity.com/2022/05/03/tlstorm-2-0/
[link-4]: https://slsa.dev/blog/2022/05/slsa-sbom
[link-5]: https://kubernetes.io/blog/2022/05/03/dockershim-historical-context/
[link-6]: https://blog.sigstore.dev/kubernetes-signals-massive-adoption-of-sigstore-for-protecting-open-source-ecosystem-73a6757da73
[link-7]: https://security.googleblog.com/2021/05/making-internet-more-secure-one-signed.html
[link-8]: https://bshayr29.medium.com/build-your-own-admission-controllers-in-kubernetes-using-go-bef8ba38d595
[link-9]: https://log.martinatkins.me/2021/06/14/terraform-plan-unknown-values/
[link-10]: https://thenewstack.io/first-malware-running-on-aws-lambda-discovered/
[link-11]: https://mikemcquaid.com/2021/07/21/how-i-get-things-done/
[link-12]: https://ashleyjanssen.com/time-blocking-and-imagining-your-ideal-week/
[link-13]: https://isovalent.com/blog/post/2022-05-03-servicemesh-security
[link-14]: https://prefetch.net/blog/2022/05/01/understanding-cloud-spend-in-your-terraform-workflows/
[link-15]: https://www.viktorious.nl/2022/05/09/ckad-exam-experience-and-some-tips-on-how-to-prepare/
[link-16]: https://leebriggs.co.uk/blog/2022/05/09/learning-to-love-yaml
[link-17]: https://manoj-bhagwat60.medium.com/kiali-manage-visualize-validate-and-troubleshoot-your-service-mesh-5f9c68ea8ced
[link-18]: https://www.helpnetsecurity.com/2022/03/08/ups-devices-vulnerabilities/
[link-19]: https://github.com/cirruslabs/tart
[link-20]: https://threatpost.com/iphones-attack-turned-off/179641/
[link-21]: https://www.readysetcloud.io/blog/allen.helton/how-to-build-your-first-architecture-diagram/
[link-22]: https://breen.tech/post/but-await-theres-more/
[link-23]: https://jmetz.com/2022/05/storage-short-take-42/
[link-24]: https://www.alexedwards.net/blog/interfaces-explained
[link-25]: https://bdarfler.medium.com/the-best-starter-desk-setup-for-2022-7586cf603581
[link-26]: http://www.daemonology.net/blog/2022-05-23-FreeBSD-Graviton-3.html
[link-27]: https://medium.com/cloud-security/zoom-rce-time-to-patch-f9ad802e0e5e
[link-28]: https://googleprojectzero.blogspot.com/2022/01/zooming-in-on-zero-click-exploits.html
[link-29]: https://www.theverge.com/2022/5/26/23142653/broadcom-vmware-acquisition-deal-software
[link-99]: https://twitter.com/scott_lowe
