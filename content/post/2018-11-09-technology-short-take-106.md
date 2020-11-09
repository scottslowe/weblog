---
author: slowe
categories: Information
comments: true
date: 2018-11-09T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Intel
- Linux
- VirtualBox
- AWS
- CLI
- Microsoft
- Fedora
- Kubernetes
- Apple
- Docker
title: 'Technology Short Take 106'
url: /2018/11/09/technology-short-take-106/
---

Welcome to Technology Short Take #106! It's been quite a while (over a month) since the last Tech Short Take, as this one kept getting pushed back. Sorry about that, folks! Hopefully I've still managed to find useful and helpful links to include below. Enjoy!<!--more-->

## Networking

* Julia Evans provides [some Envoy basics][link-26]. [Envoy][link-27] is a useful project with which to be familiar; it serves as the data plane for the [Istio service mesh][link-28], and it is the data plane for Heptio's [Contour ingress controller][link-29] (and, by extension, Heptio's [Gimbal multi-cluster routing solution][link-30]).
* Continuing on that Envoy theme, you may find [this article][link-31] by Matt Klein---one of the primary authors of Envoy---helpful in understanding some of the concepts behind modern load balancing and proxying. Many of these concepts had direct impacts on the design of Envoy.

## Servers/Hardware

* The Intel Management Engine (ME) has received a bit of attention as a potential security vulnerability; in [this article][link-2], authors Maxim Goryachy and Mark Ermolov expose some new concerns around the Intel ME and its undocumented Manufacturing Mode.
* Serve The Home takes [a critical look][link-20] at the Bloomberg Supermicro stories, debunking or at least calling into question many details of the alleged hardware hack as reported by Bloomberg.

## Security

* From an unknown author, we have [this security rant on Flatpak][link-5].
* Google's Project Zero team posted [an update on finding and exploiting Safari bugs using publicly available tools][link-7].
* This page has a list of [ways to exploit (or hack) macOS][link-18]. I share this not to encourage illegal activities, but instead to point out that _no_ operating system is secure, so be alert at all times and use good computing practices regardless of your OS.
* Björn Wenzel shows [how to use HashiCorp Vault to generate short-lived certificates for use with `kubectl`][link-19] for managing/interacting with a Kubernetes cluster. This is a pretty neat idea, but keep in mind you also have the "overhead" of managing a Vault installation (which has its own set of challenges).

## Cloud Computing/Cloud Management

* Ric Harvey shows [how to automatically deploy Hugo updates using AWS CodeBuild][link-1]. This is something I've been considering for my own site, but just haven't taken the time to really dig in. Thanks for (potentially) saving me some time, Ric!
* If you use the AWS CLI but haven't read [this post by Eric Hammond][link-8] yet, you are doing yourself a disservice.
* Maish Saidel-Keesing examines [the design trade-offs for NAT gateways versus AWS PrivateLink][link-9].
* I, for one, was very glad to see [this][link-10]. Non-code contributions are, in my opinion, an important but oft-overlooked aspect of open source communities.
* Clement Pang, co-founder and Chief Architect of Wavefront, [talks about building a highly-available service][link-14].
* Dušan Šušic has a write-up on [using Traefik as a Kubernetes ingress controller][link-15].
* Fellow VMware alum Steve Flanders has a write-up on [running Kubernetes locally][link-17] (on a Mac). Steve also shares what he thinks are [some must-have tools for Kubernetes][link-21].
* Steven Acreman shares a brief, high-level [comparison of the major Kubernetes ingress solutions][link-22].
* I enjoy Michael Hausenblas' "appops reloaded" posts, which are similar to my Tech Short Takes but more tightly focused on cloud-native stuff. He recently hit [post number 100 in the series][link-23]. Good stuff!

## Operating Systems/Applications

* Denzil Ferreira shares some experiences with [the Microsoft Surface Pro 4 and Fedora 28][link-4]. The "TL;DR" is that it works, but there's a fair amount of effort involved to get there.
* [This][link-12] got a fair amount of attention recently.
* Ed Haletky shares [his experience in migrating to a new MacBook Pro][link-13]; perhaps some of the information he shares will be helpful to others.
* Daniele Polencic shares [3 tricks for smaller Docker images][link-16]. The title is a bit misleading; the article is really more about using different base images to optimize for size. Useful nevertheless, though.
* I loved [this set of Bash tips][link-24].
* Andrej Yemelianov has [a quick introduction to managing containers in `runC`][link-25].

## Storage

Nothing this time around, but I'll stay alert for items I can include in the next Technology Short Take.

## Virtualization

* In August of this year (2018), SSD published [this article on a guest-to-host escape found in VirtualBox][link-6]. It looks like the VirtualBox community fixed the issue fairly quickly, but you may want to double-check your version to make sure you aren't vulnerable.

## Career/Soft Skills

* Consider [this][link-3] to be a career skills "anti-pattern," if you will (a list of things not to do or be).
* Nick Shrock provides [some guidelines and advice on performing code reviews][link-11].

OK, that's all I have for now. Question for the readers (we'll see how many of you make it this far)---which is better for you, regular Tech Short Takes that might be shorter _or_ Tech Short Takes about this length (~30-ish links) but less frequently? [Hit me on Twitter and let me know.][link-99] Thanks!

[link-1]: https://digilution.io/posts/ci-cd-pipeline-for-hugo/
[link-2]: http://blog.ptsecurity.com/2018/10/intel-me-manufacturing-mode-macbook.html
[link-3]: https://packetpushers.net/the-most-fragile-engineers-i-know/
[link-4]: https://denzilferreira.github.io/microsoft-surface-pro-4-and-fedora-28/
[link-5]: http://flatkill.org/
[link-6]: https://blogs.securiteam.com/index.php/archives/3736
[link-7]: https://googleprojectzero.blogspot.com/2018/10/365-days-later-finding-and-exploiting.html
[link-8]: https://alestic.com/2013/11/aws-cli-query/
[link-9]: https://technodrone.blogspot.com/2018/10/aws-privatelink-vs-nat-gateway-from.html
[link-10]: https://kubernetes.io/blog/2018/10/04/introducing-the-non-code-contributors-guide/
[link-11]: https://medium.com/@schrockn/on-code-reviews-b1c7c94d868c
[link-12]: https://gist.github.com/a7madgamal/c2ce04dde8520f426005e5ed28da8608
[link-13]: https://www.astroarch.com/2018/10/migration-week-to-an-apple-macbook-pro/
[link-14]: https://medium.com/@clementpang/thoughts-from-the-front-line-operating-at-99-95-uptime-b97d772b5e66
[link-15]: https://medium.com/@dusansusic/traefik-ingress-controller-for-k8s-c1137c9c05c4
[link-16]: https://medium.com/skills-matter/3-simple-tricks-for-smaller-docker-images-cf2760645621
[link-17]: https://sflanders.net/2018/10/24/running-kubernetes-locally/
[link-18]: https://null-byte.wonderhowto.com/how-to/ultimate-guide-hacking-macos-0188749/
[link-19]: https://koudingspawn-blog.firebaseapp.com/combine-vault-with-kubeadm/
[link-20]: https://www.servethehome.com/investigating-implausible-bloomberg-supermicro-stories/
[link-21]: https://sflanders.net/2018/10/26/must-have-kubernetes-tools/
[link-22]: https://kubedex.com/nginx-ingress-vs-kong-vs-traefik-vs-haproxy-vs-voyager-vs-contour-vs-ambassador/
[link-23]: https://tinyletter.com/mhausenblas/letters/appops-reloaded-100-o
[link-24]: https://zwischenzugs.com/2018/10/12/eleven-bash-tips-you-might-want-to-know/
[link-25]: https://blog.selectel.com/managing-containers-runc/
[link-26]: https://jvns.ca/blog/2018/10/27/envoy-basics/
[link-27]: https://www.envoyproxy.io/
[link-28]: https://istio.io/
[link-29]: https://github.com/heptio/contour
[link-30]: https://github.com/heptio/gimbal
[link-31]: https://blog.envoyproxy.io/introduction-to-modern-network-load-balancing-and-proxying-a57f6ff80236
[link-99]: https://twitter.com/scott_lowe
