---
author: slowe
categories: Information
comments: true
date: 2019-11-22T11:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- VMware
- Kubernetes
- Linux
- AWS
- Terraform
- vSphere
- HyperV
title: 'Technology Short Take 120'
url: /2019/11/22/technology-short-take-120/
---

Welcome to Technology Short Take #120! Wow...hard to believe it's been almost two months since the last Tech Short Take. Sorry about that! Hopefully something I share here in this Tech Short Take is useful or helpful to readers. On to the content!<!--more-->

## Networking

* In the event you are seeking more information on NAT hole punching, [here you go][link-7]. You're welcome. (I found this link in the serverless mullet architectures post, see the "Operating Systems/Applications" section below.)
* Mohamad Alhussein shares [how to filter the transit subnets from being redistributed from NSX-T to upstream physical routers][link-14] (by transit subnets Alhussein is referring to the NSX-managed subnets used to connect logical routers together).
* Ahmet Alp Balkan shows [how to use `mitmproxy` to inspect `kubectl` traffic][link-18]. I'm now inspired to go do this myself and see what knowledge I can gain.
* Corey Quinn proclaims "BGP is a hot mess"; Ivan Pepelnjak [lays out some facts][link-27].

## Servers/Hardware

I don't have anything to share this time around, but I'll stay alert for content to include future Tech Short Takes.

## Security

* [This basic introduction][link-1] of `firewalld` as found in CentOS 8 may prove useful to some readers. I've been messing around with `firewalld` ever since I switched to Fedora on the desktop, and found this article to be one of the better articles I've seen on the topic.
* Omer Levi Hevroni discusses [how to keep your Kubernetes secrets secure in Git][link-8].
* Roman Sachenko discusses [some serverless security risks and how to mitigate them][link-24].

## Cloud Computing/Cloud Management

* Marc Boorshtein has [a write-up on authentication in Kubernetes identity management][link-2]. I'm glad the author explicitly called out that Kubernetes doesn't directly connect to any kind of user store; this seems to be a point of confusion for new Kubernetes users with whom I've spoken.
* Yan Cui with Lumigo [tackles the concept of serverless vendor lock-in][link-6]. I have to say, I agree with a lot of the statements Cui makes regarding lock-in, coupling, risk, and the role of data in lock-in. Much of what's said in this article applies to _all_ forms of "lock-in," to be honest.
* I'm glad I read [this article][link-9] from Michael Gasch. I understand why it's titled the way it is, but the title does not do the _excellent_ content justice. If you're seeking to deepen your understanding of the Kubernetes architecture, I can heartily recommend this article. (Not to mention there's a great set of links at the end with even more information.)
* Jack Lindamood writes about how he [regrets switching from CloudFormation to Terraform][link-12], and shares lessons learned---both good and bad---about each of these two options for infrastructure as code.
* I love [this article][link-13] by Liz Fong-Jones. The sentence from which the title is taken captures it all: "Even the experts on the team were afraid to touch our Terraform configs, treating them like a haunted graveyard in which to seldom tread." The idea of using CI for infrastructure as code is something that's really been on my mind for the last few weeks, and so I'm glad I came across this article with lessons learned from the Honeycomb team.
* Ryan Matteson provides some very useful information on [creating Kubernetes manifests][link-19]. The "TL;DR" is that `kubectl explain` is one of your best friends.
* [This article by Alibaba Cloud][link-21] on how they scaled Kubernetes to 10,000 nodes has some useful information in it. It's extremely unlikely that any of the steps this team took would be needed by other organizations, but the information shared does help illuminate some of the inner workings of Kubernetes---and knowing more about Kubernetes can be helpful to anyone supporting or implementing it.
* I enjoyed this article on [describing fault domains][link-26] by Will Larson. In fact, I spent several hours a couple months ago playing around with `dot` to visually describe fault domains.

## Operating Systems/Applications

* Tim Wagner [discusses][link-3] _serverless mullet architectures._ Yes, you read that correctly---mullet as in "business in the front, party in the back", but applied to serverless application use cases. It's an interesting read. (And I also learned that mullet applies to house architectures, too!)
* The [3.4 release of etcd was announced][link-5] back at the end of August. This release improves stability and has some important fixes, including an important one related to `kube-apiserver` (see the last section of the post).
* Stefan Prodan's DZone article on [developing applications on multi-tenant clusters with Flux and Kustomize][link-10] is a pretty comprehensive article, but I almost feel like it tries to tackle too much in one article. I think I'd prefer to see multiple, smaller (and more focused) articles. If I can find some articles like this, I'll be sure to include them in future Tech Short Takes.
* William Lam shares how he stays sane with Slack by [automating the disabling of notifications using a private Slack API][link-15]. That's useful.
* Steve Sloka shares [how to run Contour on `kind`][link-20] (Kubernetes in Docker).
* Graham Barker shares [how to set up a simple `unbound` DNS server on PhotonOS][link-22] (as he points out, a good replacement for an expensive Windows Server instance).
* I love [this treatise on local-first software][link-23]. This resonates with me on so many levels.
* This is [a cool site][link-25].

## Storage

Nothing this time! If you happen to find something you think other readers would find useful, send it my way and I'll see about including it in a future Tech Short Take.

## Virtualization

* Anthony Spiteri explains how to [use variable maps to deploy vSphere VMs with Terraform][link-11].
* Ben Armstrong shares a PowerShell script that [finds Hyper-V VMs with missing virtual disks][link-28].

## Career/Soft Skills

* I'll put this here, since it's most closely aligned to career: Gustavo Franco and Matt Brown, both Customer Reliability Engineers with Google, [discuss potential SRE team organization][link-4]. I could see this article being helpful for organizations---or individuals---who are starting down the SRE path.
* So long, Datanauts! It turns out [the show is ending][link-16], and Nick Korte takes some time to write [a tribute to Datanauts][link-17].

That's all for now; I'll have more links and articles in the next Tech Short Take. Feel free to [contact me on Twitter][link-99] to share any feedback you may have on this or other content here on the site. Thanks for reading!

[link-1]: https://www.cyberciti.biz/faq/how-to-set-up-a-firewall-using-firewalld-on-centos-8/
[link-2]: https://www.linuxjournal.com/content/kubernetes-identity-management-authentication
[link-3]: https://read.acloud.guru/https-medium-com-timawagner-serverless-mullet-architectures-212487bc75c
[link-4]: https://cloudblog.withgoogle.com/products/devops-sre/how-sre-teams-are-organized-and-how-to-get-started/
[link-5]: https://kubernetes.io/blog/2019/08/30/announcing-etcd-3-4/
[link-6]: https://lumigo.io/blog/you-are-wrong-about-serverless-vendor-lock-in/
[link-7]: https://bford.info/pub/net/p2pnat/
[link-8]: https://learnk8s.io/kubernetes-secrets-in-git/
[link-9]: https://www.mgasch.com/post/k8sevents/
[link-10]: https://dzone.com/articles/developing-applications-on-multitenant-clusters-wi
[link-11]: https://anthonyspiteri.net/using-variable-maps-to-dynamically-deploy-vsphere-vms-with-terraform/
[link-12]: https://medium.com/@cep21/after-using-both-i-regretted-switching-from-terraform-to-cloudformation-8a6b043ad97a
[link-13]: https://www.honeycomb.io/blog/treading-in-haunted-graveyards/
[link-14]: http://www.vexpertconsultancy.com/2019/10/nsx-t-filter-route-re-destribution-of-transit-subnets-to-upstream-router/
[link-15]: https://www.virtuallyghetto.com/2019/10/automate-disabling-channel-here-notifications-using-private-slack-api.html
[link-16]: https://packetpushers.net/podcast/datanauts-173-goodnight-datanauts/
[link-17]: http://blog.thenetworknerd.com/2019/10/25/a-tribute-to-datanauts/
[link-18]: https://ahmet.im/blog/kubectl-man-in-the-middle/
[link-19]: https://prefetch.net/blog/2019/10/16/the-beginners-guide-to-creating-kubernetes-manifests/
[link-20]: https://projectcontour.io/kindly-running-contour/
[link-21]: https://www.alibabacloud.com/blog/how-does-alibaba-ensure-the-performance-of-system-components-in-a-10000-node-kubernetes-cluster_595469
[link-22]: https://virtualg.uk/setting-up-a-dns-server-with-photonos/
[link-23]: https://www.inkandswitch.com/local-first.html
[link-24]: https://hackernoon.com/severe-truth-about-serverless-security-and-ways-to-mitigate-major-risks-cd3i3x6f
[link-25]: https://rosettacode.org/wiki/Rosetta_Code
[link-26]: https://lethain.com//fault-domains/
[link-27]: https://blog.ipspace.net/2019/11/facts-and-fiction-bgp-is-hot-mess.html
[link-28]: https://american-boffin.com/2019/05/30/powershell-hack-finding-vms-with-missing-vhds/
[link-99]: https://twitter.com/scott_lowe
