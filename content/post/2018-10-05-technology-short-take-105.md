---
author: slowe
categories: Information
comments: true
date: 2018-10-05T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- AWS
- Kubernetes
- vSphere
- OpenStack
- Terraform
- Ansible
- Linux
- Fedora
- CLI
- BSD
- Go
title: 'Technology Short Take 105'
url: /2018/10/05/technology-short-take-105/
---

Welcome to Technology Short Take #105! Here's another collection of articles and blog posts about some of the common technologies that modern IT professionals will typically encounter. I hope that something I've included here proves to be useful for you.<!--more-->

## Networking

* Jon Williams discusses [deploying Anycast DNS using OpenBSD and BGP][link-22].
* Brian Boucheron provides a great article on [how to inspect and debug Kubernetes network primitives][link-23]. The information in this article is a great foundation upon which to build, in my opinion.

## Servers/Hardware

* The big news in the hardware space recently was [the article regarding the purported Chinese hardware supply chain attack][link-30]. This has generated a _lot_ of discussion. First, we have [Amazon's reply][link-31], in which they categorically deny all such claims. [Apple's response][link-33] similarly denies the allegations in the report. Of the (innumerable) articles commenting on the hardware hack, [this article by Joe FitzPatrick][link-32] and [this ErrataSec article][link-34] caught my eye. Both articles point out the incredible difficulty of finding such a hardware hack, and point out that exploiting software vulnerabilities would probably be a much faster/easier way to accomplish the same goal. Will we ever know the truth as to whether this actually happened? Probably not.

## Security

* I suspect I'll find myself here at some point, so Jeff Geerling's script to [get AWS STS session tokens for MFA with AWS CLI][link-2] is probably going to be handy when that time comes.
* Sathyajith Bhat covers [five open source tools for container security][link-10].
* Cory O'Daniel explains [how to use role-based access control to assign Pod Security Policies (PSPs) to Kubernetes workloads][link-11].

## Cloud Computing/Cloud Management

* I talk about Terraform a fair amount (and I use it a fair amount). Many times that's in the context of a public cloud (since that's where I spend most of my time), but here's [an example of using it for vSphere and OpenStack][link-5].
* Chris Herrera tackles a little bit of the "why" in [this article on Kubernetes cluster design][link-8].
* Bob Killen [discusses exposing StatefulSets in Kubernetes][link-9].
* Lots of folks are super-bullish on Helm, but Bartlomiej Antoniak suggests users [think twice before using Helm][link-13].
* Robert Verdam has a two-part series on deploying an application to AWS with Terraform and Ansible ([part 1][link-14], [part 2][link-15]). Of particular interest---to me, anyway---was Robert's use of the Terraform provider+inventory script, which I'm exploring for use in some of my own projects.
* The `kubespy` tool, released by the Pulumi folks, looks interesting. Have a look at [part 1][link-16] and [part 2][link-17] of their blog posts about the CLI tool.
* Grant Orchard has a three-part series (so far) on VMware Cloud Assembly ([part 1][link-19], [part 2][link-20], [part 3][link-21]).
* The folks at Platform9 recently open-sourced a tool called `etcdadm` (inspired by `kubeadm`). The GitHub repository is [here][link-28], and the blog post with the announcement is [here][link-29].

## Operating Systems/Applications

* Sayan Chowdhury talks about [Fedora 28 AMIs gaining Elastic Network Adapter (ENA) support][link-1].
* I've been looking into Dropbox replacements; [Syncthing][link-3] is a leading candidate at the moment. Here's [an article on Syncthing][link-4] that provides some basic details. If any Syncthing users have any feedback they'd like to share, [hit me up on Twitter][link-99].
* [This][link-6] looks interesting, although I don't (personally) have a use case just yet. It might be worth watching as eBPF grows in importance in the Linux community, though.
* I mentioned [this article on Pandoc][link-7] in a tweet recently. I'm a long-time Pandoc user, but I hadn't thought/didn't know about using the `-s` parameter with an external YAML file to control styling for different output formats. That's a neat trick.
* Matthew Green [rants on Chrome's new forced login policy][link-12]. I'm not a fan, either.
* Here's a nice deep dive [on Linux executables][link-18].
* I'm (increasingly) not a fan of Google services, but I thought it was interesting to find [this Google Hangouts desktop client][link-26].

## Storage

I don't have anything to share this time, but I'll stay alert for content or links to include next time.

## Virtualization

* William Lam explores some of the [caveats/gotchas of Nested ESXi on VMware Cloud on AWS (VMC)][link-27]. Some of these gotchas are fairly significant (no inter-host networking for VMs running on nested HV's, for example).

## Career/Soft Skills

* I found a couple of resources for folks interested in learning Golang. First up is [this Go study group][link-24], which has both US and India meeting times. Next up is [Awesome Go][link-25], which is---in the author's words---"a curated list of awesome Go frameworks, libraries, and software."

That's all for this Technology Short Take. Thanks for reading! If you have questions, comments, or suggestions for improvement, feel free to [contact me on Twitter][link-99]. Have a great weekend!

[link-1]: https://words.yudocaa.in/fedora-amis-gets-ena-support/
[link-2]: https://www.jeffgeerling.com/blog/2018/getting-aws-sts-session-tokens-mfa-aws-cli-and-kubectl-eks-automatically
[link-3]: https://syncthing.net/
[link-4]: https://opensource.com/article/18/9/take-control-your-data-syncthing
[link-5]: https://github.com/trodemaster/tfe-openstack-vsphere/
[link-6]: https://github.com/jessfraz/bpfd
[link-7]: https://opensource.com/article/18/9/intro-pandoc
[link-8]: https://medium.com/hashmapinc/tips-for-designing-a-kubernetes-cluster-d020f0b728ee
[link-9]: https://itnext.io/exposing-statefulsets-in-kubernetes-698730fb92a1
[link-10]: https://opensource.com/article/18/8/tools-container-security
[link-11]: https://medium.com/coryodaniel/kubernetes-assigning-pod-security-policies-with-rbac-2ad2e847c754
[link-12]: https://blog.cryptographyengineering.com/2018/09/23/why-im-leaving-chrome/
[link-13]: https://medium.com/virtuslab/think-twice-before-using-helm-25fbb18bc822
[link-14]: https://robertverdam.nl/2018/09/03/deploying-an-application-to-aws-with-terraform-and-ansible-part-1-terraform/
[link-15]: https://robertverdam.nl/2018/09/22/deploying-an-application-to-aws-with-terraform-and-ansible-part-2-ansible/
[link-16]: https://blog.pulumi.com/kubespy-and-the-lifecycle-of-a-kubernetes-pod-in-four-images
[link-17]: https://blog.pulumi.com/kubespy-trace-a-real-time-view-into-the-heart-of-a-kubernetes-service
[link-18]: https://ownyourbits.com/2018/05/23/the-real-power-of-linux-executables/
[link-19]: https://grantorchard.com/tango/introducing-cas/
[link-20]: https://grantorchard.com/tango/blueprinting-intro/
[link-21]: https://grantorchard.com/tango/blueprint-versioning/
[link-22]: https://jonwillia.ms/2018/09/23/anycast-dns-openbsd
[link-23]: https://dev.to/digitalocean/how-to-inspect-and-debug-kubernetes-networking-primitives-d7n
[link-24]: https://gophersource.com/study-group/
[link-25]: https://awesome-go.com/
[link-26]: https://github.com/yakyak/yakyak
[link-27]: https://www.virtuallyghetto.com/2018/09/nested-esxi-on-vmware-cloud-on-aws-vmc.html
[link-28]: https://github.com/platform9/etcdadm
[link-29]: https://platform9.com/blog/were-open-sourcing-etcdadm-heres-what-it-means-for-kubernetes-in-production/
[link-30]: https://www.bloomberg.com/news/features/2018-10-04/the-big-hack-how-china-used-a-tiny-chip-to-infiltrate-america-s-top-companies
[link-31]: https://aws.amazon.com/blogs/security/setting-the-record-straight-on-bloomberg-businessweeks-erroneous-article/
[link-32]: https://securinghardware.com/articles/hardware-implants/
[link-33]: https://www.apple.com/newsroom/2018/10/what-businessweek-got-wrong-about-apple/
[link-34]: https://blog.erratasec.com/2018/10/notes-on-bloomberg-supermicro-supply.html
[link-99]: https://twitter.com/scott_lowe
