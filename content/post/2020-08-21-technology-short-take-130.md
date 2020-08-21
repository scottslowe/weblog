---
author: slowe
categories: Information
comments: true
date: 2020-08-21T09:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VPN
- Automation
- Fedora
- Linux
- Kubernetes
- JSON
- Windows
- AWS
- Docker
- Git
- CLI
title: 'Technology Short Take 130'
url: /2020/08/21/technology-short-take-130/
---

Welcome to Technology Short Take #130! I've had this blog post sitting in my Drafts folder waiting to be published for almost a month, and I kept forgetting to actually make it live. Sorry! So, here it is---better late than never, right?<!--more-->

## Networking

* Here's [a walkthrough on using DNS over TLS on Fedora][link-8].
* Some Twitter followers recently pointed out [`gnmic`, a gNMI CLI client][link-9]. gNMI, by the way, stands for gRPC Network Management Interface (more information on gNMI can be found [here][link-10]). I haven't used `gnmic`, but it certainly looks like an extremely useful tool.

## Security

* Everyone's favorite three-letter agency recently released some VPN security guidance, as reported by [this InfoSecurity Magazine article][link-1]. The article also provides some links to said guidance, for those that are interested.
* Matt Hamilton takes [a closer look at a recent security vulnerability in the Harbor container registry][link-5]. This vulnerability, known as a Server-Side Request Forgery, allowed Harbor project owners to scan the TCP ports of hosts on the Harbor server's internal network.
* Ivan Pepelnjak [discusses the security implications of so-called "Smart NICs."][link-15]
* Want to use a Yubikey to automatically lock and unlock your Linux system? See [here][link-26].

## Cloud Computing/Cloud Management

* Dusty Mabe discusses K9s, a "terminal UI to interact with your Kubernetes clusters," in [this blog post][link-4].
* Steven Wade shares a cautionary tale about [how SSO brought a Kubernetes cluster to its knees][link-11]. Fortunately, Steven also shares the mitigation steps that he and his team took, so that others might be able to avoid the same fate.
* Jeff Bryner shares his experience in [using the AWS CDK for deploying Docker containers][link-17].
* Laszlo Fogas [talks a bit about Kubernetes requests and limits][link-18].
* Mike Krieger [shares some information][link-19] on how rt.live (a site sharing updates of the COVID-19 R_t_) handles their daily runs to update data.

## Operating Systems/Applications

* I recently came across the `jc` utility, which converts "ordinary" command-line output from a number of different utilities into structured JSON output. Read about [why the author created `jc` in this blog post][link-2].
* If you're new to the GNOME desktop environment, Ori Alvarez's article on [how to create a GNOME desktop entry][link-6] may be useful.
* [According to The Verge][link-7], Windows 10 users aren't very happy about Microsoft's forced roll-out of its Chromium-based Edge browser.
* Justin Garrison shared [some shell functions][link-12] for making it easier to switch AWS CLI profiles, or set AWS region (for example). They're written for `zsh`, but should be adaptable to other shells without unreasonable effort.
* James Pulec [walks readers through Git Worktrees][link-22], the "best Git feature you've never heard of." Indeed!
* I missed [the announcement][link-23] about the release of the Debian 10 "Buster" handbook.
* Via Ivan Pepelnjak (who in turn got it from Julia Evans), I learned about [`entr`][link-24], a Linux CLI tool to run arbitrary commands when files change. Handy.
* [This site][link-25] has more details about the X11 Window system than most people care to know.

## Programming

* Gergely Orosz shares [some data structures and algorithms he actually used][link-20] at a few different tech companies.
* I also learned that the Go language server (`gopls`, used by Visual Studio Code and many other editors for Go language awareness) doesn't work properly when `go.mod` isn't in the root directory of whatever you've opened (see [here][link-21]). The workaround is to use a "multi-root workspace."

## Storage

* Chris Evans [examines how persistent memory in the data center][link-13] plays out with existing storage technologies.

## Virtualization

* With the announcement of a new version of macOS come new beta builds, and articles about running those beta builds in a VM. [Here's the latest][link-3]. It'll be interesting to see how virtualization continues (or maybe doesn't) when Apple moves to its own custom ARM processors.
* William Lam talks about [options for evaluating vSphere with Kubernetes][link-14].

## Career/Soft Skills

* A co-worker (thanks Joe!) pointed out [this article on the concept of inversion][link-16]. I found the article quite interesting, and it has already affected my thinking and how I am approaching/will approach certain projects that I'd like to tackle.

And that's a wrap! Feel free to contact [me on Twitter][link-99] if you have any questions or comments (constructive feedback is always welcome). Thanks for reading!

[link-1]: https://www.infosecurity-magazine.com/news/nsa-issues-vpn-security-guidance/
[link-2]: https://blog.kellybrazil.com/2019/11/26/bringing-the-unix-philosophy-to-the-21st-century/
[link-3]: https://jgandrews.com/posts/macos-vmware/
[link-4]: https://dustymabe.com/2020/07/18/the-k9s-tui-for-kubernetes/
[link-5]: https://www.soluble.ai/blog/harbor-ssrf-cve-2020-13788
[link-6]: https://commithub.com/how-to-create-gnome-desktop-entry
[link-7]: https://www.theverge.com/21310611/microsoft-edge-browser-forced-update-chromium-editorial
[link-8]: https://fedoramagazine.org/use-dns-over-tls/
[link-9]: https://gnmic.kmrd.dev/
[link-10]: https://github.com/openconfig/gnmi
[link-11]: https://medium.com/@swade1987/how-sso-bought-a-cluster-to-its-knees-e0e002bff08
[link-12]: https://gitlab.com/jgarr/dotfiles/-/blob/main/zdotdir/zsh.d/aws.zsh
[link-13]: https://www.architecting.it/blog/persistent-memory-data-centre/
[link-14]: https://www.virtuallyghetto.com/2020/07/is-vsphere-with-kubernetes-available-for-evaluation.html
[link-15]: https://blog.ipspace.net/2020/06/smart-nic-security.html
[link-16]: https://www.anup.io/2020/07/20/invert-always-invert/
[link-17]: https://blog.jeffbryner.com/2020/07/20/aws-cdk-docker-explorations.html
[link-18]: https://gimlet.io/blog/the-cluster-admin-struggle-and-ways-to-keep-kubernetes-resource-requests-and-limits-in-check/
[link-19]: https://medium.com/@mikekrieger/automating-daily-runs-for-rt-lives-covid-19-data-dcda26ed2e2e
[link-20]: https://blog.pragmaticengineer.com/data-structures-and-algorithms-i-actually-used-day-to-day/
[link-21]: https://github.com/golang/go/issues/32394
[link-22]: https://levelup.gitconnected.com/git-worktrees-the-best-git-feature-youve-never-heard-of-9cd21df67baf
[link-23]: https://debian-handbook.info/2020/debian-handbook-for-debian-10-buster-now-live/
[link-24]: http://eradman.com/entrproject/
[link-25]: https://magcius.github.io/xplain/article/
[link-26]: https://kliu.io/post/yubico-magic-unlock/
[link-99]: https://twitter.com/scott_lowe
