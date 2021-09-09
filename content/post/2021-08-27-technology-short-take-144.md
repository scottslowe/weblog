---
author: slowe
categories: Information
comments: true
date: 2021-08-27T07:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- HyperV
- Microsoft
- Kubernetes
- Go
- macOS
- Apple
- JSON
- vSphere
- VMware
- Writing
- Intel
title: 'Technology Short Take 144'
url: /2021/08/27/technology-short-take-144/
---

Welcome to Technology Short Take #144! I have a fairly diverse set of links for readers this time around, covering topics from microchips to improving your writing, with stops along the way in topics like Kubernetes, virtualization, Linux, and the popular JSON-parsing tool `jq`. I hope you find something useful!<!--more-->

## Networking

* A short while ago I was helping someone (an acquaintance of a friend) with some odd DNS issues. I never found the root cause, but we did find a workaround; however, along the way, someone shared [this article][link-5] with me. I thought it was useful, so now I'm sharing it with you.
* Michael Kashin shares the journey of [containerizing NVIDIA Cumulus Linux][link-29].

## Servers/Hardware

* [Plastic microchips][link-2]? That's kind of cool.
* Kevin Houston [explores][link-31] multi-node servers as an alternative to blade servers due to increasing thermal requirements from CPUs. (And since Kevin didn't define TDP---shame, shame!---see [this post][link-32] for an explanation.)
* This is an interesting [deep dive into Intel's "Ice Lake" Xeon SP architecture][link-33].

## Security

* [A severity score of 9.9 out of 10 for a Hyper-V vulnerability][link-3]? Ouch.
* Dan Lorenc's [article on policy and attestations][link-7] does a great job of covering key concepts like signatures, attestations, and provenance. Well worth the read, in my opinion (unless you are already very well-versed in said concepts).
* Valentina Palmiotti discusses [finding a local privilege escalation in the Linux kernel via eBPF][link-9].
* Teri Radichel [uses some basketball analogies to explain][link-11] why defensive (proactive) security strategies are more desirable than reactive security strategies.
* Good to see [some Kubernetes hardening guidance][link-12] coming from the NSA/CISA.
* A bunch of home Wi-Fi routers are suspectible to attack; see [this article][link-20] for more details.
* Upgrading to Go 1.17 might be a good idea. See [here][link-21] for why.
* Sentinel Labs [outlines a major malware push][link-23] that is bypassing Apple's malware protections.

## Cloud Computing/Cloud Management

* I really enjoyed Evan Cordell's article on [16 things you didn't know about Kubernetes APIs and CRDs][link-5]. Good stuff.
* Pablo Vidal Bouza [discusses][link-8] Segment's move from SSH bastion hosts to AWS Systems Manager Session Manager. (I was going to make a joke about AWS Systems Manager Session Manager and Corey Quinn, but I couldn't come up with anything. I'll leave the humorous snark to Corey.)
* Luciano Mammino's article on [provisioning an Ubuntu-based EC2 instance with CDK][link-15] is a great introduction to CDK for those who aren't already familiar. Plus, I also learned about using SSM parameters to look up Ubuntu AMIs. That's really handy!
* Murat Celep (along with Andy Knapp) wrote an article on [using Prometheus and Grafana to visually expose Gatekeeper constraint violations][link-19].
* This article provides [some great "behind the scenes" information on AWS Lambda][link-25]. (Also, I didn't know anything about the galois field in AES; read [this][link-26] if you're curious.)

## Operating Systems/Applications

* This is a great article on [using `jq` with `kubectl`][link-1]. It's not bad as a general introduction to `jq`, either. (`jq` is probably one of my favorite CLI tools. So useful.)
* Bozhidar Batsov [shares the story][link-6] of how he left macOS for Linux and ended up on Windows 10 with WSL.
* I guess I'm on a bit of a `jq` kick this time around. Here's a cool article by Fabian Keller on [five useful `jq` commands for parsing JSON on the CLI][link-13]. I learned a couple of tricks from this article.
* Kudos to J. Austin Hughley for sticking it out through all the challenges and documenting [how to use a Windows gaming PC as a (Linux) Docker host][link-16].
* Alex Ellis shares some information on [how to use `kubectl` to access your private (Kubernetes) cluster][link-17]. Most of the information centers on the use of the Pro version of `inlets`, one of Alex's project. Nevertheless, there is some very useful information here.
* Blogger Mal shares [some "privacy surprises"][link-18] from well-known password manager 1Password.
* Jason Hall has some information on [OCI base image annotations][link-24].
* William Lam has a quick tip on [setting up Kubernetes using `containerd` on PhotonOS][link-30].

## Storage

* Chris Bergeron has an interesting (and kind of geeky) post on [connecting to a NAS with Thunderbolt][link-4].

## Virtualization

* Anthony Spiteri [looks at deploying KubeVirt with Platform9][link-10]. KubeVirt, if you're not aware, is a set of controllers and custom resources to allow Kubernetes to manage virtual machines (VMs).
* Rudi Martinsen has an article on [changing the Avi load balancer license tier][link-14] (this is in the context of using it with vSphere with Tanzu).
* Eric Sloof has information on [how to disable VMware plugins in vCenter Server][link-28] (the context of the article is security vulnerabilities disclosed in plugins).

## Career/Soft Skills

* Chad McElligott has a nice post on [resources for staying in touch with the tech community][link-22].
* Julia Evans has [a list of patterns in confusing explanations][link-27]. I'm sure I've done most (if not all) of these things at some point in time, but as Julia points out it's useful to have the list so that it becomes easier to avoid these mistakes in the future.

I guess I'd better wrap up now! I hope you found something useful here. If you have any questions, comments, suggestions for improvement, or just want to say hello, feel free to reach out to me. You can find [me on Twitter][link-99], and I'm also active in a number of different Slack communities (Kubernetes, Kuma, Envoy, and Pulumi, to name a few.) I'd love to hear from you!

[link-1]: https://medium.com/geekculture/my-jq-cheatsheet-34054df5b650
[link-2]: https://www.theverge.com/2021/7/23/22590001/arm-plasticarm-cheap-flexible-plastic-microchip-internet-of-everything
[link-3]: https://www.bleepingcomputer.com/news/security/critical-microsoft-hyper-v-bug-could-haunt-orgs-for-a-long-time/
[link-4]: https://chrisbergeron.com/2021/07/25/Ultra-fast-Thunderbolt-NAS-with-Apple-M1-and-Linux/
[link-5]: https://evancordell.com/posts/kube-apis-crds/
[link-6]: https://metaredux.com/posts/2021/07/31/back-to-linux.html
[link-7]: https://dlorenc.medium.com/policy-and-attestations-89650fd6f4fa
[link-8]: https://segment.com/blog/infrastructure-access/
[link-9]: https://www.graplsecurity.com/post/kernel-pwning-with-ebpf-a-love-story
[link-10]: https://anthonyspiteri.net/first-look-deploying-kubevirt-with-platform9/
[link-11]: https://medium.com/cloud-security/cloud-security-defensive-strategies-58d7079535d9
[link-12]: https://www.nsa.gov/News-Features/Feature-Stories/Article-View/Article/2716980/nsa-cisa-release-kubernetes-hardening-guidance/
[link-13]: https://www.fabian-keller.de/blog/5-useful-jq-commands-parse-json-cli/
[link-14]: https://rudimartinsen.com/2021/08/11/nsx-alb-change-license/
[link-15]: https://loige.co/provision-ubuntu-ec2-with-cdk/
[link-16]: https://www.starkandwayne.com/blog/converting-a-windows-gaming-pc-to-a-linux-docker-host/
[link-17]: https://blog.alexellis.io/get-private-kubectl-access-anywhere/
[link-18]: https://sec.gd/blog/posts/1password-leak-takeover/
[link-19]: https://itnext.io/expose-open-policy-agent-gatekeeper-constraint-violations-with-prometheus-and-grafana-6b7ac92ea07f
[link-20]: https://www.tomsguide.com/news/arcadyan-router-malware
[link-21]: https://nvd.nist.gov/vuln/detail/CVE-2021-29923
[link-22]: https://chadxz.dev/staying-in-touch/
[link-23]: https://labs.sentinelone.com/massive-new-adload-campaign-goes-entirely-undetected-by-apples-xprotect/
[link-24]: https://github.com/imjasonh/ImJasonH/tree/main/articles/oci-base-image-annotations
[link-25]: https://www.bschaatsbergen.com/behind-the-scenes-lambda
[link-26]: https://samiam.org/galois.html
[link-27]: https://jvns.ca/blog/confusing-explanations/
[link-28]: https://www.ntpro.nl/blog/archives/3633-How-to-Disable-VMware-Plugins-in-vCenter-Server.html
[link-29]: https://networkop.co.uk/post/2021-05-cumulus-ignite/
[link-30]: https://williamlam.com/2021/07/quick-tip-setting-up-kubernetes-using-containerd-on-photon-os.html
[link-31]: http://www.bladesmadesimple.com/2021/08/are-multi-node-systems-a-better-option-than-blade-servers/
[link-32]: https://techgearoid.com/articles/what-is-tdp-in-cpu/
[link-33]: https://www.nextplatform.com/2021/04/19/deep-dive-into-intels-ice-lake-xeon-sp-architecture/
[link-99]: https://twitter.com/scott_lowe
