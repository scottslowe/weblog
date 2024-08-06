---
author: slowe
categories: Information
comments: true
date: 2024-07-26T08:00:00-06:00
tags:
- Apple
- AWS
- EKS
- Git
- Hardware
- Intel
- Kubernetes
- Linux
- Markdown
- Networking
- PGP
- Security
- Storage
- Talos
- Ubuntu
- Virtualization
- VLAN
- Wireless
title: 'Technology Short Take 180'
url: /2024/07/26/technology-short-take-180/
---

Welcome to Technology Short Take #180! It's hard to believe that July is almost over, and that 2024 is flying past us. It's probably time that you, my readers, took some time to slow down and read more technical blogs. To help with that, I just happen to have a little collection of links to share. Enjoy!<!--more-->

## Networking

* Read [this article][link-8] to better understand why native VLANs exist.

## Servers/Hardware

* This article is a cool story recapping [the history of Intel's Itanium processors][link-6].
* The computer case with the built-in display described in [this post by William Lam][link-17] is pretty cool.

## Security

* This is a slightly older post, but still relevant and useful in my opinion: Dan Lorenc attempts to [help readers understand if you should sign Git commits][link-1].
* A colleague recently introduced me to the idea of [data bouncing][link-3]. It's a super-interesting technique, and it's not clear to me---although I am most definitely not a security expert---how one would go about defending against this.
* Nate Nelson discusses some of [the security implications of Apple's Wi-Fi Positioning System][link-7].
* Nick Frichette examines the security implications of [connection tracking in AWS security groups][link-10].
* Joe Leon, writing for Truffle Security, examines [how anyone can access deleted and private repository data on GitHub][link-21].

## Cloud Computing/Cloud Management

* I've had [this article][link-2] in my "to read" list for quite a long time. I think I avoided reading it because I'd convinced myself that serverless/AWS Lambda was "too hard" for an infrastructure person such as myself, but I'm glad I read it because it helped clarify some things about serverless/AWS Lambda. Maybe it will help you, too?
* In [the last Technology Short Take][xref-1], I shared a link to setting up Cilium with `kube-proxy` replacement on Amazon EKS. This time around, here's [a guide on setting up Cilium with `kube-proxy` replacement with GKE][link-9].
* In the event you need another look at the basics, here's [a guide on setting up Kubernetes on Ubuntu 22.04][link-12].
* Patrick Marie has a (slightly older) post on [enabling Cilium's ingress controller along with Hubble][link-14].
* Suraj Remanan takes readers through a journey of [automating Talos Linux on Proxmox using Packer and Terraform][link-15], integrating Cilium and Longhorn along the way.
* Via Suraj's post, I learned of `talhelper`, a tool for declaratively generating Talos configuration files. It even integrates with `sops`! You can get more details on `talhelper` [via its website][link-16].
* Luc van Donkersgoed [wants to be an AWS cloud engineer again][link-18]. (Luc is not the only one for whom AWS' generative AI obsession is proving worrisome.)
* Here's everything you [ever wanted to know about Istio, from A to Y][link-19]. (Why not Z? I don't know. You'll have to ask Quentin.)

## Operating Systems/Applications

* Major Hayden shows [how to redirect local ports with firewalld][link-5].
* [This recent post][link-11] by John Maxwell introduced me to `typst`. I was already very familiar with Pandoc (it's been a part of my workflow for several years now), but producing PDFs from Markdown via Pandoc was less than ideal. Typst seems to work really well, but I'll have to spend some time creating a Typst template that works well for my specific use cases. Thanks for the post, John!
* Nick Janetakis shows off [how to check if a string contains a substring in Bash][link-13].
* Daniel Wayne Armstrong takes a closer look at [setting charging thresholds on Linux laptops][link-20].

## Virtualization

* William Lam [provides a reader-supplied fix][link-4] for selecting multi-gigabit speeds when using Intel network adapters with ESXi 8.x.

And that's a wrap for this Technology Short Take. Short and sweet, as the saying goes! I hope you found something useful here. In the event you'd like to leave some feedback---on this post, or regarding the site in general---feel free to reach out to me. My e-mail address isn't terribly hard to figure out, I'm often found online in a variety of Slack communities, and you can reach [me on Twitter][link-99] as well as [in the Fediverse][link-30] (Mastodon). Enjoy your weekend!

[link-1]: https://dlorenc.medium.com/should-you-sign-git-commits-f068b07e1b1f
[link-2]: https://aws.amazon.com/blogs/containers/aws-lambda-for-the-containers-developer/
[link-3]: https://databouncing.io/
[link-4]: https://williamlam.com/2024/07/quick-tip-updating-intel-ixgben-driver-enables-multi-gigabit-2-5gbe-5gbe-selection-in-esxi.html
[link-5]: https://major.io/p/firewalld-port-redirection/
[link-6]: https://www.abortretry.fail/p/the-itanic-saga
[link-7]: https://www.darkreading.com/endpoint-security/apple-geolocation-api-exposes-wi-fi-access-points-worldwide
[link-8]: https://lostintransit.se/2024/07/08/why-do-we-have-native-vlans/
[link-9]: https://medium.com/@amitmavgupta/cilium-installing-cilium-in-gke-with-no-kube-proxy-826e84f971b4
[link-10]: https://hackingthe.cloud/aws/general-knowledge/connection-tracking/
[link-11]: https://imaginarytext.ca/posts/2024/pandoc-typst-tutorial/
[link-12]: https://medium.com/@kvihanga/how-to-set-up-a-kubernetes-cluster-on-ubuntu-22-04-lts-433548d9a7d0
[link-13]: https://nickjanetakis.com/blog/check-if-a-string-contains-a-substring-in-bash
[link-14]: https://mkz.me/weblog/posts/cilium-enable-ingress-controller/
[link-15]: https://surajremanan.com/posts/automating-talos-installation-on-proxmox-with-packer-and-terraform/
[link-16]: https://budimanjojo.github.io/talhelper/latest/
[link-17]: https://williamlam.com/2024/07/slick-jonsbo-d31-computer-case-with-embedded-lcd-screen-for-homelab.html
[link-18]: https://lucvandonkersgoed.com/2024/07/13/dear-aws-please-let-me-be-a-cloud-engineer-again/
[link-19]: https://a-cup-of.coffee/blog/istio/
[link-20]: https://www.dwarmstrong.org/charging-thresholds/
[link-21]: https://trufflesecurity.com/blog/anyone-can-access-deleted-and-private-repo-data-github
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2024-06-28-technology-short-take-179.md" >}}
