---
author: slowe
categories: Information
comments: true
date: 2021-01-15T10:30:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VPN
- SSH
- AWS
- Kubernetes
- Apple
- Encryption
- vSphere
- Microsoft
- Linux
- macOS
title: 'Technology Short Take 136'
url: /2021/01/15/technology-short-take-136/
---

Welcome to Technology Short Take #136, the first Short Take of 2021! The content this time around seems to be a bit more security-focused, but I've still managed to include a few links in other areas. Here's hoping you find something useful!<!--more-->

## Networking

* Jason Eckert provides [an introduction to using WireGuard for VPN connectivity][link-4].
* Who else knew that [HAProxy could route SSH connections][link-10]?
* This article by Joshua Fox outlines [how and when to use each of the various types of firewalls offered by AWS][link-13].
* Rory McCune points out that [Kubernetes is a router][link-20], and users should not rely on the fact that pods are not accessible from the outside by default as any form of a security barrier.

## Servers/Hardware

* Thinking of buying an M1-powered Mac? You may find [this list][link-12] helpful.

## Security

* [UEFI implants][link-23] and [executable PNGs][link-24]? The (computing) world is getting to be a scary place, my friends. Compute safely.
* Scott Piper shares some [lesser-known techniques for attacking AWS environments][link-2].
* The popular open source cryptography library known as Bouncy Castle has uncovered a severe authentication bypass vulnerability. More details are available in [this article][link-7].
* From early December 2020, there's also [this reminder][link-8] about the security updates released by VMware to address a zero-day vulnerability in several products.
* Teri Radichel discusses [why encryption at rest isn't necessarily a silver bullet][link-14].
* Dark Reading shares some [additional details on the SolarWinds attack][link-19].

## Cloud Computing/Cloud Management

* Want to enable logging in every AWS service that exists (as of 2021)? Matt Fuller [has you covered][link-1].
* Kief Morris walks readers through [why pull requests are not necessary with your team][link-3]. Although written with infrastructure as code in mind, conceivably his arguments could be applied to other environments as well.
* Heidi Howard and Ittai Abraham illustrate [some of the challenges of the Raft consensus protocol when there is a network partition][link-11].
* Cormac Hogan shares some lessons learned [using a Kubernetes Operator to query vSphere resources][link-22].
* Wasmer, a WebAssembly runtime, [recently hit 1.0][link-25].

## Operating Systems/Applications

* Here's [an explanation on including the Origin header with `curl`][link-5].
* Ryan Blunden has authored [a fairly comprehensive piece on environment variables in Linux and macOS][link-6].
* I found this article on [`systemd-resolved`, split DNS, and VPN configuration][link-9] to be rather helpful.
* [Linux may be coming to the Apple M1 chip][link-15].
* And speaking of the Apple M1 chip, Ernie Smith shares [how he managed to "break" his M1-powered Mac using Dropbox][link-16].
* [Here][link-17] are five lines that should---according to the author---get included in your `.vimrc` configuration file.
* After reading [this article][link-21], I learned two things. First, that there _is_ a CLI for Microsoft 365. Second, that you can run this CLI in a Docker container. Well, there you go.

## Career/Soft Skills

* Have you ever accidentally supported one of [these myths of IT][link-18]?

That's it this time around! If you have any questions, comments, or corrections, feel free to contact me. I'm a regular visitor to [the Kubernetes Slack instance][link-98], or you can just hit [me on Twitter][link-99]. Thanks!

[link-1]: https://matthewdf10.medium.com/how-to-enable-logging-on-every-aws-service-in-existence-circa-2021-5b9105b87c9
[link-2]: https://tldrsec.com/blog/lesser-known-aws-attacks/
[link-3]: https://infrastructure-as-code.com/book/2021/01/02/pull-requests.html
[link-4]: https://jasoneckert.github.io/myblog/an-introduction-to-wireguard-vpn/
[link-5]: https://dannyda.com/2021/01/11/how-to-include-origin-in-curl-command-how-to-send-a-regular-cross-origin-resource-sharing-cors-request-using-curl/
[link-6]: https://doppler.com/blog/how-to-set-environment-variables-in-linux-and-mac
[link-7]: https://www.bleepingcomputer.com/news/security/bouncy-castle-crypto-authentication-bypass-vulnerability-revealed/
[link-8]: https://www.bleepingcomputer.com/news/security/vmware-fixes-zero-day-vulnerability-reported-by-the-nsa/
[link-9]: https://blogs.gnome.org/mcatanzaro/2020/12/17/understanding-systemd-resolved-split-dns-and-vpn-configuration/
[link-10]: https://www.haproxy.com/blog/route-ssh-connections-with-haproxy/
[link-11]: https://decentralizedthoughts.github.io/2020-12-12-raft-liveness-full-omission/
[link-12]: https://isapplesiliconready.com/for/m1
[link-13]: https://blog.doit-intl.com/aws-firewalls-101-how-and-when-to-use-each-one-d4ad8087a6b3
[link-14]: https://medium.com/cloud-security/the-encryption-fallacy-6872435bdef6
[link-15]: https://thetechnologygeek.org/linux-could-be-coming-to-the-apple-m1-chip/
[link-16]: https://debugger.medium.com/how-to-break-apples-m1-chip-ccdcbe3bf0df
[link-17]: https://swordandsignals.com/2020/12/13/5-lines-in-vimrc.html
[link-18]: https://networktherapy.wordpress.com/2010/12/08/the-myths-of-it-part-2/
[link-19]: https://www.darkreading.com/threat-intelligence/more-solarwinds-attack-details-emerge/d/d-id/1339885
[link-20]: https://raesene.github.io/blog/2021/01/03/Kubernetes-is-a-router/
[link-21]: https://pnp.github.io/cli-microsoft365/user-guide/run-cli-in-docker-container/
[link-22]: https://cormachogan.com/2021/01/04/using-a-kubernetes-operator-to-query-vsphere-resources/?amp=1
[link-23]: https://eclypsium.com/2020/10/14/protecting-your-organizations-from-mosaicregressor-and-other-uefi-implants/
[link-24]: https://djharper.dev/post/2020/12/26/executable-pngs/
[link-25]: https://medium.com/wasmer/wasmer-1-0-3f86ca18c043
[link-98]: https://kubernetes.slack.com/
[link-99]: https://twitter.com/scott_lowe
