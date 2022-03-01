---
author: slowe
categories: Information
comments: true
date: 2022-02-28T17:30:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Envoy
- NSX
- macOS
- AWS
- Kubernetes
- Terraform
- Linux
title: 'Technology Short Take 152'
url: /2022/02/28/technology-short-take-152/
---

Welcome to Technology Short Take #152! Normally I'd publish a Technology Short Take in the morning on a Friday, but I really wanted to get this one out so I'm making it live late in the day on a Monday. Here's hoping I've included some content below that you find useful!<!--more-->

## Networking

* I was (and am) familiar with RFC 1918 and the concept of non-routable address spaces. However, I was not familiar with the term "bogons" to refer to such prefixes that should not be publicly routed. Thanks to [this article][link-1], that oversight is now corrected. Oh, and the article shares a handy Python script to help implement bogon filtering in NSX-T.
* Koyeb [describes][link-5], at a high level, the global networking stack for their serverless platform. Components involved include the open source Kuma service mesh (in turn leveraging Envoy), anycast BGP, and mutual TLS (mTLS).
* Ivan Pepelnjak does a great job of describing all the things you really shouldn't do (or don't really need to do) when [trying to deal with migrating container hosts in a data center fabric][link-12]. In truth, the answer is exactly as Ivan says at the top of the article: when it comes to migrating container hosts, you don't. (Seriously.)

## Servers/Hardware

* The Eclectic Light Company is back with an article on [how the M1's efficiency (E) cores win][link-6]. I must admit I am a bit surprised at how much improvement there seems to be between the M1 Pro's E cores and the original M1 E cores.
* William Lam takes a look at [some potentially interesting homelab hardware][link-14].

## Security

* If you aren't paying attention to the security of your CI/CD pipelines, you probably should. Read [this NCC Group article about some ways they've compromised CI/CD pipelines][link-11].
* Sometimes I read articles like [this one on UEFI-based rootkits][link-13] and I think about getting rid of all my electronics and moving deep into the wilderness to live off the land.
* Rory McCune [writes][link-17] about a CVE in the Linux kernel that could allow for container escape in Kubernetes.
* Teri Radichel [draws some connections between recent events in the Ukraine and cybersecurity][link-26].

## Cloud Computing/Cloud Management

* Anders Eknert shows how to do---or at least provides a framework on getting started with---dynamic policy composition with Open Policy Agent (OPA) in [this blog post from April 2021][link-3].
* Here's an article on [how to get started with OrgFormation][link-18].
* This workshop on [using Graviton2 on AWS][link-19] is pretty neat.
* Xavier Avrillier writes about how to workaround [a vSphere CSI controller CrashLoopBackoff][link-22] when using Cluster API for vSphere (CAPV).
* Jorge de la Cruz shares [how to install Kubernetes on Ubuntu 20.04][link-23]. Most of my readers are probably already familiar with this process, but I wanted to share it here just in case.
* Marc Brooker has a pair of articles on circuit breakers and retries that are, in my opinion, worth reading (part 1 on circuit breakers is [here][link-24], part 2 on retries is [here][link-25]). And when I say "circuit breakers," I'm not talking about your electrical panel, either. Read the articles if you're unfamiliar with the concept of circuit breakers in modern distributed systems.
* For folks who may be just getting started with Terraform, here's a tutorial on [how to deploy EC2 instances in AWS using Terraform][link-29].
* I just came across this relatively-new [EKS News site][link-30]. Have a look at [the archives][link-31] for links to all the issues of EKS News. It's a shame the site doesn't appear to have an RSS/Atom feed...

## Operating Systems/Applications

* [Here's][link-2] an article on using a GitOps approach for declaratively managing the configuration of a Kong API gateway.
* I don't have a use case for `htmlq` (which I learned about via [this article][link-8]), but it's so cool that I have it installed on my Mac anyway.
* Andrey Babushkin takes readers along on a journey of [using `mcrouter` with `memcached` on Kubernetes][link-9].
* A [redesign][link-15] for Apple's long-stagnant System Preferences app? Yes, please.
* This was [a pretty cool story][link-20].
* Read this article if you're interested in [deploying ArgoCD on AWS][link-21].

## Storage

* Jim Jones has an article on [getting started with S3-compatible object storage][link-7].
* I have to confess that prior to just a few days ago I was not familiar with the term "kibibyte." For those who may be in a similar situation, read [this][link-10].

## Programming

* Kamaleshree Nagaraj talks about [scaling microservices with gRPC][link-27]. I'm looking forward to part two of the series, which will discuss Envoy Proxy.

## Virtualization

* Normally Ivan Pepelnjak's articles land in the "Networking" section, but this time Ivan is squarely discussing virtualization when he talks about [running an Ubuntu VM on an M1-based Mac][link-28]. I guess this is just yet another example of how fluid the "boundaries" between technical disciplines has become.

## Career/Soft Skills

* I really enjoyed this post on [lessons learned by moving the office home][link-4]. There's several nuggets in here that I need to apply to my next office move (which will be happening in about a month or so, if I don't run into any more delays).
* I like [this idea][link-16] of thinking about an _intention_ instead of _resolutions_.

That's a wrap! I'll be back in a couple of weeks with another Technology Short Take; until then, keep exploring, keep learning, and keep sharing! If you want to contact me---perhaps to provide some feedback, suggest a site you've found helpful, or just to say hello---feel free to contact [me on Twitter][link-99] or hit me up in any one of a number of Slack communities. I'd love to hear from you.

[link-1]: https://blog.engyak.co/2022/01/bogons-and-how-to-leverage-public-ip.html
[link-2]: https://konghq.com/blog/gitops-for-kong-managing-kong-declaratively-with-deck-and-github-actions/
[link-3]: https://blog.styra.com/blog/dynamic-policy-composition-for-opa
[link-4]: https://originalgreen.org/blog/what-i-learned-by-moving-my
[link-5]: https://www.koyeb.com/blog/building-a-multi-region-service-mesh-with-kuma-envoy-anycast-bgp-and-mtls
[link-6]: https://eclecticlight.co/2022/01/03/power-frequency-management-how-m1-e-cores-win/
[link-7]: https://www.koolaid.info/getting-started-with-s3-compatible-object-storage/
[link-8]: https://linuxconfig.org/how-to-scrape-web-pages-from-the-command-line-using-htmlq
[link-9]: https://blog.flant.com/highly-available-memcached-with-mcrouter-in-kubernetes/
[link-10]: https://ozanerhansha.medium.com/kilobytes-vs-kibibytes-d77eb2ff6c2a
[link-11]: https://research.nccgroup.com/2022/01/13/10-real-world-stories-of-how-weve-compromised-ci-cd-pipelines/
[link-12]: https://blog.ipspace.net/2022/02/bgp-on-virtual-machines.html
[link-13]: https://www.darkreading.com/threat-intelligence/researchers-uncover-dangerous-new-firmware-level-rootkit
[link-14]: https://williamlam.com/2022/02/potentially-interesting-vmware-homelab-kits-for-2022.html
[link-15]: https://basicappleguy.com/basicappleblog/settingsapp
[link-16]: https://cate.blog/2022/01/31/the-intention/
[link-17]: https://blog.aquasec.com/cve-2022-0185-linux-kernel-container-escape-in-kubernetes
[link-18]: https://bahr.dev/2022/02/07/org-formation/
[link-19]: https://graviton2-workshop.workshop.aws/en/gettingstarted.html
[link-20]: https://spacelift.io/blog/tricking-postgres-into-using-query-plan
[link-21]: https://www.modulo2.nl/blog/argocd-on-aws-with-multiple-clusters
[link-22]: https://www.vxav.fr/2022-02-02-vsphere-csi-controller-crashloopbackoff-in-capv-cluster/
[link-23]: https://jorgedelacruz.uk/2022/01/31/how-to-install-kubernetes-on-ubuntu-20-04/
[link-24]: https://brooker.co.za/blog/2022/02/16/circuit-breakers.html
[link-25]: https://brooker.co.za/blog/2022/02/28/retries.html
[link-26]: https://medium.com/cloud-security/why-does-whats-happening-in-ukraine-matter-for-cybersecurity-3686efaa6223
[link-27]: https://www.thoughtworks.com/insights/blog/microservices/scaling-microservices-gRPC-part-one
[link-28]: https://blog.ipspace.net/2022/02/ubuntu-mac-m1.html
[link-29]: https://gmusumeci.medium.com/how-to-deploy-ec2-instances-in-aws-using-terraform-45e304230262
[link-30]: https://eks.news/
[link-31]: https://eks.news/archives/
[link-99]: https://twitter.com/scott_lowe
