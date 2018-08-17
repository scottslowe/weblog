---
author: slowe
categories: Information
comments: true
date: 2018-08-17T09:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- Cisco
- Linux
- Kubernetes
- Azure
- AWS
title: 'Technology Short Take 103'
url: /2018/08/17/technology-short-take-103/
---

Welcome to Technology Short Take 103, where I'm back yet again with a collection of links and articles from around the World Wide Web (Ha! Bet you haven't seen that term used in a while!) on various technology areas. Here's hoping I've managed to include something useful to you!<!--more-->

## Networking

* Gerald Dykeman shares some information on [using Ansible to automate Cisco Meraki][link-11].
* If you're not familiar with VPCs and associated AWS constructs, you should read [this article][link-20]. It's really good.
* I can't say that I've ever thought about writing a CNI plugin in Bash, but apparently [it can be done][link-25].

## Servers/Hardware

Nothing this time around, sorry!

## Security

* Scott McCarty [explains sVirt and how it's used to isolate Linux containers][link-3]. You may also find [this related article][link-7] (linked within Scott's post) to be helpful.
* Andrew Martin has [a write-up with security recommendations for your Kubernetes clusters][link-23].

## Cloud Computing/Cloud Management

* Fellow Heptonian Chuck Ha [walks through some Kubernetes logs][link-6] to show how to use them to better understand the relationships between the various components.
* Gabe Monroy [introduces Dev Spaces for AKS][link-4], a developer productivity-focused feature that aims to make it easier for developers to build and troubleshoot microservices in a Kubernetes environment.
* Written in a somewhat entertaining format (gotta give them kudos for that), Gravitational has a post on [the intricacies of upgrading etcd beneath Kubernetes][link-5].
* Alex Komljen provides an overview of [using Cluster Autoscaler with a Kubernetes cluster on AWS][link-8].
* And in the event you're not really familiar with the various autoscaling aspects of Kubernetes (of which cluster autoscaling is just one), [this article][link-9] by Mohamed Ahmed provides a 101-level overview.
* The CNCF has a 4-part series taking a deeper look at some of the feature changes in Kubernetes 1.11. Check out these articles talking about [IPVS-based in-cluster load balancing][link-16], [CoreDNS][link-17], [dynamic kubelet configuration][link-18], and [resizing persistent volumes in Kubernetes][link-19].
* Nathaniel Avery [shares a tip on saving some money][link-22] by using the right servers on Azure.
* Here's [how to let Traefik run on worker nodes][link-23] in a Docker Swarm cluster.
* These users [did not have a good experience with AKS][link-24].

## Operating Systems/Applications

* Kushal Das (briefly) talks about [using Podman for containers][link-1]. Looks like Podman is still evolving pretty rapidly, but it may be worth giving a try.
* Nick Janetakis decides he wants to [benchmark Debian versus Alpine as a base Docker image][link-2] and shares what he found. (The reasons behind his decision are in the article.)
* YACIS (Yet Another Container Isolation Scheme) has arrived: [Nabla containers][link-12]. Like Google GVisor, I suspect this will see limited uptake (at least initially) since it requires a specific base image in order to work.
* I got bitten by [this issue][link-13] recently when doing some testing using Ansible and Jinja2; fortunately, using `-e ansible_python_interpreter=/usr/bin/python` on the Ansible command line fixed it for me.
* Prometheus is the second project to "graduate" within the CNCF; more details are [here][link-21].

## Storage

* This is from July 2017, but still applicable I think: Detectify has a [deep dive on access controls for Amazon's S3 object storage service][link-26], which increasingly has been a source of data breaches.

## Virtualization

Sorry, I didn't find anything to include this time. I'm sure there's a ton of great content out there, but none of it passed across my "desk." (If you were needing a sign of a shift in my focus, this should be it!)

## Career/Soft Skills

* I'll put this here since it kind of applies to lots of different technology areas, even though it is sort of networking-focused: Ben Cotton recently [shared 6 RFCs you should read][link-10]. This is helpful information for folks new to IT, for developers or systems-oriented folks who need a better understanding of some networking fundamentals.
* In recounting [his journey to the cloud][link-14], Stephen Manley shares a tidbit that I think it useful for folks in a similar situation: regardless of how technology changes, there's a piece of your experience that remains valuable, and you can build on that piece to create a bridge into a new area. For Stephen, it was data management. For you, it may be something else. (I love his closing statement!)
* Finally, I found this article from Scott Young on [increasing your focus][link-15] to be helpful. As an "information worker," our focus is most definitely one of our most valuable resources.

That's all for now, folks! Have a great weekend!

[link-1]: https://kushaldas.in/posts/using-podman-for-containers.html
[link-2]: https://nickjanetakis.com/blog/benchmarking-debian-vs-alpine-as-a-base-docker-image
[link-3]: http://crunchtools.com/what-is-svirt-and-how-does-it-isolate-linux-containers/
[link-4]: https://azure.microsoft.com/en-us/blog/introducing-dev-spaces-for-aks/
[link-5]: https://gravitational.com/blog/kubernetes-and-offline-etcd-upgrades/
[link-6]: https://dev.to/chuck_ha/reading-kubernetes-logs-315k
[link-7]: https://opensource.com/article/18/2/understanding-selinux-labels-container-runtimes
[link-8]: https://akomljen.com/kubernetes-cluster-autoscaling-on-aws/
[link-9]: https://medium.com/@Mohamed.ahmed/kubernetes-autoscaling-101-cluster-autoscaler-horizontal-pod-autoscaler-and-vertical-pod-2a441d9ad231
[link-10]: https://opensource.com/article/18/7/requests-for-comments-to-know
[link-11]: https://gdykeman.github.io/2018/07/15/meraki-security-policy/
[link-12]: https://nabla-containers.github.io/
[link-13]: https://dmsimard.com/2016/01/08/selinux-python-virtualenv-chroot-and-ansible-dont-play-nice/
[link-14]: https://medium.com/nuvoloso/how-i-found-my-path-to-the-cloud-539c30ca333a
[link-15]: https://www.scotthyoung.com/blog/2018/07/12/guide-to-focus/
[link-16]: https://kubernetes.io/blog/2018/07/09/ipvs-based-in-cluster-load-balancing-deep-dive/
[link-17]: https://kubernetes.io/blog/2018/07/10/coredns-ga-for-kubernetes-cluster-dns/
[link-18]: https://kubernetes.io/blog/2018/07/11/dynamic-kubelet-configuration/
[link-19]: https://kubernetes.io/blog/2018/07/12/resizing-persistent-volumes-using-kubernetes/
[link-20]: https://start.jcolemorrison.com/aws-vpc-core-concepts-analogy-guide/
[link-21]: https://www.cncf.io/announcement/2018/08/09/prometheus-graduates/
[link-22]: http://www.notyourdadsit.com/blog/2018/6/19/azure-tip-use-b-series-servers-for-cost-savings
[link-23]: https://kubernetes.io/blog/2018/07/18/11-ways-not-to-get-hacked/
[link-24]: https://movingfulcrum.com/horrors-of-using-azure-kubernetes-service-in-production/
[link-25]: https://www.altoros.com/blog/kubernetes-networking-writing-your-own-simple-cni-plug-in-with-bash/
[link-26]: https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/
