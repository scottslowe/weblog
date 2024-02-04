---
author: slowe
categories: Information
comments: true
date: 2022-10-28T07:30:00-04:00
tags:
- Apple
- Automation
- AWS
- DNS
- Hardware
- Kubernetes
- macOS
- Networking
- OpenStack
- Pulumi
- Security
- Storage
- Virtualization
- VMware
title: 'Technology Short Take 161'
url: /2022/10/28/technology-short-take-161/
---

Welcome to Technology Short Take #161! It's been a little over a month since the last Technology Short Take, although [the Full Stack Journey recently did an "Audio Edition" of a Technology Short Take][link-29] that you should probably check out. In any case, I've spent the last month collecting links to articles and tutorials from around the web on all the various technologies that us IT folk are likely to encounter in our day-to-day adventures. I hope there's something here that you find useful!<!--more-->

## Networking

* Atulpriya Sharma [illustrates][link-20] how to use DNS-based routing (with Argo Rollouts and Azure Traffic Manager) to implement a blue/green deployment strategy.
* Ivan Pepelnjak addresses [the idea that network automation is harmful][link-24]. I believe that IT folks in _all_ disciplines, including networking, need to embrace automation.
* How awesome is it that [the first commercial distribution of SONiC][link-25] comes from a company named Hedgehog?

## Servers/Hardware

* Howard Oakley has a great series on Apple Silicon; the series is up to three posts so far. The [first post][link-11] provides a high-level overview of how Apple Silicon M-series chips are different, and the [second post][link-18] has more details on the capabilities of the P and E cores. The [third post][link-13] discusses how macOS allocates threads to different cores to maximize performance. Good stuff!
* Looks like some significant bandwidth increases could be on the horizon; see [this article][link-23] for more details.

## Security

* Using Auth0? You may find [this article][link-6] by Juan Cruz Martinez on using Pulumi to provision Auth0 resources to be helpful.
* Alejandro Villanueva shares [26 AWS security best practices to adopt in production][link-9].

## Cloud Computing/Cloud Management

* The open source `ko` project [has applied to become a CNCF Sandbox project][link-15].
* Ahmet Alp Balkan [writes about some intricacies][link-2] regarding reloading files from Kubernetes Secret and ConfigMap volumes.
* Tiago Sousa writes about Amplemarket's [use of Pulumi to deal with growing pains][link-3].
* Daniel Karnutsch shares [an ideal Azure Kubernetes Cluster configuration][link-5].
* William Lam writes about [the beta for VMware Cloud Consumption Interface (CCI)][link-7].
* This article lists all the reasons why [you should have lots of AWS accounts][link-8].
* [This site][link-10] seems to have a pretty decent amount of Azure-related content, which is helpful for folks who may be spinning up their Azure skills.
* Tim Myers [shares][link-12] how to set up continuous deployment of infrastructure-as-code using GitHub Actions.
* Dekel Malul [shares 6 `kubectl` plugins you must try][link-21].
* Guillermo Ramirez Garcia asks, ["Is OpenStack fighting a lost battle?"][link-22] In a word: yes. For all the reasons described in this article, although I do want to highlight one in particular: making it easy to install. I believe this to be one of the biggest issues---the OpenStack community never focused on ease of adoption. Contrast that with Kubernetes, which started out "difficult" but invested in core tooling like `kubeadm` and Cluster API.
* Aidan Steele shares how to save some money on AWS (for hobby/side projects) with [this article][link-28].

## Operating Systems/Applications

* I found [this article on how Apple Pay works][link-4] to be quite interesting reading.
* Jim Perrin [announces][link-14] preview availability of [CBL-Mariner][link-19] as a container host for Azure Kubernetes Service (AKS).
* Got (or getting) a new Mac? Here's [a setup guide][link-27] that might have some useful information for you.

## Programming

* Engin Diri shares [some tools and techniques for learning Rust][link-26].

## Virtualization

* Nicholas Schmidt takes on [using `cloud-init` with vSphere][link-1], using openSUSE as the Linux platform.

## Career/Soft Skills

* I love [this idea][link-16]!
* Mike McQuaid talks about [entitlement in open source][link-17]. I think some of the lessons in this article could apply to a lot of different scenarios, not just open source usage.

That's all for now! As always, thank you for taking the time to read this post, and feel free to reach out to me with any feedback or suggestions for improvement. You can reach [me on Twitter][link-99], or find me in a number of different Slack communities (such as [the Pulumi community Slack][link-30] or [the Kubernetes community Slack][link-31]). I look forward to hearing from you!

[link-1]: https://blog.engyak.co/2022/09/using-cloud-init-with-vsphere-and.html
[link-2]: https://ahmet.im/blog/kubernetes-inotify/
[link-3]: http://blog.amplemarket.com/using-pulumi-to-deal-with-growing-pains/
[link-4]: https://codeburst.io/how-does-apple-pay-actually-work-f52f7d9348b7
[link-5]: https://danielkarnutsch.com/posts/2022-09-04-my-ideal-azure-kubernetes-service-configuration/
[link-6]: https://auth0.com/blog/provisioning-auth0-resources-with-type-script-and-pulumi/
[link-7]: https://williamlam.com/2022/09/beta-for-vmware-cloud-consumption-interface-cci-formally-project-cascade.html
[link-8]: https://src-bin.com/you-should-have-lots-of-aws-accounts/
[link-9]: https://sysdig.com/blog/26-aws-security-best-practices/
[link-10]: https://build5nines.com/
[link-11]: https://eclecticlight.co/2022/10/03/making-the-most-of-apple-silicon-power-1-m-series-chips-are-different/
[link-12]: https://fearlessaws.substack.com/p/automating-infrastructure-as-code
[link-13]: https://eclecticlight.co/2022/10/13/making-the-most-of-apple-silicon-power-3-controls/
[link-14]: https://techcommunity.microsoft.com/t5/azure-infrastructure-blog/announcing-preview-availability-of-the-mariner-aks-container/ba-p/3649154
[link-15]: https://opensource.googleblog.com/2022/10/ko-applies-to-become-a-cncf-sandbox-project.html
[link-16]: https://ericholscher.com/blog/2017/aug/2/pacman-rule-conferences/
[link-17]: https://mikemcquaid.com/entitlement-in-open-source/
[link-18]: https://eclecticlight.co/2022/10/05/making-the-most-of-apple-silicon-power-2-core-capabilities/
[link-19]: https://github.com/microsoft/CBL-Mariner
[link-20]: https://www.infracloud.io/blogs/blue-green-deployments-dns-routing/
[link-21]: https://itnext.io/6-kubectl-plugins-you-must-try-1411dcbcf950
[link-22]: https://memooo.ooo/posts/is-openstack-losing/
[link-23]: https://www.inavateonthenet.net/news/article/new-chip-can-transmit-the-entire-internets-traffic-in-one-second
[link-24]: https://blog.ipspace.net/2022/10/network-automation-considered-harmful.html
[link-25]: https://www.nextplatform.com/2022/10/12/after-long-last-a-commercial-grade-sonic-network-operating-system/
[link-26]: https://blog.ediri.io/learn-rust-in-under-10-mins
[link-27]: https://www.swyx.io/new-mac-setup
[link-28]: https://awsteele.com/blog/2022/10/15/cheap-serverless-containers-using-api-gateway.html
[link-29]: https://packetpushers.net/podcast/full-stack-journey-071-technology-short-takes-audio-edition/
[link-30]: https://pulumi-community.slack.com
[link-31]: https://kubernetes.slack.com
[link-99]: https://twitter.com/scott_lowe
