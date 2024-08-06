---
author: slowe
categories: Information
comments: true
date: 2024-02-23T10:00:00-06:00
tags:
- AWS
- BSD
- Cilium
- CLI
- Cloud
- Encryption
- GitHub
- Hardware
- Kubernetes
- Linux
- Networking
- Security
- Storage
- Tetragon
- Virtualization
title: 'Technology Short Take 175'
url: /2024/02/23/technology-short-take-175/
---

Welcome to Technology Short Take #175! Here's your weekend reading---a collection of links and articles from around the internet on a variety of data center- and cloud-related topics. I hope you find something useful here!<!--more-->

## Networking

* The good folks over at Packet Pushers have compiled [a list of open source networking projects][link-14].

## Security

* I attended a local meetup here in the Denver metro area a short while ago and was introduced to [`sops`][link-3].
* AMD processors have been discovered to have multiple security flaws; more details available [here][link-10].
* The Linux kernel project has become a CVE Numbering Authority (CNA); Greg Kroah-Hartman wrote [a blog post that discusses this in more depth][link-11].

## Cloud Computing/Cloud Management

* Josh Biggley shows [how to deploy Tetragon with Cribl Edge][link-1]. The blog post is a bit heavy on the Cribl marketing, but I suppose that is to be expected (it's _extremely_ common with most vendor blogs).
* Jack Lindamood's list of [infrastructure decisions he endorses or regrets][link-2] provides some valuable insight into his personal experience with a variety of technologies and processes. Well worth reading, in my opinion. (Hat tip to Simon Wardley for sharing this on Twitter.)
* Ivan Yurochko of PerfectScale discusses [how to manage S3 throttling][link-5].
* [This post][link-6] is an interesting look "inside" the CNCF Technical Oversight Committee (TOC), with a view on some of the challenges facing the CNCF and its related projects.
* Tyler Treat argues that it's possible---preferable, perhaps---to do [cloud without Kubernetes][link-7].
* Rory McCune reviews his [final Kubernetes census][link-12].
* The Open Constructs Foundation [recently launched][link-16] a "community-driven CDK construct library initiative," which seeks to provide a way for the CDK community to build and share CDK constructs.
* Michael Levan insists that [cloud-native is in shambles][link-17]. I think the article title is a bit click-baity, but the key point in the article---focusing on the expected outcome---is spot on.
* Tony Norlin discusses [running Kubernetes with Cilium on FreeBSD][link-18].
* This is an older post (but still useful, I think, given the review of the code that implements the functionality) on [Kubernetes leader election][link-19].

## Operating Systems/Applications

* Google has open sourced Magicka, an AI-powered file type identification library. More details are available in [this blog post][link-9].
* Andy Ibanez has [a pretty thorough tutorial for `rclone`][link-15] (which, if you aren't aware, is an extraordinarily useful utility).

## Programming/Development

* Although it gets a bit deep into Rego, [this article][link-4] by Jasper Van der Jeugt of Snyk explains automatic source code location for violations---pinpointing the file, line, and column where policy violations occur.
* Josh Collinsworth weighs in regarding LLMs and generative AI in [his essay regarding GitHub Copilot][link-13]. The experiences Josh describes with Copilot are not unique to Copilot; I've experienced the same with other LLM-based generative AI tools. The key takeaway (for me) is that generative AI doesn't make things _more_ accessible; it's actually the opposite, because you need to know enough to know whether or not the generative AI tool is actually accurate or not.

## Virtualization

* While certainly not unique to virtualization, I think it's fair to say that virtualization has had a pretty significant impact on home labs. Sean Massey takes a moment to provide [an update on his latest home lab update][link-8].

That's all I have for you this time around. I love to hear from readers, so if you have feedback on this post (or any post!) on my site, please feel free to reach out. You can find [me on Twitter][link-99], [on the Fediverse][link-30], or in a number of different Slack communities. My e-mail address is also on this site and isn't too hard to find...feel free to drop me a line!

[link-1]: https://cribl.io/blog/taming-tetragon-with-cribl-cloud/
[link-2]: https://cep.dev/posts/every-infrastructure-decision-i-endorse-or-regret-after-4-years-running-infrastructure-at-a-startup/
[link-3]: https://github.com/getsops/sops
[link-4]: https://snyk.io/blog/automatic-source-locations-rego/
[link-5]: https://www.perfectscale.io/blog/aws-s3-throttling
[link-6]: https://codeengineered.com/blog/2024/retro-cncf-toc/
[link-7]: https://blog.realkinetic.com/cloud-without-kubernetes-d0487a4ab345
[link-8]: https://thevirtualhorizon.com/2024/02/16/the-home-lab-update-2024/
[link-9]: https://opensource.googleblog.com/2024/02/magika-ai-powered-fast-and-efficient-file-type-identification.html
[link-10]: https://securityonline.info/high-alert-amd-processors-hit-by-multiple-security-flaws/
[link-11]: http://www.kroah.com/log/blog/2024/02/13/linux-is-a-cna/
[link-12]: https://raesene.github.io/blog/2024/02/17/a-final-kubernetes-censys/
[link-13]: https://joshcollinsworth.com/blog/copilot
[link-14]: https://packetpushers.net/blog/open-source-networking-projects/
[link-15]: https://www.andyibanez.com/posts/rclone-basics-encryption/
[link-16]: https://www.open-constructs.org/
[link-17]: https://dev.to/thenjdevopsguy/cloud-native-is-in-shambles-1klf
[link-18]: https://medium.com/@norlin.t/kubernetes-on-freebsd-with-linux-worker-nodes-and-cilium-a87c50daef03
[link-19]: https://medium.com/michaelbi-22303/deep-dive-into-kubernetes-simple-leader-election-3712a8be3a99
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
