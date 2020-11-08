---
author: slowe
categories: Information
comments: true
date: 2020-01-17T09:45:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- AWS
- Microsoft
- Windows
- Kubernetes
- Docker
- macOS
title: 'Technology Short Take 123'
url: /2020/01/17/technology-short-take-123/
---

Welcome to Technology Short Take #123, the first of 2020! I hope that everyone had a wonderful holiday season, but now it's time to jump back into the fray with a collection of technical articles from around the Internet. Here's hoping that I found something useful for you!<!--more-->

## Networking

* Eric Sloof [mentions][link-27] the NSX-T load balancing encyclopedia (found [here][link-28]), which intends to be an authoritative resource to NSX-T load balancing configuration and management.
* David Gee has an interesting set of articles exploring service function chaining in service mesh environments ([part 1][link-30], [part 2][link-31], [part 3][link-32], and [part 4][link-33]).

## Servers/Hardware

* Frank Denneman has a nice article aimed at helping readers understand [PCIe device to NUMA Node Locality][link-1].
* Giuliano Bertello [introduces PowerVCF][link-6], a PowerCLI module aimed at interacting with the SDDC Manager and VMware Cloud Foundation (VCF) stack. (If you're not familiar with VCF, see [here][link-7].)
* [This][link-8] is pricey, but I can't say I don't want one.
* James Hamilton is back with a post on [the AWS Graviton2 ARM processors][link-11].

## Security

* On January 13, Brian Krebs [discussed][link-12] the critical flaw (a vulnerability in `crypt32.dll`, a core Windows cryptographic component) that was rumored to be fixed the next day (January 14) on the first "Patch Tuesday" of 2020. The next day, [the Microsoft Security Response Center confirmed the vulnerability][link-13]. Time to get patching, folks!
* It's good to see 1Password [add support for U2F security keys][link-24] for logging into your 1Password.com account via a web browser. Now I really want to see hardware security key support in the desktop and mobile apps!

## Cloud Computing/Cloud Management

* The Grafana folks have [introduced Tanka][link-2], a new tool aimed a simplifying the task of managing Kubernetes manifests. While I'm sure that the use of `jsonnet` _may_ appeal to some users, I'm not sure it's going to appeal to "the masses," so to speak.
* Eric Shanks has a good walkthrough on [setting up a Kubernetes cluster on AWS with the AWS cloud provider enabled][link-3]. It's a nice companion/supplement/partner article to [my own post on the same topic][xref-1] (which, although it specifically mentions 1.15, should still work with 1.16 and 1.17 as well).
* My colleague Dan Finneran discusses [designing and building HA Kubernetes clusters on bare metal][link-5].
* Here's [a list of Kubernetes tools][link-10].
* HashiCorp recently announced the expansion of their Terraform Module Registry to include providers (and renaming the service to the Terraform Registry). Read more details [here][link-19].
* Sam McGeown [explains][link-21] how he uses GitLab pipelines to deploy his Hugo-based site to AWS. Oh, and you might also enjoy Nicolas Fränkel's post outlining his [blogging stack and publishing process][link-22].
* How about a bash "wrapper" for working with AWS resources from the command line? [See here][link-23].
* Chris Evans posits that [AWS is not building hybrid cloud][link-26], and I must say I can't really disagree with his reasoning.

## Operating Systems/Applications

* As of January 14, Windows 7 is now End of Life (EOL), as pointed out [here][link-14] by Sergiu Gatlan.
* [This][link-15] is kind of cool.
* Casey McMullen takes readers through [a step-by-step guide to use Docker containers to do local development for PHP, MySQL, and Redis][link-16]. Readers can, of course, extrapolate from the concepts presented here to include other development environments. If you're a non-developer wondering why containers are getting so much attention, read this article and try to see it from a developer's perspective.
* Giovanni Collazo shares [how to configure iTerm2 to recognize macOS-specific keyboard shortcuts][link-18].
* Steve Sloka [talks about new features in the Contour 1.1 release][link-20].
* I'm just getting started learning Golang, and already other folks [are talking about moving to Rust][link-25]. I'm so far behind.

## Storage

Nothing this time around; I'll try to find some content for next time!

## Virtualization

* William Lam keeps everyone updated regarding [running ESXi on the 2019 Mac Pro][link-29].

## Career/Soft Skills

* Scott Driver [talks][link-4] about "the vCommunity" and some of the benefits that can be had, career-wise, from actively participating. Although Scott's article focuses on the VMware-centric community, this is true (in my opinion) of many different communities, including various open source communities---and, to be frank, the physical communities in which we live and work.
* Now that I've moved back onto macOS (see [here][xref-2] for the explanation), I'm also taking OmniFocus back up, and so I found [this article][link-9] on using OmniFocus' new Tags functionality helpful.
* Thomas Maurer has some great tips for [creating technical demos and presentations][link-17], although (I guess somewhat understandably given he works for Microsoft) the tips are a tad Windows-centric.

I guess that's all for now! Thanks for reading, and again I hope that you found something useful here. If you have feedback, suggestions for improvement, or just want to say hi, feel free to [hit me up on Twitter][link-99]. Thanks!

[link-1]: https://frankdenneman.nl/2020/01/10/pcie-device-numa-node-locality/
[link-2]: https://grafana.com/blog/2020/01/09/introducing-tanka-our-way-of-deploying-to-kubernetes/
[link-3]: https://theithollow.com/2020/01/13/deploy-kubernetes-on-aws/
[link-4]: https://virtualvt.wordpress.com/2020/01/13/why-do-we-vcommunity/
[link-5]: https://thebsdbox.co.uk/2020/01/02/Designing-Building-HA-bare-metal-Kubernetes-cluster/
[link-6]: https://blog.bertello.org/2020/01/introducing-powervcf/
[link-7]: https://www.cloudlabnode.com/articles/vmware-cloud-foundation-vcf-introduction/
[link-8]: https://linedock.co/products/macbook-13in-docking-station?variant=10786211233828
[link-9]: http://uncorrected.net/using-omnifocus-to-shape-your-day/
[link-10]: http://dockerlabs.collabnix.com/kubernetes/kubetools/
[link-11]: https://perspectives.mvdirona.com/2020/01/aws-graviton2/
[link-12]: https://krebsonsecurity.com/2020/01/cryptic-rumblings-ahead-of-first-2020-patch-tuesday/
[link-13]: https://msrc-blog.microsoft.com/2020/01/14/january-2020-security-updates-cve-2020-0601/
[link-14]: https://www.bleepingcomputer.com/news/microsoft/windows-7-reaches-end-of-life-tomorrow-what-you-need-to-know/
[link-15]: https://github.com/seemoo-lab/opendrop
[link-16]: https://medium.com/better-programming/php-how-to-run-your-entire-development-environment-in-docker-containers-on-macos-787784e94f9a
[link-17]: https://www.thomasmaurer.ch/2020/01/how-to-create-great-tech-demos-and-presentations/
[link-18]: https://gcollazo.com/making-iterm-2-work-with-normal-mac-osx-keyboard-shortcuts/
[link-19]: https://www.hashicorp.com/blog/announcing-providers-in-the-new-terraform-registry/
[link-20]: https://projectcontour.io/announcing-contour-1.1/
[link-21]: https://www.definit.co.uk/2020/01/using-gitlab-pipelines-to-deploy-hugo-sites-to-aws/
[link-22]: https://blog.frankel.ch/my-blogging-stack-publishing-process/
[link-23]: https://github.com/bash-my-aws/bash-my-aws/
[link-24]: https://blog.1password.com/introducing-support-for-u2f-security-keys/
[link-25]: http://networkstatic.net/getting-started-with-the-rust-programming-language/
[link-26]: https://www.architecting.it/blog/aws-not-hybrid-cloud/
[link-27]: https://www.ntpro.nl/blog/archives/3551-NSX-T-LB-Encyclopedia.html
[link-28]: https://communities.vmware.com/docs/DOC-40434
[link-29]: https://www.virtuallyghetto.com/2020/01/esxi-on-the-new-2019-apple-mac-pro.html
[link-30]: http://ipengineer.net/2019/11/pt1-composition-in-network-service-meshes/
[link-31]: http://ipengineer.net/2019/11/pt2-composition-in-network-service-meshes/
[link-32]: http://ipengineer.net/2019/11/pt3-composition-in-network-service-meshes/
[link-33]: http://ipengineer.net/2019/11/pt4-composition-in-network-service-meshes/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2019-08-14-setting-up-aws-integrated-kubernetes-115-cluster-kubeadm.md" >}}
[xref-2]: {{< relref "2019-12-30-new-year-new-adventure.md" >}}
