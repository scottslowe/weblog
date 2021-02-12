---
author: slowe
categories: Information
comments: true
date: 2021-02-12T00:05:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Kubernetes
- Cilium
- macOS
- AWS
- vSphere
- Terraform
title: 'Technology Short Take 137'
url: /2021/02/12/technology-short-take-137/
---

Welcome to Technology Short Take #137! I've got a wide range of topics for you this time around---eBPF, Falco, Snort, Kyverno, etcd, VMware Code Stream, and more. Hopefully one of these links will prove useful to you. Enjoy!<!--more-->

## Networking

* Matt Oswalt [digs into eBPF][link-12].
* Justin Pietsch Has a lengthy article on why network engineers need to focus on [getting the network out of the way][link-27]. Although it is a lengthy read, it is---in my opinion---well worth the time to read it.

## Servers/Hardware

* I recently mentioned on Twitter that I was considering building out a new Linux PC to replace my aging Mac Pro (it's a 2012 model, so going on 9 years old). Joe Utter shared with me [his new lab build information][link-5], and now I'm sharing it with all of you. Sharing is caring, you know.

## Security

* Kaizhe Huang examines [Falco versus AuditD from a host intrusion detection system (HIDS) perspective][link-15].
* A major security flaw in `sudo` has been discovered; more details [available here][link-16].
* Eclypsium discusses ways of [protecting system firmware storage][link-29].
* I didn't even know that Snort was still a thing, but apparently [version 3 was recently released][link-30].

## Cloud Computing/Cloud Management

* I was going to put this under Security, but decided it fit here better---Michael Foster of StackRox (recently acquired by Red Hat) has been working on a CKS (Certified Kubernetes Security Specialist) certification study guide. I'll link you to [part 6][link-1], since it contains links to all the previous sections. I also like that StackRox has published [a GitHub repository][link-2] with the study materials as well.
* Sebastian Kurfürst shares some insights on [running Cilium in K3s and K3d on macOS][link-3] (for development purposes).
* David Anderson shares [some thoughts on what a better Kubernetes][link-4] might look like.
* If you're using AWS' AI or ML services, it may be worth reading [this post about opting out of data usage][link-6].
* Christian Posta talks a bit about some of the [challenges of adopting service mesh in enterprise organizations][link-7]. His recommendations do feel a bit biased (he recommends starting with an API gateway, such as the one his company provides), but it doesn't mean the recommendations are wrong. To summarize Christian's recommendations at the end: start small and expand slowly where and as it makes sense.
* Emre Odabas has [some "best practices" for the CKA exam][link-8].
* Chip Zoller has [a great article on some use cases for Kyverno][link-9].
* There is an absolute _wealth_ of information in [this etcd article][link-14] by Michael Gasch. Make time to read it. Seriously, it's that good.
* Daniele Ulrich has a four-part series (so far) on installing the TKG extensions ([part 1][link-17], [part 2][link-18], [part 3][link-19], and [part 4][link-20]).
* Mark Ukotic has some information on [using Kubernetes pipelines and tasks in VMware Code Stream][link-28].

## Operating Systems/Applications

* Turns out that the `apt-key` command on Debian and Debian derivatives (like Ubuntu and its derivatives) has been deprecated. [This article][link-10] walks users through how to work with OpenPGP repository signing keys without the use of `apt-key`.
* I recently watched [this YouTube video series on `tmux`][link-11] in order to get more familiar with this very popular tool. I can definitely see the value, but it's going to take me some time to adjust my habits and workflows to take advantage of `tmux`.
* Red Hat continues its effort to commodotize Docker's position with developers: this time by [taking aim at Docker Compose][link-31].

## Storage

* Here's [a post on OpenZFS Fusion Pool][link-22].
* Mark McBride shares his experience with [five years of Btrfs][link-23].

## Virtualization

* Here's an article on [deploying Kubernetes nodes on vSphere using Terraform][link-13].
* James Kiarie shows [how to create a VM template for KVM][link-24].
* Julia Evans spent some time with [Firecracker][link-26], and shares what she's learned [here][link-25].

## Career/Soft Skills

* Lee Briggs shares a great post on [learning to code with infrastructure as code][link-21] (using infrastructure as code is something I think is a good career move for pretty much everyone). I like how Lee shares some very specific recommendations on how folks can get started.

while I'd love to keep going, I'd better wrap it up here. If you have any feedback for me, feel free to hit [me on Twitter][link-99]. I'd love to hear from you.

[link-1]: https://www.stackrox.com/post/2021/01/cks-certification-study-guide-monitoring-logging-and-runtime-security/
[link-2]: https://github.com/stackrox/Kubernetes_Security_Specialist_Study_Guide
[link-3]: https://sandstorm.de/de/blog/post/running-cilium-in-k3s-and-k3d-lightweight-kubernetes-on-mac-os-for-development.html
[link-4]: https://blog.dave.tf/post/new-kubernetes/
[link-5]: https://non-descript.com/2020/12/11/new-lab-build-physical-build-info-2020-edition/
[link-6]: https://summitroute.com/blog/2021/01/06/opting_out_of_aws_ai_data_usage/
[link-7]: https://blog.christianposta.com/challenges-of-adopting-service-mesh-in-enterprise-organizations/
[link-8]: https://medium.com/@emreodabas_20110/best-practices-for-cka-exam-9c1e51ea9b29
[link-9]: https://neonmirrors.net/post/2021-01/kyverno-the-swiss-army-knife-of-kubernetes/
[link-10]: https://www.linuxuprising.com/2021/01/apt-key-is-deprecated-how-to-add.html
[link-11]: https://www.youtube.com/playlist?list=PLT98CRl2KxKGiyV1u6wHDV8VwcQdzfuKe
[link-12]: https://oswalt.dev/2021/01/introduction-to-ebpf/
[link-13]: https://virtjo.com/2021/deploy-k8s-vms-with-terraform-on-vsphere/
[link-14]: https://www.mgasch.com/2021/01/listwatch-part-1/
[link-15]: https://sysdig.com/blog/falco-vs-auditd-hids/
[link-16]: https://www.sudo.ws/alerts/unescape_overflow.html
[link-17]: https://vdan.niceneasy.ch/vmware-tanzu-basic-installing-tkg-extensions-1-2-0-part-1/
[link-18]: https://vdan.niceneasy.ch/vmware-tanzu-basic-installing-tkg-extensions-1-2-0-part-2/
[link-19]: https://vdan.niceneasy.ch/vmware-tanzu-basic-installing-tkg-extensions-1-2-0-part-3/
[link-20]: https://vdan.niceneasy.ch/vmware-tanzu-basic-installing-tkg-extensions-1-2-0-part-4/
[link-21]: https://www.leebriggs.co.uk/blog/2021/01/27/learn-to-code-with-iac.html
[link-22]: http://storagegaga.com/discovering-openzfs-fusion-pool/
[link-23]: https://markmcb.com/2020/01/07/five-years-of-btrfs/
[link-24]: https://www.tecmint.com/create-kvm-virtual-machine-template/
[link-25]: https://jvns.ca/blog/2021/01/23/firecracker--start-a-vm-in-less-than-a-second/
[link-26]: https://github.com/firecracker-microvm/firecracker/
[link-27]: https://elegantnetwork.github.io/posts/network-out-of-the-way/
[link-28]: https://blog.ukotic.net/2021/01/26/working-with-kubernetes-pipelines-tasks-in-vmware-code-stream/
[link-29]: https://eclypsium.com/2019/10/23/protecting-system-firmware-storage/
[link-30]: https://9to5linux.com/snort-3-open-source-intrusion-prevention-system-released-with-major-new-features
[link-31]: https://fedoramagazine.org/manage-containers-with-podman-compose/
[link-99]: https://twitter.com/scott_lowe
