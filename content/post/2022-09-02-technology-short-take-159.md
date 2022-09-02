---
author: slowe
categories: Information
comments: true
date: 2022-09-02T08:55:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Intel
- GitOps
- Kubernetes
- Git
- AWS
- Terraform
- Pulumi
- EKS
- iOS
- WireGuard
- VPN
- Cilium
- VMware
- vSphere
- ESXi
title: 'Technology Short Take 159'
url: /2022/09/02/technology-short-take-159/
---

Welcome to Technology Short Take #159! If you're interested in finding some links to articles around the web on topics like WASM, Git, Sigstore, or EKS---among other things---then you've come to the right place. I've spent the last few weeks collecting articles I think you'll find useful, gleaning them from the depths of Twitter, RSS feeds, Reddit, and Slack. Enjoy, and never stop learning!<!--more-->

## Networking

* I'm not a Tailscale user (although I do use WireGuard), but [this KB article describing how to use Tailscale on AWS App Runner][link-18] is pretty cool.
* Speaking of WireGuard: here's [a tutorial on setting up WireGuard on Ubuntu][link-23] (both 20.04 and 22.04 are covered).
* Ivan Pepelnjak laments the sad state of terminology surrounding [router interfaces and switch ports][link-22].

## Servers/Hardware

* Steven J. Vaughan-Nichols [discusses a leak discovered in Intel SGX][link-5]. It's a shame, too...I had high hopes for SGX.
* I found Andrew Meier's [post on his personal infrastructure][link-16] interesting to read. I stopped doing the homelab thing back in 2016, but it's still interesting to read about how others do things. Thanks for sharing, Andrew!

## Security

* Hayden Blauzvern talks through how organizations with existing infrastructure for signing (here we are referring to signing packages or other artifacts) could potentially [incrementally adopt Sigstore][link-7].
* Ashley Penney takes a look at SealedSecrets and ExternalSecrets in order to provide [a guide for managing secrets in a GitOps environment][link-11].

## Cloud Computing/Cloud Management

* Syrus Akbary of Wasmer discusses using WebAssembly as a universal binary format in a two-part (so far) blog series ([part 1][link-1], [part 2][link-2]).
* Eric Evans reviews how to [create a serverless AWS EKS cluster using Pulumi][link-6].
* EKS News is a great resource for links related to EKS; check out [the latest issue][link-8] (as of this article being written).
* Ian Miell [takes a historical journey][link-10] through the epochs of IT in order to answer the question, "Who should write the Terraform?" Not surprisingly, the answer is "It depends," but Ian does a good job of explaining what it depends upon.
* Alan Scott [discusses options][link-12] for enforcing policy with Kubernetes, including using your existing CI/CD infrastructure, leveraging Kubernetes' own admission control functionality, and using tools like OPA/Gatekeeper.
* Lakindu Hewawasam walks readers through [managing "micro-stacks" with Pulumi][link-13] (exports and StackReferences are the key here).
* Engin Diri shows [how to enable the Cilium Hubble UI in a Civo k3s cluster][link-14].
* Jack Roper shows some [best practices and examples for managing Terraform state][link-15].
* Kyle Galbraith [introduces Depot's new self-hosted builders][link-19].
* Zac Charles takes [a closer look at Lambda's response payload size limit][link-30].

## Operating Systems/Applications

* Felix Krause caused quite a stir with his revelation that a number of iOS apps are "able to track every single interaction with external websites, from all form inputs like passwords and addresses, to every single tap." More details are available [here][link-3].
* Leigh McCulloch shows [how to push to multiple remotes at once][link-4].
* And while we're speaking of Git, Nick Janetakis [shows how to use `git shortlog`][link-21] to see all kinds of useful information about a repository.
* [This list of macOS `defaults` commands][link-26] may be useful for those who like to tweak their systems.
* This article by Jacob Davis-Hansson on how [your `Makefile`s are wrong][link-27] was informative. Some tips didn't apply to me (not surprisingly, macOS is stuck with an old version of `make`), but I did learn a few things from the article.
* Howard Oakley has [a great article on macOS memory][link-29] (as seen via Activity Monitor).

## Programming

* Mat Ryer talks about [aligning the happy path to the left edge][link-17].

## Virtualization

* William Lam provides instructions on [manually cleaning up a Distributed Virtual Switch (DVS) on an ESXi host][link-20].
* Christian Mohn shares [some details on what is expected in vSphere 8][link-24]. Duncan Epping also has [a post on vSphere 8][link-25]. (There's probably a bunch more, these are just two that caught my eye.)

## Career/Soft Skills

* Sophie Weston lays out the argument for [why psychological safety is key for knowledge workers][link-9].
* Ofir Sharony says that [using a journal can help improve you as a leader or manager][link-28].

It's time to wrap up now; I hope that you found something useful. If you have any feedback (all constructive feedback is welcome!), please feel free to reach out to me. You can find [me on Twitter][link-99], or in any number of different Slack communities. Thanks for reading!

[link-1]: https://wasmer.io/posts/wasm-as-universal-binary-format-part-1-native-executables
[link-2]: https://wasmer.io/posts/wasm-as-universal-binary-format-part-2-wapm
[link-3]: https://krausefx.com/blog/ios-privacy-instagram-and-facebook-can-track-anything-you-do-on-any-website-in-their-in-app-browser
[link-4]: https://leighmcculloch.com/posts/git-push-to-multiple-remotes-at-once/
[link-5]: https://thenewstack.io/intel-sgx-not-so-safe-after-all-aepic-leak/
[link-6]: https://blog.scalesec.com/create-an-serverless-aws-eks-cluster-using-pulumi-95c65ac94dea
[link-7]: https://blog.sigstore.dev/adopting-sigstore-incrementally-1b56a69b8c15
[link-8]: https://eks.news/archives/029/
[link-9]: https://confluxhq.com/insight/our-organizational-os-needs-an-upgrade-why-knowledge-works-needs-a-new-approach
[link-10]: https://zwischenzugs.com/2022/08/08/who-should-write-the-terraform/
[link-11]: https://releasehub.com/blog/how-to-manage-gitops-secrets-a-detailed-guide
[link-12]: https://itnext.io/kubernetes-owasp-top-10-centralised-policy-enforcement-9adc53438e22
[link-13]: https://blog.bitsrc.io/managing-micro-stacks-using-pulumi-87053eeb8678
[link-14]: https://blog.ediri.io/how-to-enable-the-cilium-hubble-ui-in-a-civo-k3s-cluster
[link-15]: https://spacelift.io/blog/terraform-state
[link-16]: https://andrewmeier.dev/personal-infrastructure
[link-17]: https://medium.com/@matryer/line-of-sight-in-code-186dd7cdea88
[link-18]: https://tailscale.com/kb/1127/aws-app-runner/
[link-19]: https://depot.dev/blog/self-hosted-depot
[link-20]: https://williamlam.com/2022/08/how-to-manually-clean-up-a-distributed-virtual-switch-vds-on-an-esxi-host.html
[link-21]: https://nickjanetakis.com/blog/counting-all-git-commits-from-all-authors-and-more-with-git-shortlog
[link-22]: https://blog.ipspace.net/2022/09/interfaces-ports.html
[link-23]: https://www.howtoforge.com/how-to-set-up-wireguard-vpn-on-ubuntu-22-04/
[link-24]: https://vninja.net/2022/08/30/vmware-vsphere8-announced/
[link-25]: https://www.yellow-bricks.com/2022/08/30/introducing-vsphere-8/
[link-26]: https://macos-defaults.com/#%F0%9F%99%8B-what-s-a-defaults-command
[link-27]: https://tech.davis-hansson.com/p/make/
[link-28]: https://medium.com/@ofirsharony/leadership-journal-become-an-inspiring-leader-in-10-minutes-a-day-60233908ec14
[link-29]: https://eclecticlight.co/2022/08/10/getting-more-from-activity-monitor-memory/
[link-30]: https://zaccharles.medium.com/deep-dive-lambdas-response-payload-size-limit-8aedba9530ed
[link-99]: https://twitter.com/scott_lowe
