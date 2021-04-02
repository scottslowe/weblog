---
author: slowe
categories: Information
comments: true
date: 2021-04-02T09:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VMware
- Linux
- Kubernetes
- AWS
- CLI
- Go
- Git
- vSphere
title: 'Technology Short Take 139'
url: /2021/04/02/technology-short-take-139/
---

Welcome to Technology Short Take #139! This Technology Short Take is a bit heavy on cloud, OS, and programming topics, but there should be enough other interesting links to be useful to plenty of folks. (At least, I hope that's the case!) Now, let's get on to the content!<!--more-->

## Networking

* Tony Mackay has a tutorial showing [how to use Traefik to rate-limit requests to a WordPress instance][link-1].
* Ali Al Idrees has [a post][link-6] on using NSX ALB (formerly Avi Networks) with Kubernetes clusters in a vSphere with Tanzu environment.
* [This post][link-7] provides some examples of shared control planes (and thus shared failure domains) within networking.
* In [this post][link-18], Jakub Sitnicki digs way deep into the Linux kernel to uncover the answer to the question, "Why are there no entries in the conntrack table for SYN packets dropped by the firewall?" Get ready to get nerdy!
* [This article][link-29] on eBPF and Isovalent (the company behind the Cilium CNI plugin for Kubernetes) has some statements with which I agree, and some that don't make sense to me. For example, I agree with the statement that the "impact eBPF will have on networking, security and observability will be widespread". However, I don't understand how eBPF will "reduce reliance on legacy network overlays". I could see how eBPF will _change_ how network overlays are implemented, sure, but reduce the reliance on network overlays? I'm not sure about that. If you have strong feelings about this, hit [me on Twitter][link-99] and let's discuss.

## Servers/Hardware

* Dominic Hopton shares a sordid tale of getting [three monitors to work with a 13" MacBook Pro][link-10].

## Security

* Linux malware is [getting more sophisticated][link-2].
* A [browser-based side-channel attack][link-11]? Even worse, this isn't just limited to Intel chips, but may also affect ARM-based systems like Apple's M1 CPUs. Further, turning off JavaScript doesn't help. Ugh.
* Given the prevalence of VMware's ESXi hypervisor, I suppose it was only a matter of time before the bad guys really [started targeting it in a major way][link-14]. This time, they're exploiting a weakness that VMware can't patch: people.
* A while ago I chatted with the folks at Indeni about [Cloudrail][link-31], a security solution for infrastructure-as-code environments.

## Cloud Computing/Cloud Management

* Patrick Kremer [writes][link-4] about using vRealize Log Insight Cloud to monitor for firewall changes in a VMware Cloud on AWS environment.
* [Aye aye, Popeye!][link-8]
* Daniel Mangum's post on [Crossplane as the infrastructure LLVM][link-9] is (in my opinion) a great read, particularly so if you're interested in the intersection of Kubernetes and infrastructure as code.
* Here's a post on [installing and configuring containerd as a Kubernetes container runtime][link-12].
* If you're a DynamoDB user, check out this list of [29 DynamoDB best practices][link-17] compiled by Rafal Wilinski.
* Marcin Cuber discusses [process and considerations for upgrading EKS to version 1.19][link-20].
* Need to list assets across multiple cloud providers? [Check out `cloudlist`][link-27].

## Operating Systems/Applications

* [This announcement of the Scarf Gateway][link-3] popped into my Twitter timeline recently, and after taking a look at how the Scarf Gateway is described I can see how this is an important addition to companies' secure software supply chain efforts, especially in the beginning. Why in the beginning? Because that's when you're struggling to understand the dependencies of your software supply chain, and the Scarf Gateway provides that sort of visibility (as I understand it).
* [This is handy][link-16].
* Ben Kehoe shares [his favorite Zoom tips][link-19].
* How about [an OCI runtime for FreeBSD Jails][link-21]?
* `xh` appears to be an as-yet-incomplete reimplementation of HTTPie in Rust. Check out [the GitHub repository][link-24].
* Jan Grzegorowski [shares how to remap a single Mac keyboard key][link-25] using `hidutil`.
* I just recently [learned about `sox`][link-28], which I think of as the audio file equivalent of ImageMagick.

## Programming

* A fair amount of this article was over my head, but I still enjoyed reading about [how Tailscale built a new IP address type for Go][link-5].
* Francisco Trindade launches a series of posts that tackle the prevalent use of pull requests (PRs) in software development with the statement that [PRs are considered harmful][link-13].
* Here's a list of [10 advanced Git tips to help improve your developer workflow][link-22] (be aware this appears to be an HTTP-only site).
* Some of [these repositories][link-23] may be worth checking out.

## Storage

* [This post][link-26] from Enterprise Storage Forum attempts to provide a comparison of cloud storage between AWS and Google Cloud. Frankly, though, I found the article to be a bit unfocused, also discussing other cloud services instead of really concentrating on being the best comparison of cloud storage services. Maybe that's just me, though.

## Virtualization

* Mike Foley [shares details][link-15] on a new feature in vSphere 7 Update 2 that leverages AMD-specific functionality to create what are called "Confidential Containers."

Happy reading and learning! If you have any questions, comments, suggestions for improvement, or other feedback, I'm always happy to hear from you. Contact [me on Twitter][link-99] and let's chat!

[link-1]: https://graspingtech.com/wordpress-docker-traefik-rate-limit/
[link-2]: https://www.bleepingcomputer.com/news/security/linux-malware-uses-open-source-tool-to-evade-detection/
[link-3]: https://about.scarf.sh/post/announcing-scarf-gateway
[link-4]: https://www.patrickkremer.com/monitoring-vmware-cloud-on-aws-firewall-changes-with-vrealize-log-insight-cloud/
[link-5]: https://tailscale.com/blog/netaddr-new-ip-type-for-go/
[link-6]: https://yallavirtual.com/2020/11/23/tanzu-kubernetes-cluster-ingress-with-nsx-alb/
[link-7]: https://blog.engyak.net/2021/03/unearned-uptime-present-and-future.html
[link-8]: https://popeyecli.io/
[link-9]: https://danielmangum.com/posts/crossplane-infrastructure-llvm/
[link-10]: https://www.codevoid.net/ruminations/2020/09/27/three-displays-three-times-the-fun.html
[link-11]: https://9to5mac.com/2021/03/11/browser-based-attack-affects-intel-m1-macs/
[link-12]: https://www.centinosystems.com/blog/containers/installing-and-configuring-containerd-as-a-kubernetes-container-runtime/
[link-13]: https://medium.com/@franciscomt/pull-requests-considered-harmful-c3a10af8becd
[link-14]: https://www.crowdstrike.com/blog/carbon-spider-sprite-spider-target-esxi-servers-with-ransomware/
[link-15]: https://core.vmware.com/blog/confidential-containers-vsphere-pods-amd
[link-16]: https://www.cyberciti.biz/hardware/how-to-protects-linux-and-unix-machines-from-accidental-shutdownsreboots-with-molly-guard/
[link-17]: https://dynobase.dev/dynamodb-best-practices/
[link-18]: https://blog.cloudflare.com/conntrack-turns-a-blind-eye-to-dropped-syns/
[link-19]: https://ben11kehoe.medium.com/my-favorite-zoom-tips-c944961c0191
[link-20]: https://itnext.io/amazon-eks-upgrade-journey-from-1-18-to-1-19-cca82de84333
[link-21]: https://samuel.karp.dev/blog/2021/03/runj-a-new-oci-runtime-for-freebsd-jails/
[link-22]: http://alanpryorjr.com/2019-03-09-git_tips/
[link-23]: https://javascript.plainenglish.io/10-essential-github-repos-for-software-developers-6a42ebba279
[link-24]: https://github.com/ducaale/xh
[link-25]: https://www.grzegorowski.com/how-to-remap-single-mac-keyboard-key
[link-26]: https://www.enterprisestorageforum.com/cloud/amazon-web-services-vs-google-cloud-cloud-storage-comparison/
[link-27]: https://www.kitploit.com/2021/02/cloudlist-tool-for-listing-assets-from.html?m=1
[link-28]: https://opensource.com/article/20/2/linux-sox
[link-29]: https://containerjournal.com/topics/container-networking/isovalent-container-networking-in-2021-using-ebpf/
[link-30]: https://acloudguru.com/blog/engineering/from-student-to-engineer-how-to-study-smarter-for-cloud-certs
[link-31]: https://indeni.com/cloudrail/
[link-99]: https://twitter.com/scott_lowe
