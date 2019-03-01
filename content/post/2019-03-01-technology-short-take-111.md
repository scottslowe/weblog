---
author: slowe
categories: Information
comments: true
date: 2019-03-01T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Terraform
- AWS
- HyperV
- Microsoft
- Linux
- Docker
- vSphere
- VMware
- Terraform
title: 'Technology Short Take 111'
url: /2019/03/01/technology-short-take-111/
---

Welcome to Technology Short Take #111! I'm a couple weeks late on this one; wanted to publish it earlier but work has been keeping me busy (lots and lots of interest in Kubernetes and cloud-native technologies out there!). In any event, here you are---I hope you find something useful for you!<!--more-->

## Networking

* Daniel Dib has [a great article][link-3] on how network engineers need to evolve. The network isn't going away, it's just changing.
* I referenced part 1 of Ajay Chenampara's series on the Ansible `network-engine` command parser back in [Technology Short Take 102][xref-1] (July of last year). I'm not sure how I missed that part 2 was published only 2 days later, so I'm rectifying that now. Go check out [part 2][link-13].
* Paul Fitzgerald shows [how to use Hyper-V and Docker to build a VyOS 1.2.0 ISO image][link-14]. ([VyOS][link-15] is an open source Linux-based network operating system.)
* Matt Cowger shares [how to use MetalLB with the Unifi USG][link-20] to take advantage of Kubernetes LoadBalancer functionality in your home lab. Nifty.

## Servers/Hardware

* James Hamilton is back with [a more in-depth look][link-10] at the components of the AWS Nitro System.
* I must say that my conclusions regarding the ThinkPad X1 Carbon mirror those found in [this post][link-12].

## Security

* Etiene Dalcol of Gruntwork has a three part series on automating HashiCorp Vault. Part 1 covers [auto-unsealing Vault][link-7]; part 2 covers [authenticating to Vault using instance metadata][link-8]; and part 3 discusses [authenticating to Vault using an IAM user or role][link-9].

## Cloud Computing/Cloud Management

* Stefan Büringer writes about how you [can implement "advanced" RBAC functionality in Kubernetes using Open Policy Agent (OPA)][link-1].
* Cormac Hogan is back to playing around with PKS, and he explains [how to review PKS logs and status][link-4].
* Kamesh Sampath talks about [configuring Knative auto-scaling][link-5] (mostly focusing on scale-to-zero).
* I'm not sure I would refer to using `kubeadm` to bootstrap a Kubernetes cluster as "the hard way," but if you're looking for a fairly detailed tutorial on using `kubeadm` to bootstrap a Kubernetes cluster, [this post][link-11] by Yair Etziony has quite a bit of information on the process.
* Forrest Brazeal hits the nail on the head with the assertion that [IAM is the real cloud lock-in][link-19].

## Operating Systems/Applications

* Oriol Tauleria has [a write-up on how to layout Terraform code][link-6] to accommodate a project as it scales. I like some of the ideas Tauleria presents and hope to be able to implement some of them soon in my own project(s).
* David Holder explores some thoughts around [efficiency gains from small(er) containers][link-17].
* Maish Saidel-Keesing lays out [his thoughts on the death of Docker][link-18]. In the past, I might have felt the same way. However, Docker's recent (seeming) pivot to focus on a paid desktop product might change things a pretty fair amount. Let's face it, Docker's hold wasn't on the back-end systems---it was on the developers who valued the workflow. Focusing on a paid desktop solution caters to that audience. Given that containerd seems to be winning on the back-end, this allows Docker to remain influential in the container space, in my opinion.
* Folks running Fedora who have work/corporate VPNs that muck up their DNS settings might be interested in [this article on the DNSMasq plugin for Network Manager][link-21].

## Storage

Nothing this time around, but I'll stay alert for items to add next time.

## Virtualization

* William Lam talks about [the ESXi native driver for USB NIC][link-16], a Fling that will enable ESXi support for three of the most popular USB NIC chipsets (see the article for the specific chipsets).

## Career/Soft Skills

* You may have heard of Wardley mapping, a way of understanding context and situational awareness. If you're looking for a reasonably gentle introduction to the concept, check out [this article][link-2].
* Readers may find [this list of recommended books][link-22] from Jessie Frazelle of interest.

OK, that's all for now. [Hit me up on Twitter][link-99] if you have any comments, questions, suggestions, or corrections---I'd love to hear from you!

[link-1]: https://itnext.io/kubernetes-authorization-via-open-policy-agent-a9455d9d5ceb
[link-2]: https://realtimeboard.com/blog/wardley-maps-whiteboard-canvas/
[link-3]: http://lostintransit.se/2019/02/04/sdn-ate-my-hamster/
[link-4]: https://cormachogan.com/2019/02/05/reviewing-pks-logs-and-status/
[link-5]: https://medium.com/@kamesh_sampath/knative-as-a-pod-time-machine-3c1ca0cfb48a
[link-6]: https://medium.com/@uri.tau/terraform-layout-be3674dfe657
[link-7]: https://blog.gruntwork.io/a-guide-to-automating-hashicorp-vault-1-auto-unsealing-b219970f02c6
[link-8]: https://blog.gruntwork.io/a-guide-to-automating-hashicorp-vault-2-authenticating-with-instance-metadata-c3f9eaeaba53
[link-9]: https://blog.gruntwork.io/a-guide-to-automating-hashicorp-vault-3-authenticating-with-an-iam-user-or-role-a3203a3ee088
[link-10]: https://perspectives.mvdirona.com/2019/02/aws-nitro-system/
[link-11]: https://medium.com/polarsquad/how-to-bootstrap-kubernetes-the-hard-way-ca7ca46381f5
[link-12]: https://www.secretfader.com/blog/2019/02/lenovo-thinkpad-x1-carbon-all-business/
[link-13]: https://termlen0.github.io/2018/07/15/observations/
[link-14]: https://pgfitzgerald.wordpress.com/2019/02/11/how-to-build-a-vyos-1-2-0-iso-image/
[link-15]: https://vyos.io
[link-16]: https://www.virtuallyghetto.com/2019/02/esxi-native-driver-for-usb-nic-fling.html
[link-17]: https://www.virtualthoughts.co.uk/2019/02/11/efficiency-gains-from-smaller-containers/
[link-18]: https://technodrone.blogspot.com/2019/02/goodbye-docker-and-thanks-for-all-fish.html
[link-19]: https://forrestbrazeal.com/2019/02/18/cloud-irregular-iam-is-the-real-cloud-lock-in/
[link-20]: https://blog.cowger.us/2019/02/10/using-metallb-with-the-unifi-usg-for-in-home-kubernetes-loadbalancer-services.html
[link-21]: https://fedoramagazine.org/using-the-networkmanagers-dnsmasq-plugin/
[link-22]: https://blog.jessfraz.com/post/books/
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-07-13-technology-short-take-102.md" >}}
