---
author: slowe
categories: Information
comments: true
date: 2019-09-27T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Linux
- Terraform
- Pulumi
- Ansible
- Docker
title: 'Technology Short Take 119'
url: /2019/09/27/technology-short-take-119/
---

Welcome to Technology Short Take #119! As usual, I've collected some articles and links from around the Internet pertaining to various data center- and cloud-related topics. This installation in the Tech Short Takes series is _much_ shorter than usual, but hopefully I've managed to find something that proves to be helpful or informative! Now, on to the content!<!--more-->

## Networking

* Chip Zoller has a write-up on doing [HTTPS ingress with Enterprise PKS][link-1]. Normally I'd put something like this in a different section, but this is as much a write-up on how to configure NSX-T correctly as it is about configuring Ingress objects in Kubernetes.
* I saw [this headline][link-14], and immediately thought it was just "cloud native"-washing (i.e., tagging everything as "cloud native"). Fortunately, the diagrams illustrate that there _is_ something substantive behind the headline. The "TL;DR" for those who are interested is that this solution bypasses the normal `iptables` layer involved in most Kubernetes implementations to load balance traffic directly to Pods in the cluster. Unfortunately, this appears to be GKE-specific.

## Servers/Hardware

Nothing this time around. I'll stay tuned for content to include next time!

## Security

* The Kubernetes project recently underwent a security audit; more information on the audit, along with links to the findings and other details, is available [here][link-7].
* Daniel Sagi of Aqua Security explains the mechanics behind [a Pod escape using file system mounts][link-8].

## Cloud Computing/Cloud Management

* David Holder has a post on [using Terraform to create a highly available, cross-AZ etcd cluster][link-2].
* Anthony Spiteri shares [a Terraform configuration he created to deploy a sandbox Kubernetes cluster on VMware vSphere][link-3].
* Phoummala Schmitt talks about [the importance of tags with cloud resources][link-4]. Although her article is written specifically for Azure, the underlying concept of being sure to tag resources appropriately is valuable for any cloud provider.
* Mike Metral shows how to use Pulumi to [migrate workloads across EKS node groups with no downtime][link-11] (the "no downtime" part is subject to certain caveats and restrictions, largely driven by the nature of the application involved).
* Are you a (largely non-technical) manager seeking to better understand Kubernetes? (I'd be surprised if that was the case, since that's not my target audience.) Perhaps [this article][link-15] can help.

## Operating Systems/Applications

* Containous, the folks behind the Traefik ingress controller, [recently introduced Yaegi][link-5], a Go interpreter. I haven't yet had time to take a closer look at this, but based on what I've read so far this might be a useful tool to help accelerate learning Golang. Yaegi is [hosted on GitHub][link-6].
* From the same folks, we have [Maesh][link-10], a "simpler service mesh." I'm not sure "simple" and "service mesh" belong in the same sentence, but given that I haven't yet had the time to look into this more deeply I'll let it slide.
* Luc Dekens [shares][link-9] how to customize PowerShell to show things like the connected vSphere server, the Git repository or branch, the PowerCLI version, and more. Of course, Linux folks have been doing things like this with Bash for quite a while...
* Puja Abbassi, a developer advocate at Giant Swarm, discusses [the future of container image building][link-12] by looking at some of the concerns with the existing "Docker way" of building images.
* Jeff Geerling explains [how to test your Ansible roles with Molecule][link-13]. This is a slightly older post, but considering I found it useful I thought other readers might find it useful as well.

## Storage

I don't have any links to share this time, sorry!

## Virtualization

Nope, nothing here either. I'll stay alert for more content to include in the future.

## Career/Soft Skills

* [This checklist][link-16] is described as a "senior engineer's checklist," but it seems to be pretty applicable to most technology jobs these days.

See, I told you it was _actually_ a short take this time! I should have more content to share next time. Until then, feel free to [hit me up on Twitter][link-99] and share any feedback or comments you may have. Thanks, and have a great weekend!

[link-1]: https://www.sovsystems.com/blog/https-ingress-with-enterprise-pks
[link-2]: https://www.virtualthoughts.co.uk/2019/09/08/creating-a-highly-available-cross-az-loadbalanced-etcd-cluster-in-aws-with-terraform/
[link-3]: https://anthonyspiteri.net/deploying-a-kubernetes-sandbox-on-vmware-with-terraform/
[link-4]: https://techcommunity.microsoft.com/t5/ITOps-Talk-Blog/Tags-An-easy-way-to-gain-insight-into-your-cloud-resources/ba-p/846412
[link-5]: https://blog.containo.us/announcing-yaegi-263a1e2d070a
[link-6]: https://github.com/containous/yaegi
[link-7]: https://www.cncf.io/blog/2019/08/06/open-sourcing-the-kubernetes-security-audit/
[link-8]: https://blog.aquasec.com/kubernetes-security-pod-escape-log-mounts
[link-9]: http://www.lucd.info/2019/09/05/at-your-fingertips/
[link-10]: https://mae.sh/
[link-11]: https://www.pulumi.com/blog/day-2-kubernetes-migrating-eks-nodegroups-with-zero-downtime/
[link-12]: https://blog.giantswarm.io/what-is-the-future-of-container-image-building/
[link-13]: https://www.jeffgeerling.com/blog/2018/testing-your-ansible-roles-molecule
[link-14]: https://cloudblog.withgoogle.com/products/containers-kubernetes/container-native-load-balancing-on-gke-now-generally-available/
[link-15]: https://unixism.net/2019/08/a-managers-guide-to-kubernetes-adoption/
[link-16]: https://littleblah.com/post/2019-09-01-senior-engineer-checklist/
[link-99]: https://twitter.com/scott_lowe
