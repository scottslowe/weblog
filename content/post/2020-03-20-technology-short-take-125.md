---
author: slowe
categories: Information
comments: true
date: 2020-03-20T08:50:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Git
- Azure
- Microsoft
- Kubernetes
- Pulumi
- AWS
- CLI
title: 'Technology Short Take 125'
url: /2020/03/20/technology-short-take-125/
---

Welcome to Technology Short Take #125, where I have a collection of articles about various data center and cloud technologies collected from around the Internet. I hope I have managed to find a few useful things for you! (If not, [contact me on Twitter][link-99] and tell me how I can make this more helpful for you.)<!--more-->

## Networking

* Dinesh Dutt of Cumulus Networks writes about [the effect of switch port count in Clos topologies][link-1].
* Tim Smith shares [how to set up additional blocklists for Pi-Hole for your home lab][link-7].
* Matt Oswalt has a super-cool post on [building your own JunOS router with cRPD and LinuxKit][link-15]. Good stuff here! This blending of "traditional" network engineering with containers, Linux, and DevOps tooling is how Matt is setting new trends and directions for the networking industry.

## Servers/Hardware

Nothing this time around. I'll try hard to find something useful for the next Technology Short Take.

## Security

* Chris Wahl touches on the topic of [using GitHub personal tokens to authenticate to HashiCorp Vault][link-6].
* Guido Appenzeller---once my manager at VMware, now at Yubico---shared [this article about the Cerberus banking trojan][link-9], which is apparently now able to steal two-factor authentication (2FA) tokens from the Google Authenticator application. Yikes.
* And while we are on the topic of Yubico: the company recently released [this security advisory][link-13] related to running a self-hosted one-time password (OTP) validation server. Note that the vulnerability discussed does not affect their most well-known product, the Yubikeys.
* Alexander Bakker discusses [a mysterious bug in the firmware of Google's Titan M chip][link-23].
* Mark Ermolov [discusses a vulnerability][link-25] in the ROM (read-only memory) of the Intel Converged Security and Management Engine (CSME).
* Aaron Parecki discusses [the first draft of OAuth 2.1][link-26].

## Cloud Computing/Cloud Management

* Paul Czarkowski has a post on [creating self-signed certificates on Kubernetes][link-8].
* Pulumi recently introduced [the ability to generate YAML][link-11] from a supported programming language (like JavaScript/TypeScript, Python, or .NET---Go support is coming soon). It feels like this implementation---and others like it, like `jk`, which I wrote about [here][xref-1]---are just syntactic differences, meaning you're still writing the manifest. You're just writing it in a different language/format than YAML. Maybe it's just me. Convince me I'm wrong.
* Eric Hammond has a great article on [running AWS CLI commands across multiple accounts in an AWS organization][link-12]. Lots of `bash` here, you've been forewarned.
* Nigel Poulton has a list of `kubectl` tips [here][link-16].
* Keith Lee has [a list of Cluster API resources][link-21] you might want to check out. (I've been known to write [a few Cluster API things][link-22] as well.)
* Javier Aguilar takes readers through [using CloudFormation to create a VPC with a NAT gateway][link-24].
* William Lam provides [a sneak peek at deploying Tanzu Kubernetes Grid on vSphere and VMware Cloud on AWS][link-28]. I hope to publish something similar for "native" AWS soon.
* And here's Joe Mann talking about [deploying Kubernetes clusters to vSphere using Cluster API Provider for vSphere (CAPV) version 0.6.0][link-29] (which brings support for v1alpha3).
* Anne Currie has [an update][link-30] on the "climate friendliness" of various cloud providers.

## Operating Systems/Applications

* Kornelis Sietsma looks at the options for working with [multiple git identities][link-3] on a single system.
* Michael Dales [shares his experience][link-14] trying out Windows 10 and Windows Subsystem for Linux (WSL) as a Linux development environment.
* Robert Guske has a three-part series on running Linux development desktops with VMware Horizon ([part 1][link-17], [part 2][link-18], and [part 3][link-19]).
* Jeff Johnson talks about [notarization and customer privacy][link-31] in newer releases of macOS.

## Storage

* Chin-Fah Heoh has [a write-up][link-4] from Storage Field Day 19 about a range of open source projects and initiatives related to storage.
* You can also go get [your fill of storage-related links][link-5] from J Metz.

## Virtualization

* Hannel Hazeley of Microsoft shows [how to set up nested virtualization for Azure VM/VHD][link-2]. Hannel's post also lays out a couple of use cases/scenarios for such a configuration.
* [This][link-10] is interesting, albeit more from a "science experiment" perspective than anything useful.
* Frank Denneman walks readers through the [initial placement of a vSphere Pod][link-20] (a _vSphere Pod_ is a new construct available with "Project Pacific").

## Career/Soft Skills

* Jessica Dean [shares her home office setup][link-27], which is applicable since so many folks are/will be working from home for a while. Her setup is a bit pricey for my budget, but still provides some useful ideas.

That's all for now! I hope that you've found something useful. I welcome all feedback from readers, so I invite you to contact [me on Twitter][link-99] if you have corrections, feedback, suggestions for improvement, or just want to say hello.

[link-1]: https://elegantnetwork.github.io/posts/Effect-of-Switch-Port-Count/
[link-2]: https://techcommunity.microsoft.com/t5/itops-talk-blog/how-to-setup-nested-virtualization-for-azure-vm-vhd/ba-p/1115338
[link-3]: https://blog.korny.info/2020/02/10/multiple-git-identities.html
[link-4]: http://storagegaga.com/open-source-and-open-standards-open-the-future/
[link-5]: https://jmetz.com/2020/02/storage-short-take-24/
[link-6]: https://wahlnetwork.com/2020/02/10/vault-auth-using-github-personal-tokens/
[link-7]: https://tsmith.co/2018/setting-up-additional-pi-hole-blocklists-forwarding-for-homelab/
[link-8]: https://tech.paulcz.net/blog/creating-self-signed-certs-on-kubernetes/
[link-9]: https://www.threatfabric.com/blogs/2020_year_of_the_rat.html
[link-10]: https://www.vimalin.com/blog/install-android-x86-in-vmware-fusion/
[link-11]: https://www.pulumi.com/blog/kubernetes-yaml-generation/
[link-12]: https://alestic.com/2019/12/aws-cli-across-organization-accounts/
[link-13]: https://www.yubico.com/support/security-advisories/ysa-2020-01/
[link-14]: https://www.digitalflapjack.com/blog/2020/3/12/your-next-linux-distrubtion-windows-10
[link-15]: https://oswalt.dev/2020/03/building-your-own-junos-router-with-crpd-and-linuxkit/
[link-16]: https://nigelpoulton.com/blog/f/kubectl-tips-part-1
[link-17]: https://rguske.github.io/post/a-linux-development-desktop-with-vmware-horizon-part-i-horizon/
[link-18]: https://rguske.github.io/post/a-linux-development-desktop-with-vmware-horizon-part-ii-applications/
[link-19]: https://rguske.github.io/post/a-linux-development-desktop-with-vmware-horizon-part-iii-shell/
[link-20]: https://frankdenneman.nl/2020/03/06/initial-placement-of-a-vsphere-native-pod/
[link-21]: https://keithlee.ie/2020/03/07/all-things-cluster-api/
[link-22]: /tags/capi/
[link-23]: https://alexbakker.me/post/mysterious-google-titan-m-bug-cve-2019-9465.html
[link-24]: https://godof.cloud/vpc-with-nat-gateway/
[link-25]: http://blog.ptsecurity.com/2020/03/intelx86-root-of-trust-loss-of-trust.html
[link-26]: https://aaronparecki.com/2020/03/11/14/oauth-2-1
[link-27]: https://jessicadeen.com/remote-current-office-setup/
[link-28]: https://www.virtuallyghetto.com/2020/03/sneak-peak-at-deploying-tanzu-kubernetes-grid-plus-on-vsphere-vmware-cloud-on-aws.html
[link-29]: https://mannimal.blog/2020/03/13/deploy-kubernetes-clusters-to-vsphere-with-capv-0-6-0/
[link-30]: https://blog.container-solutions.com/the-green-cloud-how-climate-friendly-is-your-cloud-provider
[link-31]: https://lapcatsoftware.com/articles/notarization-privacy.html
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-10-29-programmatically-creating-kubernetes-manifests.md" >}}
