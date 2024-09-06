---
author: slowe
categories: Information
comments: true
date: 2024-09-06T08:30:00-06:00
tags:
- Automation
- AWS
- Docker
- EKS
- Go
- Hardware
- Kubernetes
- Linux
- macOS
- Microsoft
- Networking
- Security
- Storage
- Virtualization
- Web
- Wireless
title: 'Technology Short Take 182'
url: /2024/09/06/technology-short-take-182/
---

Welcome to Technology Short Take #182! I have a slightly bulkier list of links for you today, bolstered by some recent additions to my RSS feeds and supplemented by some articles I found through social media. There should be enough here to keep folks entertained this weekend---enjoy!<!--more-->

## Networking

* New to network automation? Ivan Pepelnjak [has made publicly available][link-11] the materials from his _Network Automation 101_ course.
* This is an older article from 2020, but still useful: Nathan Taber ["demystifies" cluster networking for Amazon EKS worker nodes][link-12].
* Ales Brelih provides [a reasonably comprehensive introduction to container networking][link-16], covering all the significant concepts involved (network namespaces, veth pairs, bridges, and our good friend `iptables`).
* And here's [another breakdown of network namespaces and container networking][link-33].
* Running your own FreeRADIUS implementation to control Wi-Fi access is overkill for me, but [for Neil it's just another day][link-18].
* Alexis Ducastel provides [some CNI benchmark results][link-32].

## Servers/Hardware

* I thought [this write-up][link-24] of Andy Bechtolsheim's keynote at Hot Interconnects 2024 was an interesting summary of where we could see hardware development go in the next 4 years.
* It turns out that Yubikeys---hardware security keys---are subject to a potential cloning vulnerability, although it does require physical access to the device. Ars Technica has more details [here][link-29]. There's also a more detailed write-up available [here][link-30].

## Security

* The so-called "0.0.0.0 Day" vulnerability apparently has the potential to expose services on local networks; get more details in [this Hacker News article][link-4].
* Oh man, [this][link-21] was something I didn't need to know. I was happier in my ignorance.
* Gabriella Gonzalez shares some tricks for [jailbreaking hosts behind "secure" enterprise firewalls][link-22].
* Microsoft shares some details on [a Chromium zero-day exploit leveraged by a North Korean threat actor][link-34].
* I recently stumbled across AWS Cloud Security Weekly; [here's issue 60][link-35].

## Cloud Computing/Cloud Management

* Vegard Hagen shares [how to use OpenTofu to stand up Talos Kubernetes on Proxmox][link-8].
* Even when I worked at Pulumi, I wasn't a fan of using infrastructure-as-code for defining Kubernetes resources. Instances exist where it can help reduce complexity, but it feels like for many other instances of Kubernetes infrastructure the use of IaC results in a net increase in complexity. I must be looking at this the wrong way, though, because I see a _ton_ of articles on using IaC to define Kubernetes resources---like [this one discussing the use of CDK8s][link-25].

## Operating Systems/Applications

* I started using `eza` on my desktop systems (both macOS and Linux) a while ago, but it's nice to see it [getting more attention][link-1].
* Giacomo Coletto shares some ["quality of life" improvements for Arch Linux][link-2].
* John Woltman has [a trick for quickly extracting archives][link-3] in KDE Plasma.
* A Microsoft update that wasn't supposed to affect Linux is [wreaking havoc with dual-boot Linux systems][link-5].
* Nick Janetakis [reminds folks to use `compose.yaml`][link-7].
* Martin Heinz shares [10 reasons why `curl` is awesome][link-15].
* The author of [this post on exploring desktop Linux][link-17] has apparently had enough with Apple's direction with macOS. I can't say I disagree.
* Oliver Davies explains why he feels [abbreviations are better than aliases][link-19].
* I'm a avid fan of RSS/Atom, so I loved reading [Mark Nottingham's suggestions on what RSS needs][link-20]. Great ideas here! I would love to see an "RSS comeback."
* This is a slightly older article (from 2019), but worthwhile if you're considering [a switch from Bash to Fish][link-31].

## Programming/Development

* I really enjoyed [Julia Evans' breakdown][link-9] of some of the common mistakes from this list of [common Go mistakes][link-10].

## Storage

* This is a great story on [the history of block storage at AWS][link-6]. Well worth a read.
* Kirill Bobrov helps readers [choose the right AWS storage service by comparing S3, S3N, and S3A][link-13].
* Here's [a guide to building a minimal ZFS NAS][link-14].
* [Plausible deniability in storage][link-26]. Cool.

## Career/Soft Skills

* Here are some suggestions for [identifying experts versus imitators][link-23].
* I recently watched a video recording, recently released by the NSA, of a presentation given by Grace Hopper in 1982 ([part one][link-27], [part two][link-28]). Truly, this is a must-watch pair of videos. I was amazed to see and hear Grace Hopper predicting and advocating for "systems of computers" and "systems of software" using "independent modules." Why? What Grace Hopper predicted and advocated for **42 years ago** sound so much like what the industry is using today! I also felt it was interesting to hear her advocating for better security, and talking about problems that we haven't yet solved after 42 years.

That's it for me---I hope that you find something useful among the links I've shared here. As always, you're welcome to reach out to me with feedback, corrections, comments, or suggestions for improvement. Find me [on Twitter][link-99], on [the Fediverse][link-100], via e-mail (my address is on this site and isn't hard to find), or hit me up in one of the Slack communities I haunt. Thanks for reading!

[link-1]: https://www.adamsdesk.com/posts/modernize-command-line-directory-list-eza/
[link-2]: https://giacomo.coletto.io/blog/arch-conf/
[link-3]: https://woltman.com/quick-extract-with-ark/
[link-4]: https://thehackernews.com/2024/08/0000-day-18-year-old-browser.html
[link-5]: https://arstechnica.com/security/2024/08/a-patch-microsoft-spent-2-years-preparing-is-making-a-mess-for-some-linux-users/
[link-6]: https://www.allthingsdistributed.com/2024/08/continuous-reinvention-a-brief-history-of-block-storage-at-aws.html
[link-7]: https://nickjanetakis.com/blog/docker-tip-99-prefer-using-compose-yaml-over-docker-compose-yml
[link-8]: https://blog.stonegarden.dev/articles/2024/08/talos-proxmox-tofu/
[link-9]: https://jvns.ca/blog/2024/08/06/go-structs-copied-on-assignment/
[link-10]: https://100go.co/
[link-11]: https://blog.ipspace.net/2024/06/network-automation-101-videos/
[link-12]: https://aws.amazon.com/blogs/containers/de-mystifying-cluster-networking-for-amazon-eks-worker-nodes/
[link-13]: https://luminousmen.com/post/choosing-the-right-aws-storage-service-a-comprehensive-guide-to-s3-s3n-and-s3a
[link-14]: https://neil.computer/notes/how-to-setup-minimal-zfs-nas-without-truenas/
[link-15]: https://martinheinz.dev/blog/113
[link-16]: https://alesbrelih.dev/posts/2024-07-31-containers-and-network-namespaces/
[link-17]: https://www.rousette.org.uk/archives/exploring-desktop-linux/
[link-18]: https://neilzone.co.uk/2024/08/switching-back-to-self-signed-certs-for-freeradius-wi-fi-authentication/
[link-19]: https://www.oliverdavies.uk/daily/2024/08/24/abbreviations-are-better-than-aliases
[link-20]: https://www.mnot.net/blog/2024/08/25/feeds
[link-21]: https://ian.sh/tsa
[link-22]: https://www.haskellforall.com/2024/08/firewall-rules-not-as-secure-as-you.html
[link-23]: https://fs.blog/experts-vs-imitators/
[link-24]: https://www.nextplatform.com/2024/08/26/bechtolsheim-outlines-scaling-xpu-performance-by-100x-by-2028/
[link-25]: https://dev.to/abhirockzz/write-your-kubernetes-infrastructure-as-go-code-getting-started-with-cdk8s-37n
[link-26]: https://shufflecake.net/
[link-27]: https://www.youtube.com/watch?v=si9iqF5uTFk
[link-28]: https://www.youtube.com/watch?v=AW7ZHpKuqZg
[link-29]: https://arstechnica.com/security/2024/09/yubikeys-are-vulnerable-to-cloning-attacks-thanks-to-newly-discovered-side-channel/
[link-30]: https://ninjalab.io/eucleak/
[link-31]: https://brettterpstra.com/2019/10/11/branching-out-from-bash-fishing-expedition/
[link-32]: https://itnext.io/benchmark-results-of-kubernetes-network-plugins-cni-over-40gbit-s-network-2024-156f085a5e4e
[link-33]: https://medium.com/@alexandre.mahdhaoui_34782/cni-and-network-namespaces-complete-guide-part-1-b174b21e0f7c
[link-34]: https://www.microsoft.com/en-us/security/blog/2024/08/30/north-korean-threat-actor-citrine-sleet-exploiting-chromium-zero-day/
[link-35]: https://aws-cloudsec.com/p/issue-60
[link-99]: https://twitter.com/scott_lowe
[link-100]: https://fosstodon.org/@scottslowe
