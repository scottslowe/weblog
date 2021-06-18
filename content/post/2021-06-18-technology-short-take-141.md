---
author: slowe
categories: Information
comments: true
date: 2021-06-18T07:30:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Cilium
- Kubernetes
- macOS
- Azure
- AWS
- Apple
- CAPI
- Git
- Pulumi
- Go
- VMware
title: 'Technology Short Take 141'
url: /2021/06/18/technology-short-take-141/
---

Welcome to Technology Short Take #141! This is the first Technology Short Take compiled, written, and published entirely on my M1-based MacBook Pro (see [my review here][xref-1]). The collection of links shared below covers a fairly wide range of topics, from old Sun hardware to working with serverless frameworks in the public cloud. I hope that you find something useful here. Enjoy!<!--more-->

## Networking

* Via Ivan Pepelnjak, I was pointed to Jon Langemak's [in-depth discussion of working with Linux VRFs][link-1].
* I read a couple of Cilium-related blog posts recently that may be useful. The first is [a post on Cilium and F5 load balancer integration][link-6], while the second discusses [implementing Kubernetes network policies with Cilium and Linkerd][link-7].
* Sonia Cuff provides a set of links for detailed instructions on setting up [VPN access from macOS to Microsoft Azure with Azure Active Directory authentication][link-15].
* Patrick Kremer [walks through][link-22] using Postman to implement BGP route filtering with VMware Cloud on AWS. I think this post is interesting/useful for a couple different reasons. One, the content is useful in and of itself. Second, the content reflects---in my opinion---the changing nature of what it means to be a "networking professional." It's not just about working with network device CLIs, but using APIs and interacting with both on-premises networking as well as cloud networking.

## Servers/Hardware

* Much has been said about the "snappy" feel of Apple's new M1-based Macs, and [this article][link-11] takes a look at how Quality of Service (QoS) across the efficiency and performance cores may be the reason why these new Macs seem so responsive.
* Here's [an entertaining tale][link-16] of attempting to resurrect a Sun Ultra 1 Workstation.
* John Gruber's [post on "Secure Intent" on Apple devices][link-27] was, for me at least, an informative read. I hadn't delved that much into Apple's hardware security efforts around Secure Enclave, mostly due to the fact that I was running older Apple hardware (a fact that has since changed).

## Security

* Hari Rana has [a detailed discussion of security concerns regarding Flatpak][link-2] (a packaging format for Linux applications).
* Published back in January of this year, Sysdig's container security and usage report [reveals][link-9] some interesting security trends.
* [This][link-10] is an interesting look at how someone managed to transmit arbitrary data via Apple's "Find My" network.
* SentinelOne [shares][link-14] a list of "must-have" apps and tools for hackers working on a Mac.
* Ugh---a new Rowhammer exploit/technique. More details are available [here][link-17].
* Want to learn more about X.509 certificates? [Check this out][link-23].

## Cloud Computing/Cloud Management

* Sandy Cash has a two-part series on working with Kubernetes ConfigMaps ([part 1][link-3] and [part 2][link-4]).
* Alessandro Perilli has [a very lengthy article on NoOps][link-5]. I love his different "paths" to NoOps (like "No(rmalized)Ops" or "No(tMy)Ops").
* Rory McCune [tackles][link-8] the topic of permissions and RBAC in Kubernetes.
* Sam Weston [takes a look at EKS Managed Node Groups][link-12], examining both the good and bad aspects of this functionality, and provides some sample configurations.
* I recently got turned on to Alex Mitelman's System Design Weekly, which is like a super-cool version of Technology Short Takes but a bit more tightly focused. In any event, there's a ton of great information available [here][link-13], so I recommend checking it out.
* How about a tutorial on installing Cilium and Linkerd in a Kubernetes cluster? [Here you go][link-18].
* For VMware-heavy customers, [this post][link-26] by Cormac Hogan that draws out the connections between the upstream Cluster API project and VMware's TKG product may be helpful.

## Operating Systems/Applications

* Michael Gasch has [a nice post on `git` and using it to collaborate on an open source project][link-28]. I was happy to see that many of the `git` practices I've adopted align with Michael's recommendations, and learned a few new tricks along the way.

## Programming

* Lee Briggs [takes a look][link-21] at Pulumi's use of the `Output` type and the `apply()` method in writing declarative code using an imperative programming language.
* Ahmet Alp Balkan shows [how to serve up gRPC and HTTP from a Go app on Cloud Run][link-29]. I like reading posts like this, because even though I don't necessarily understand _all_ the code (yet) I feel like exposing me to the code is still helpful. I could be wrong; time will tell.
* AJ Stuyvenberg [demonstrates][link-30] how to use the serverless framework to enable and simplify the use of multiple stacks for developers and avoid trying to emulate cloud environments on laptops.

## Virtualization

* Andrei Warkentin provides [an update regarding ESXi-Arm][link-25] (ESXi on ARM hardware).

## Career/Soft Skills

* I enjoyed this article [by Cat Swetel on the nature of power][link-19].
* Here are some tips for [unshackling your education][link-20].
* I suspect [this list of CRE "life lessons"][link-24] from Google probably has some wisdom that can be applied in a variety of ways, not just in a CRE-specific context.

That's all I have for now, but hopefully it's enough to provide some helpful and useful reading over the weekend. I'm always open to hearing from readers, so feel free to reach out to [me on Twitter][link-99], find me on Slack (I frequent the Kubernetes Slack instance), or send me an e-mail (my address isn't hard to find). Thanks for reading!

[link-1]: http://www.dasblinkenlichten.com/working-with-linux-vrfs/
[link-2]: https://theevilskeleton.frama.io/2021/02/11/response-to-flatkill-org.html
[link-3]: https://itnext.io/working-with-kubernetes-configmaps-part-1-volume-mounts-f0ace283f5aa
[link-4]: https://itnext.io/working-with-kubernetes-configmaps-part-2-watchers-b6dd0e583d71
[link-5]: https://alessandroperilli.com/acontrarianview/noops/
[link-6]: https://cilium.io/blog/2021/01/29/how-to-build-k8s-networking-with-f5-and-cilium
[link-7]: https://buoyant.io/2020/12/23/kubernetes-network-policies-with-cilium-and-linkerd/
[link-8]: https://raesene.github.io/blog/2021/01/16/Getting-Into-A-Bind-with-Kubernetes/
[link-9]: https://sysdig.com/blog/sysdig-2021-container-security-usage-report/
[link-10]: https://positive.security/blog/send-my
[link-11]: https://eclecticlight.co/2021/05/17/how-m1-macs-feel-faster-than-intel-models-its-about-qos/
[link-12]: https://cablespaghetti.dev/2021/03/20/aws-eks-managed-node-groups/
[link-13]: https://mitelman.engineering/system-design-weekly/
[link-14]: https://www.sentinelone.com/blog/hackers-on-macs-what-are-the-must-have-apps-tools/
[link-15]: https://techcommunity.microsoft.com/t5/itops-talk-blog/vpn-access-to-azure-from-macos-with-azure-active-directory/ba-p/2400026
[link-16]: https://www.rup12.net/posts/2021/adventures-with-sun-ultra-1-workstation/
[link-17]: https://security.googleblog.com/2021/05/introducing-half-double-new-hammering.html
[link-18]: https://selcuk.miynat.com/post/2021-05-18-cilium-linkerd-kubernetes/
[link-19]: https://www.catswetel.com/blog/2021/5/4/an-extended-subtweet-on-power
[link-20]: https://nesslabs.com/unbounded-learning
[link-21]: https://www.leebriggs.co.uk/blog/2021/05/09/pulumi-apply.html
[link-22]: http://www.patrickkremer.com/vmware-cloud-on-aws-bgp-route-filtering-with-postman/
[link-23]: https://darutk.medium.com/illustrated-x-509-certificate-84aece2c5c2e
[link-24]: https://cloud.google.com/blog/products/devops-sre/sre-at-google-our-complete-list-of-cre-life-lessons
[link-25]: https://blogs.vmware.com/arm/2021/06/15/esxi-arm-fling-1-4-refresh/
[link-26]: https://cormachogan.com/2021/06/01/a-closer-look-at-cluster-api-and-tkg-v1-3-1/
[link-27]: https://daringfireball.net/2021/06/secure_intent_on_apple_devices
[link-28]: https://www.mgasch.com/2021/05/git-basics/
[link-29]: https://ahmet.im/blog/grpc-http-mux-go/
[link-30]: https://dev.to/aws-builders/developing-against-the-cloud-55o4
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2021-06-02-review-2020-m1-based-macbook-pro.md" >}}
