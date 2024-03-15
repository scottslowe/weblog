---
author: slowe
categories: Information
comments: true
date: 2024-03-15T09:00:00-06:00
tags:
- Apple
- Automation
- AWS
- ESXi
- Hardware
- Intel
- Kubernetes
- KVM
- Linux
- macOS
- Networking
- Security
- SSH
- Storage
- TLS
- Virtualization
- VMware
title: 'Technology Short Take 176'
url: /2024/03/15/technology-short-take-176/
---

Welcome to Technology Short Take #176! This Tech Short Take is a bit heavy on security-related links, but there's still some additional content in a number of other areas, so you should be able to find something useful---or at least interesting---in here. Thanks for reading!<!--more-->

## Networking

* Lee Briggs (formerly of Pulumi, now with Tailscale) shows [how to use the Tailscale Operator to create "free" Kubernetes load balancers][link-2] ("free" as in no additional charge above and beyond what it would normally cost to operate a Kubernetes cluster).
* Ivan Pepelnjak dives deep on [DHCP relaying on a Linux host][link-24].
* I also enjoyed Ivan's [realistic take][link-25] on rollbacks in a network automation environment. (TL;DR: It's not as easy as it might seem.)

## Servers/Hardware

* Menno Finlay-Smits shares information on [reducing fan noise on Intel NUCs][link-11].
* Rob McBryde shares his story of [reviving a 2012 MacBook Pro with Linux][link-20].
* Kevin Houston [previews the first AMD-powered Cisco UCS blade server][link-23].

## Security

* In early February a vulnerability was uncovered in a key component of the Linux boot process. The vulnerability affects virtually all Linux distributions and allows attackers to bypass the secure boot protections and insert a low-level bootkit. While the requirements for exploiting the vulnerability are not insurmountable, they do require a certain level of effort. More details available [via Ars Technica][link-5] and [via ZDnet][link-6].
* Nick Frichette shares [how to bypass GuardDuty Tor client findings][link-7] (basically, how to connect to Tor without GuardDuty detecting it).
* The Sysdig Threat Research Team uncovered the malicious use of a network mapping tool called SSH-Snake. Read more about it [in this post][link-8].
* VMware is patching a set of severe "sandbox escape" bugs. Two of the vulnerabilities are rated a 9.3 out of 10, and even VMware's flagship ESXi hypervisor is affected. More details are [available from Ars Technica][link-18].
* Think Linux doesn't have malware? A [new Bifrost remote access trojan][link-19] (RAT) for Linux employs a number of techniques to remain hidden, including using a "VMware-esque" domain name for command and control servers.
* And [here's another example][link-22] of malware that is targeting Linux (along with Windows).
* [This][link-21] would be why I hate it when companies force me to use SMS for two-factor authentication---at least let me use a one-time passcode or something.

## Cloud Computing/Cloud Management

* Mina Abadir shares some experiences around [migrating from PlanetScale to Amazon Aurora][link-10].
* Rory McCune [explains Kubernetes authentication][link-14].
* Falco has [graduated within the CNCF][link-16].

## Operating Systems/Applications

* Here's one person's take [on `sudo` for Windows][link-4].
* [This is a handy trick][link-3].
* David Both has an article on [using systemd journals for troubleshooting][link-13]. It looks like this is part of a larger series on systemd.
* Major Hayden talks about [connecting Caddy to Porkbun][link-17] to help with automating TLS certificates.

## Storage

* Gergely Imreh [discusses ZFS on a Raspberry Pi][link-1].
* Cal Paterson [explains why S3 is not a filesystem][link-26].

## Virtualization

* In the wake of Broadcom discontinuing VMware ESXi Free, Nutanix is hoping to fill the gap with Nutanix Community Edition. Vladan Seget [provides some additional details in his blog post][link-9]. Given that Nutanix Community Edition is based on the open source KVM hypervisor, this _could_ lead to greater KVM adoption among small businesses and virtualization hobbyists who formerly would have used VMware's solution.
* Staf Wagemakers (I think I have the name right) describes [running OpenBSD as a UEFI virtual machine on a Raspberry Pi][link-12].
* I stumbled across a pair of articles by Greg Gant on the use of QEMU to run older versions of Mac OS (including pre-Mac OS X versions): there's [the original piece][link-28], and then [an updated piece][link-27].

## Career/Soft Skills

* Robb Owen shares [why it's OK to abandon your side project][link-15].
* [This distinction between stories and tasks][link-29] is probably applicable even outside agile development environments and practices, especially when it boils down to "you _must_ still think about what the user needs". Good stuff!

That's all for now! I always love hearing from readers, so if you found something useful in this post---or in any post---don't hesitate to reach out! You can reach me [on Twitter][link-99], [on the Fediverse][link-30], or in a number of different Slack communities. You're also welcome to drop me an e-mail; my address is here on the site (it's not hard to find). Enjoy!

[link-1]: https://gergely.imreh.net/blog/2024/02/zfs-on-a-raspberry-pi/
[link-2]: https://leebriggs.co.uk/blog/2024/02/26/cheap-kubernetes-loadbalancers
[link-3]: https://monospacementor.com/2023/09/save-a-file-from-vim-with-root-permissions/
[link-4]: https://www.tiraniddo.dev/2024/02/sudo-on-windows-quick-rundown.html
[link-5]: https://arstechnica.com/security/2024/02/critical-vulnerability-affecting-most-linux-distros-allows-for-bootkits/
[link-6]: https://www.zdnet.com/article/shim-vulnerability-exposes-most-linux-systems-to-attack/
[link-7]: https://hackingthe.cloud/aws/avoiding-detection/guardduty-tor-client/
[link-8]: https://sysdig.com/blog/ssh-snake/
[link-9]: https://www.vladan.fr/nutanix-community-edition/
[link-10]: https://www.flightcontrol.dev/blog/we-migrated-from-planetscale-to-aws-aurora-v2
[link-11]: https://menno.io/posts/intel-nuc-fan-noise/
[link-12]: https://stafwag.github.io/blog/blog/2024/02/25/run-opentbsd-as-a-vm-on-pi/
[link-13]: https://www.both.org/?p=3876
[link-14]: https://securitylabs.datadoghq.com/articles/kubernetes-security-fundamentals-part-3/
[link-15]: https://robbowen.digital/wrote-about/abandoned-side-projects/
[link-16]: https://sysdig.com/blog/falco-cncf-graduation/
[link-17]: https://major.io/p/caddy-porkbun
[link-18]: https://arstechnica.com/security/2024/03/vmware-issues-patches-for-critical-sandbox-escape-vulnerabilities/
[link-19]: https://www.bleepingcomputer.com/news/security/new-bifrost-malware-for-linux-mimics-vmware-domain-for-evasion/#google_vignette
[link-20]: https://robmcbryde.com/reviving-a-2012-macbook-pro-with-linux/
[link-21]: https://techcrunch.com/2024/02/29/leaky-database-two-factor-codes/
[link-22]: https://www.linuxsecurity.com/news/hackscracks/krustyloader-backdoor
[link-23]: https://bladesmadesimple.com/2024/02/first-look-at-ciscos-amd-blade-server/
[link-24]: https://blog.ipspace.net/2024/02/dhcp-relaying-linux-host.html
[link-25]: https://blog.ipspace.net/2024/02/undo-network-automation.html
[link-26]: https://calpaterson.com/s3.html
[link-27]: https://blog.greggant.com/posts/2021/12/18/ppc-qemu-mac-os-9-with-sound-on-apple-silicon-intel-mac.html
[link-28]: https://blog.greggant.com/posts/2021/01/13/install-powerpc-macos-osx-on-apple-silicon-m1-and-x86-intel.html
[link-29]: https://ntietz.com/blog/work-on-tasks-not-stories/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
