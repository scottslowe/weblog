---
author: slowe
categories: Information
comments: true
date: 2020-11-06T09:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Apple
- macOS
- Docker
- Kubernetes
- Azure
- AWS
- Git
- Linux
- Fedora
- VMware
- Go
title: 'Technology Short Take 133'
url: /2020/11/06/technology-short-take-133/
---

Welcome to Technology Short Take #133! This time around, I have a collection of links featuring the new Raspberry Pi 400, some macOS security-related articles, information on AWS Nitro Enclaves and gVisor, and a few other topics. Enjoy!<!--more-->

## Networking

* Pratik Mankad shows [how to use DNS hostnames as targets for an AWS NLB][link-25]. If we're honest, it's a bit of a hack; it uses AWS Lambda to periodically resolve the hostname and update the NLB target IP address(es) accordingly. Native DNS hostname support in NLBs would be a far better solution.
* Netflix has a good post on [how they use prioritized load shedding][link-29] to provide a good user experience during system outages.

## Servers/Hardware

* The Raspberry Pi 400 is a neat offering. See [this post][link-26] for more details.

## Security

* In the [last Technology Short Take][xref-1], I mentioned that some testing with macOS Big Sur indicated that Apple had left itself a backdoor for network traffic from its own applications. Here's [another article on the topic][link-1], with some additional technical detail.
* Here's [a fairly detailed article on macOS authorization][link-5].
* Wojciech Regula takes a look at [accessing ("stealing") confidential data stored in the macOS Keychain][link-21].
* How did I not know about Network Time Security (NTS)? Here's a post on [using NTS to secure NTP][link-23] on Fedora.
* Here's [a list of some `Dockerfile` security best practices][link-30].

## Cloud Computing/Cloud Management

* Raphael Yoshiga provides an [Azure-to-AWS mapping of services][link-2].
* Ben Bridts has four tips to help you [level up your CloudFormation usage][link-6].
* Chip Zoller explains [how to use custom registries with Tanzu Kubernetes Grid (TKG)][link-7]. His method is enabled by the fact that TKG leverages Cluster API, and Cluster API builds on other community efforts like `kubeadm`. (See, this is why learning `kubeadm` still has value in a Cluster API-based world!)
* Validating Kubernetes manifests in some sort of automated fashion is something I've been interested in for a while (I've had a draft blog post sitting around for nigh on a year now), and `kube-linter` falls right into that area. Check out [the `kube-linter` GitHub repository][link-11], and read [the Stackrox blog post announcing the project][link-12].
* vSphere administrators may find [this guide to day 2 operations with Tanzu Kubernetes Grid][link-13] helpful.
* Google has a guide on [how users can help prepare their Google Cloud environments for the Docker Hub pull request limits][link-16] that will, by the time this post is published, have been live for almost two weeks.
* Forrest Brazeal provides [his take][link-22] on the rumor that AWS will announce a multi-cloud management tool.

## Operating Systems/Applications

* Geert Baeke has a post on the new HashiCorp tool, Waypoint, showing [how to build and tag an image using Docker][link-3].
* And while we're on the topic of building images, here's a post from Alex Ellis on [building containers without Docker][link-4].
* Richard Hughes provides [an update on `fwupd` version 1.5.0][link-8].
* Here are [some handy `git` tricks][link-9].
* This is [a pretty awesome (pun intended) list][link-17] of resources for Visual Studio Code.
* Scott Bollinger has a post on [an all-in-one alias for updating macOS][link-24].
* The Cilium team recently conducted the eBPF Summit, focused on all things eBPF. The Cilium website hosts recap posts of day 1 ([here][link-27]) and day 2 ([here][link-28]).

## Programming

* I haven't (yet) had the chance to walk through it, but [this tutorial][link-10] looks to be very promising, providing exposure to both AWS Lambda and using Go. It's definitely on my list!

## Virtualization

* David Stevens has a write-up on [how to backup vCenter v7 using SMB][link-14].
* AWS Nitro Enclaves look like _very_ interesting technology; see [this blog post from AWS][link-15]. Also, [this post by Aidan Steele][link-19] has some great information on Nitro Enclaves as well. If I'm honest, this is an opportunity VMware should have capitalized on a long time ago with vSphere. (If you're wondering why this is under the "Virtualization" section, go read the blog post!)
* Sam Perrin has [a list of useful automation/orchestration resources][link-18].
* Ian Lewis and Michael Pratt have [a good post on gVisor and how it uses the concept of a "platform"][link-20] in its functionality.

That's all I have for now---hopefully you found something useful and informative! Feel free to hit [me on Twitter][link-99] if you have any feedback or suggestions for improvement. I'm also open to items that I should consider for inclusion in a future Tech Short Take.

[link-1]: https://medium.com/tripmode/apple-started-hiding-the-traffic-of-its-own-mac-apps-9dc83c5a9c5b
[link-2]: https://itnext.io/azure-to-aws-map-70d4c56f55a7
[link-3]: https://blog.baeke.info/2020/10/19/hashicorp-waypoint-image-tagging/
[link-4]: https://blog.alexellis.io/building-containers-without-docker/
[link-5]: https://theevilbit.github.io/posts/macos_authorization/
[link-6]: https://www.cloudar.be/awsblog/level-up-your-cloudformation-usage-with-these-4-tips/
[link-7]: https://neonmirrors.net/post/2020-10/using-custom-registries-with-tkg/
[link-8]: https://blogs.gnome.org/hughsie/2020/10/26/new-fwupd-1-5-0-release/
[link-9]: https://opensource.com/article/20/10/advanced-git-tips
[link-10]: https://buddy.works/tutorials/determine-prominent-colors-in-a-picture-your-first-aws-lambda-in-go
[link-11]: https://github.com/stackrox/kube-linter
[link-12]: https://www.stackrox.com/post/2020/10/introducing-kubelinter-an-open-source-linter-for-kubernetes/
[link-13]: https://veducate.co.uk/vsphere-tanzu-kubernetes-day-2-operations/
[link-14]: https://blog.psustevens.com/2020/10/30/how-to-backup-vcenter-v7-using-smb/
[link-15]: https://aws.amazon.com/blogs/aws/aws-nitro-enclaves-isolated-ec2-environments-to-process-confidential-data/
[link-16]: https://cloud.google.com/blog/products/containers-kubernetes/mitigating-the-impact-of-new-docker-hub-pull-request-limits
[link-17]: https://github.com/viatsko/awesome-vscode
[link-18]: https://samperrin.com/posts/vmware-automation-orchestration-resources/
[link-19]: https://awsteele.com/2020/11/02/2020-11-02-nitro-enclaves-first-impressions.html
[link-20]: https://gvisor.dev/blog/2020/10/22/platform-portability/
[link-21]: https://wojciechregula.blog/post/stealing-macos-apps-keychain-entries/
[link-22]: https://cloudirregular.substack.com/p/aws-hearts-multi-cloud-its-gonna
[link-23]: https://fedoramagazine.org/secure-ntp-with-nts/
[link-24]: https://scott-bollinger.com/macos-update-aio-alias
[link-25]: https://aws.amazon.com/blogs/networking-and-content-delivery/hostname-as-target-for-network-load-balancers/
[link-26]: https://www.raspberrypi.org/blog/raspberry-pi-400-the-70-desktop-pc/
[link-27]: https://cilium.io/blog/2020/10/28/ebpf-summit-day-1
[link-28]: https://cilium.io/blog/2020/10/29/ebpf-summit-day-2
[link-29]: https://netflixtechblog.com/keeping-netflix-reliable-using-prioritized-load-shedding-6cc827b02f94
[link-30]: https://cloudberry.engineering/article/dockerfile-security-best-practices/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2020-10-23-technology-short-take-132.md" >}}
