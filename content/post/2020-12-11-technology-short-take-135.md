---
author: slowe
categories: Information
comments: true
date: 2020-12-11T10:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- macOS
- SSH
- Google
- Kubernetes
- BSD
- Linux
- Go
- Windows
- Docker
title: 'Technology Short Take 135'
url: /2020/12/11/technology-short-take-135/
---

Welcome to Technology Short Take #135! This will likely be the last Technology Short Take of 2020, so it's a tad longer than usual. Sorry about that! You know me---I just want to make sure everyone has plenty of technical content to read during the holidays. And speaking of holidays...whatever holidays you do (or don't) celebrate, I hope that the rest of the year is a good one for you. Now, on to the content!<!--more-->

## Networking

* Arthur Chiao [cracks open `kube-proxy`][link-21], a key part of Kubernetes networking, to expose the internals, and along the way exposes readers to a few different technologies. This is a good read if you're trying to better understand some aspects of Kubernetes networking.
* Gian Paolo [takes a look at using tools like `curl` and `jq`][link-31] when working with networking-related APIs.
* It's not unusual to see "networking professionals need to learn developer tools," but how often do you see "developers need to learn these networking tools"? Martin Heinz discusses that very topic in [this post][link-32].

## Servers/Hardware

* Kay Singh [collects some user comments][link-16] on the new M1-powered Apple hardware.
* Matt Bagnara shares his journey of [building a mechanical keyboard from scratch][link-28]. Lots of geekery in here!

## Security

* [SilentKnight][link-3] is a set of security tools/checks for macOS.
* Jonathan Bowman writes about the new crypto policies in Fedora 33 and whether you'll need [to adjust your SSH keys][link-4].
* This [list of best practices for securing OpenSSH systems][link-5] may be useful.
* As if another reason was needed _not_ to use `curl | bash`, [here you go][link-22].
* Akihiro Suda [admonishes readers not to use host networking][link-26] for their containers.
* Wei Lien Dang [discusses the recent MITM (man in the middle) vulnerability in Kubernetes][link-33] (assigned CVE-2020-8554).

## Cloud Computing/Cloud Management

* Mark Brookfield has a two-part series relating to Ansible AWX and Hashicorp Vault ([part 1][link-11], [part 2][link-12]).
* Mete Atamel has [a great walkthrough of using Workflows on Google Cloud to connect various API-driven services together][link-14]. This seems like an _incredibly_ useful service---I wonder why it's not getting more attention? Perhaps someone more educated than me can provide some context on the downsides of Workflows?
* This is an older article, but hopefully still useful nevertheless: Brian Mathews explains [how to use the `linkerd` sidecar to find and fix application issues on Kubernetes][link-15].
* This looks like [a handy tool][link-23] when working with Kubernetes manifests.
* Sounds like EKS is about to hit the road---to other platforms, that is. Check out [this blog post on the AWS re:Invent announcement][link-24], and take a look at [Weave's blog post on their involvement][link-25].
* Kubernetes 1.20 [has been released][link-29].
* Gaurav Agarwal has a write-up on [understanding Kubernetes multi-container Pod patterns][link-30]. (The sidecar pattern is one such example.)
* Here's a two-part series (so far) on setting up a multi-architecture Kubernetes cluster ([part 1][link-34], [part 2][link-35]).

## Operating Systems/Applications

* Here's [a quick cheatsheet for OpenBSD][link-1]. Handy.
* Dusty Mabe walks readers through [an installation of Fedora CoreOS][link-2].
* Aaron Kili has [a few helpful `sudo` configuration tips][link-8].
* Eric Schabell shows readers what it takes to [install Fedora 33 on a late 2011-era 13" MacBook Pro][link-17].
* Here's a post on [using Windows Subsystem for Linux (WSL) to run Linux containers on Windows][link-20].

## Programming

* Although I do not (yet) consider myself a developer, I found John Arundel's [Rust versus Go article][link-9] to be very informative.
* Ben Kehoe provides readers with [a hygienic Python setup for Linux, macOS, and WSL][link-10].
* The folks at SemaphoreCI shared with me an e-book on CI/CD with Docker and Kubernetes. Although some parts of it do focus on SemaphoreCI (as would be expected), I do believe it may still be a useful resource for some readers. It's available behind a regwall (you have to supply an e-mail address) [here][link-27].

## Virtualization

* Corey Minyard of MontaVista [disusses the role of Linux and virtualization in safety-critical systems][link-18].

## Career/Soft Skills

* Looking for some resources to help you prepare for the AWS Solutions Architect Associate certification exam? [Look no further][link-7]!
* Scott Hanselman lays out [one of the best arguments I've heard][link-13] for maintaining a blog or a wiki. Why keep writing the same thing over and over again in Slack channels, e-mails, or IRC conversations?
* Paul Johnston shares several comparisons in support of [how tech teams should run more like sports teams][link-19].

That's all for this time around. I'll be back in early 2021 with the next Technology Short Take and more interesting---and hopefully useful---technical content. Until then, feel free to hit [me on Twitter][link-99]; I'd love to hear from you!

[link-1]: https://slaanesh.org/2020/11/openbsd-cheatsheet/
[link-2]: https://dustymabe.com/2020/11/18/coreos-install-via-live-iso-copy-network/
[link-3]: https://eclecticlight.co/lockrattler-systhist/
[link-4]: https://dev.to/bowmanjd/upgrade-ssh-client-keys-and-remote-servers-after-fedora-33-s-new-crypto-policy-47ag
[link-5]: https://www.cyberciti.biz/tips/linux-unix-bsd-openssh-server-best-practices.html
[link-6]: https://www.antitree.com/2020/11/pod-security-policies-are-being-deprecated-in-kubernetes/
[link-7]: https://dannys.cloud/aws-solutions-architect-associate-exam-guide
[link-8]: https://www.tecmint.com/sudoers-configurations-for-setting-sudo-in-linux/
[link-9]: https://bitfieldconsulting.com/golang/rust-vs-go
[link-10]: https://read.acloud.guru/my-python-setup-77c57a2fc4b6
[link-11]: https://virtualhobbit.com/2020/07/23/enabling-hashicorp-vault-lookups-in-ansible-awx/
[link-12]: https://virtualhobbit.com/2020/11/11/enabling-hashicorp-vault-lookups-in-ansible-awx-part-2/
[link-13]: https://www.hanselman.com/blog/do-they-deserve-the-gift-of-your-keystrokes
[link-14]: https://atamel.dev/posts/2020/09-08_first_look_at_workflows/
[link-15]: https://medium.com/swlh/efficiently-finding-fixing-issues-on-kubernetes-using-linkerd-2-0-sidecar-8817973a39bc
[link-16]: https://www.singhkays.com/blog/apple-silicon-m1-black-magic/
[link-17]: https://www.schabell.org/2020/11/installing-fedora33-on-macbook-pro-13inch-late-2011.html?m=1
[link-18]: https://www.mvista.com/en/blog/detail/why-we-are-moving-away-from-xen-and-hypervisors-for-safety-episode-1
[link-19]: https://pauldjohnston.medium.com/how-tech-teams-should-be-run-more-like-sports-teams-6a3280322977
[link-20]: https://hackernoon.com/how-to-run-docker-linux-containers-natively-on-windows-ti1i3uxr
[link-21]: https://arthurchiao.art/blog/cracking-k8s-node-proxy/
[link-22]: https://www.idontplaydarts.com/2016/04/detecting-curl-pipe-bash-server-side/
[link-23]: https://github.com/ryane/kfilt
[link-24]: https://aws.amazon.com/eks/eks-distro/
[link-25]: https://www.weave.works/blog/on-prem-kubernetes-gitops-eks-distro
[link-26]: https://medium.com/nttlabs/dont-use-host-network-namespace-f548aeeef575
[link-27]: https://semaphoreci.com/resources/cicd-docker-kubernetes
[link-28]: https://bagnaram.github.io/blog/2020/12/01/keyboard
[link-29]: https://kubernetes.io/blog/2020/12/08/kubernetes-1-20-release-announcement/
[link-30]: https://medium.com/better-programming/understanding-kubernetes-multi-container-pod-patterns-577f74690aee
[link-31]: https://www.ifconfig.it/hugo/2020/12/dnac-api-curl-and-jq/
[link-32]: https://dev.to/martinheinz/networking-tools-every-developer-needs-to-know-4a3n
[link-33]: https://www.stackrox.com/post/2020/12/cve-2020-8554-man-in-the-middle-vulnerability-in-kubernetes-top-recommendations/
[link-34]: https://blog.goeri.de/k8-cluster-part-1/
[link-35]: https://blog.goeri.de/k8-cluster-part-2/
[link-99]: https://twitter.com/scott_lowe
