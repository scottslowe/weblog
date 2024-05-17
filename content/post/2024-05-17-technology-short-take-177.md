---
author: slowe
categories: Information
comments: true
date: 2024-05-17T09:00:00-06:00
tags:
- Apple
- Automation
- Docker
- Git
- Hardware
- Kubernetes
- Linux
- Networking
- NSX
- OPA
- Pulumi
- Python
- Security
- SSH
- Storage
- Terraform
- Virtualization
- VPN
- VXLAN
title: 'Technology Short Take 177'
url: /2024/05/17/technology-short-take-177/
---

Welcome to Technology Short Take #177! Wow, is it the middle of May already? The year seems to be flying by---much in the same way that all these technical articles keep flying by my Inbox, occasionally getting caught and included here! In this Technology Short Take, I have links on things ranging from physical network designs to running retro operating systems as virtual machines. Surely there will be something useful in here for you!<!--more-->

## Networking

* Blogger Evert has a two part series ([here][link-7] and [here][link-3]) on managing NSX ALBs with Terraform.
* Ivan launches a series of blog posts exploring routing protocol designs that can be used to implement EVPN-with-VXLAN L2VPNs in a leaf-and-spine fabric. The first one is [here][link-17]. What's really cool is that Ivan also includes a `netlab` topology readers can use to create a lab and see how it works.
* Eduard Tolosa discusses [binding wireless network adapters to `systemd-nspawn` containers][link-20].
* Ioannis Theodoridis has a three-part series on how he and his team used tools like Nautobot, Nornir, and Python to help with some extensive network migrations. Check out the series ([part 1][link-25], [part 2][link-26], and [part 3][link-27]); I think you'll find some useful information in there.

## Servers/Hardware

* While in many respects Apple's M series CPUs are amazing, all is not perfect: security researchers have discovered a flaw that would allow attackers to steal cryptographic keys. More details are available in [this Zero Day article][link-14].

## Security

* Rory McCune explores [using Tailscale for getting persistence][link-15] in a compromised Kubernetes cluster.
* The Cisco Talos team is warning of [large-scale brute force attacks against VPN and SSH services][link-18] on a variety of devices (including Cisco, CheckPoint, Fortinet, SonicWall, and Ubiquiti).

## Cloud Computing/Cloud Management

* Open Policy Agent (OPA) is approaching their 1.0 release, and they've already started [discussing what users can do today to prepare for the big release][link-2]. I must say I appreciate the OPA team's efforts in making the transition to 1.0 as smooth and seamless as possible.
* A colleague shared this article on [CPU limits and throttling in Kubernetes][link-4].
* Anton Weiss of PerfectScale explores [memory QoS with EKS and Bottlerocket][link-21].
* Brian Grant explores the question of [whether GitOps is actually useful][link-11].
* Via [this blog post on the Docker web site][link-22], Diana Esteves of Pulumi shares how to use the new Docker Build provider to automate image builds. It also looks like the Pulumi team has added [a Docker Build example][link-23] to their "examples" repository. C'mon, Pulumi team---show us more than just TypeScript!

## Operating Systems/Applications

* Julia Evans digs into [what "current branch" means in Git][link-5].
* Nikhil shares [a framework for selecting a Linux distribution][link-6].
* José Ignacio Amelivia Santiago takes readers on [a detailed walkthrough][link-8] of setting up Arch Linux on a new Framework 13 AMD-based laptop.
* Here's [a list of "crisis tools"][link-9] recommended to install on your Linux servers _before_ you need them.
* Only one word applies [here][link-12]: oops.
* I found [this tool][link-13] in the last couple of weeks, and it is so absolutely useful (to me, anyway).
* Envoy Gateway has officially released version 1.0.0, marking GA for the project. More details are available in [this announcement][link-19].

## Programming/Development

* This is a fantastic article by Jeremiah Lee [about Spotify's failed "squad model"][link-10] and some of the key lessons folks can learn.

## Storage

* Steven Sklar explains [how CSI (Container Storage Interface) works][link-16].

## Virtualization

* Talk about a blast from the past! William Lam [discusses][link-1] running a prerelease version of OS/2 2.0---an operating system I myself ran in the mid-1990s before switching to Windows NT---as a virtual machine on VMware ESXi. For what it's worth, I remain convinced that OS/2 version 2 was technologically superior to its Windows peers (including Windows NT). It's another example of when the best technology doesn't always win.

## Career/Soft Skills

* If you're looking for a list of skills that are valuable for a DevOps engineer/SRE/platform engineer to know, look no further than [this comprehensive list from Nick Janetakis][link-24].

OK, that's all for this time around. Did you like this post, or another post on the site? Or maybe you have a question? Feel free to reach out! I always enjoy hearing from readers, so I invite you to find me [on Twitter][link-99], [on the Fediverse][link-30], or in one of the various Slack communities I frequent. (You can drop me an e-mail, if you'd prefer---my address isn't too hard to find.) Thanks for reading!

[link-1]: https://williamlam.com/2024/03/pre-release-microsoft-os-2-2-0-on-esxi.html
[link-2]: https://blog.openpolicyagent.org/opa-1-0-is-coming-heres-what-you-need-to-know-c8fb0d258368
[link-3]: https://www.amcom.io/posts/managing-nsx-alb-with-terraform-part-2
[link-4]: https://www.numeratorengineering.com/requests-are-all-you-need-cpu-limits-and-throttling-in-kubernetes/
[link-5]: https://jvns.ca/blog/2024/03/22/the-current-branch-in-git/
[link-6]: https://www.unsungnovelty.org/posts/01/2024/a-linux-distro-recommendation-framework-and-my-picks-for-2024/
[link-7]: https://www.amcom.io/posts/managing-nsx-alb-with-terraform-part-1/
[link-8]: https://namelivia.com/i-switched-to-a-framework-amd-13/
[link-9]: https://www.brendangregg.com/blog/2024-03-24/linux-crisis-tools.html
[link-10]: https://www.jeremiahlee.com/posts/failed-squad-goals/
[link-11]: https://medium.com/@briankgrant/is-gitops-actually-useful-a1c851ba99d8
[link-12]: https://thehftguy.com/2023/11/14/the-linux-kernel-has-been-accidentally-hardcoded-to-a-maximum-of-8-cores-for-nearly-20-years/
[link-13]: https://difftastic.wilfred.me.uk/
[link-14]: https://www.zetter-zeroday.com/apple-chips/
[link-15]: https://raesene.github.io/blog/2024/03/24/Using-Tailscale-for-persistence/
[link-16]: https://sklar.rocks/how-container-storage-interface-works/
[link-17]: https://blog.ipspace.net/2024/04/evpn-designs-vxlan-leaf-spine-fabric.html
[link-18]: https://www.bleepingcomputer.com/news/security/cisco-warns-of-large-scale-brute-force-attacks-against-vpn-services/
[link-19]: https://gateway.envoyproxy.io/announcements/v1.0/
[link-20]: https://blog.nspawn.org/posts/wireless-adapters-on-systemd-nspawn-containers/
[link-21]: https://www.perfectscale.io/blog/cgroups-and-memoryqos-w-bottlerocket
[link-22]: https://www.docker.com/blog/pulumi-and-docker-build-cloud/
[link-23]: https://github.com/pulumi/examples/tree/master/dockerbuildcloud-ts
[link-24]: https://nickjanetakis.com/blog/120-skills-i-use-in-an-sre-platform-devops-developer-position
[link-25]: https://www.mythryll.com/?p=1976
[link-26]: https://www.mythryll.com/?p=2237
[link-27]: https://www.mythryll.com/?p=2238
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
