---
author: slowe
categories: Information
comments: true
date: 2019-06-21T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- Kubernetes
- Terraform
- Pulumi
- Fusion
- macOS
- vSphere
- Automation
- Encryption
- Docker
- Terraform
- Pulumi
- SSH
- Ansible
title: 'Technology Short Take 115'
url: /2019/06/21/technology-short-take-115/
---

Welcome to Technology Short Take #115! I'm back from my much-needed vacation in Bali, and getting settled back into work and my daily routine (which, for the last few weeks, was mostly swimming in the pool and sitting on the beach). Here's a fresh new collection of links and articles from the around the web to propel myself back into blogging. I hope you find something useful here!<!--more-->

## Networking

* Mohamad Alhussein shares information on [how to add a floating static route to an NSX edge via the NSX REST API][link-1].
* Mircea Ulinic shows readers [how to use `salt-sproxy` to take a different approach to network automation using Salt][link-15]. Normally this would require the use of Proxy Minions, but Ulinic's post on `salt-sproxy` shows how this can be done without any Proxy Minions.
* Michael Kashin has published a couple of posts on a project of his called NaaS (Network-as-a-Service). Part 1 is [here][link-26], and part 2 is [here][link-27]. These two articles are interesting (to me) because they combine both network automation _and_ Kubernetes. Nifty! I'm looking forward to seeing how NaaS evolves.
* David Holder walks through [removing unused load balancer IP allocations in NSX-T when used with PKS][link-29]. Although I believe David's post focuses on Enterprise PKS, it may also apply to NSX-T when integrating with "generic" Kubernetes as well. (I haven't tested it.)

## Servers/Hardware

Nothing this time around, sorry!

## Security

* Software company Agile Bits [recently announced][link-23] support for U2F-compatible hardware security keys in their 1Password product. Currently, the support is limited to the web interface of 1Password and only in specific browsers, but it would not be unreasonable to see the support expand in the future.
* Vivek Gite shows to [how use `oathtool` to generate time-based one-time passwords for use with 2FA systems][link-24] (in the article Google is the example service being secured, but instructions are provided near the end for working with other online services as well).
* Although [this article][link-30] is titled "How to use OpenSSL," it's really more of an educational article on hashes, digital signatures, and digital certificates, with some `openssl` commands thrown in along the way. It's a misleading title, but the content differs from the title in a good way.

## Cloud Computing/Cloud Management

* My teammate Duffie Cooley has a post on [how to use KinD (Kubernetes-in-Docker) to test a PR for Kubernetes][link-4]. Cool stuff.
* David Holder walks readers through the steps for [bootstrapping Prometheus, Grafana, and AlertManager in Enterprise PKS (Kubernetes) clusters][link-5]. He's using some Enterprise PKS-specific stuff, so keep in mind it won't apply to more "generic" Kubernetes environments.
* The folks at Pulumi recently added support for Terraform remote state; check out this blog post on [using Terraform remote state with Pulumi][link-6].
* Nick Korte walks readers through [creating a Wavefront proxy in AWS][link-8].
* Kief Morris, the author behind the O'Reilly _Infrastructure as Code_ book (see my review here) has launched [a website that provides infrastructure patterns][link-11] (re-usable frameworks for creating infrastructure).
* Joshua Sheppard spins [a cautionary tale about adopting Kubernetes][link-12]. As Sheppard mentions in his article, the project did suffer from some scope creep, and it sounds like maybe tools like `kubeadm` (which can help address some of the certificate management concerns breached in the article) weren't used. All in all, this article should remind readers that _any_ large software project---whether it be implementing Kubernetes or migrating applications to the public cloud---needs good planning and good oversight, and won't be as easy as the blogs make it seem to be.
* Kayan Azimov has a write-up on [using `terraform` and `kubeadm` to stand up a highly-available Kubernetes cluster on AWS][link-14]. I liked Azimov's use of AWS SSM to store PKI artifacts; that's a handy trick I'll have to try myself. I didn't like that Azimov used `cfssl` to generate the PKI artifacts instead of just using `kubeadm`, or that he ran etcd as a systemd unit instead of as a static pod. Still, it's a nice walk-through that can most certainly be used as a learning exmaple.
* My teammate Jim Weber has a nice post on [the TokenReview API in Kubernetes][link-28].

## Operating Systems/Applications

* Ajay Chenampara shows [how to add a custom credentials that stores SSH private keys into Ansible Tower][link-9]. The use git, in this instance, is cloning a private Git repository via an Ansible playbook.
* Systango has [this high-level overview][link-10] of serverless application architecture along with some pros/cons, use cases, etc.
* Marko Lukša shares [a nifty Bash trick to repeat a command until it works][link-13].
* Bryan Culver of Network to Code has [a 101-level primer on Ansible Vault][link-16].
* Based on [this article][link-25], it looks like the primary benefit of using Universal Base Images is that you can base it on RHEL without having to be a Red Hat customer. Is there something else I'm missing?

## Storage

* Cormac Hogan has recently published three good articles on storage in Kubernetes (the articles are all part of a larger "Kubernetes Storage on vSphere" series). The [first article][link-17] covers StatefulSets with a focus on PersistentVolumes (PVs) and PersistentVolumeClaims (PVCs). The [second article][link-18] covers failure scenarios with a focus on node failure/removal, and the [third article][link-19] discusses ReadWriteMany PVs using NFS. Good stuff!
* Eli Finkelshteyn explains why [data moats are not just about the data][link-20] (they also about creating a data-driven culture).

## Virtualization

* Wil van Antwerpen talks about [how to create a macOS Catalina VM using VMware Fusion][link-2].
* Myles Gray shows [how to use `cloud-init` for VM templating on vSphere][link-3]. I haven't had the chance to fully dig into this yet, but I'm a big fan of `cloud-init` and have long lamented that it didn't work on vSphere (or didn't seem to, anyway).
* Simon Long is [launching a podcast called The VCDX Podcast][link-7], focused on providing information pertinent to folks pursuing the VCDX certification.

## Career/Soft Skills

* This doesn't really fit anywhere else, but it's such a good article on network effect that I felt like it would be useful. Ali Yahya's article on [robot hiveminds and network effects][link-21] does a great job, I think, of explaining network effect and defensibility through a real-world example.
* Although this article is focused on [teaching a kid to build a game in Python using Pygame Zero][link-22], I think some of the takeaways listed near the end could be applicable to anyone learning a complex new skill.

I have plenty more material I could include, but I'll stop here so as to not overwhelm the readers (this is a lot of material to digest!). If you have any questions about any of these links, or comments about this or other articles on my site, you're always welcome to interact with me [via Twitter][link-99]. Have a great weekend, all!

[link-1]: http://www.vexpertconsultancy.com/2019/05/nsx-for-vsphere-add-floating-static-routes-to-nsx-edge-via-rest-api/
[link-2]: https://planetvm.net/blog/?p=64552
[link-3]: https://blah.cloud/infrastructure/using-cloud-init-for-vm-templating-on-vsphere/
[link-4]: https://mauilion.dev/posts/kind-k8s-testing/
[link-5]: https://www.virtualthoughts.co.uk/2019/05/14/bootstrapping-prometheus-grafana-and-alertmanager-to-pks-deployed-k8s-clusters/
[link-6]: https://blog.pulumi.com/using-terraform-remote-state-with-pulumi
[link-7]: https://www.simonlong.co.uk/blog/2019/05/03/welcome-to-the-vcdx-podcast/
[link-8]: http://blog.thenetworknerd.com/2019/05/25/creating-a-wavefront-proxy-in-aws/
[link-9]: https://termlen0.github.io/2019/06/08/observations/
[link-10]: https://www.systango.com/blog/serverless-architecture-smart-choice/
[link-11]: https://infrastructure-as-code.com/patterns/
[link-12]: https://sheppard.in/2019/kubernetes-a-cautionary-tale/
[link-13]: https://medium.com/@marko.luksa/bash-trick-repeat-last-command-until-success-750a61c43c8a
[link-14]: https://ifritltd.com/2019/06/16/automating-highly-available-kubernetes-cluster-and-external-etcd-setup-with-terraform-and-kubeadm-on-aws/
[link-15]: https://mirceaulinic.net/2019-06-17-minionless-salt-automation/
[link-16]: https://www.networktocode.com/blog/post/ansible-vault-primer/
[link-17]: https://cormachogan.com/2019/06/12/kubernetes-storage-on-vsphere-101-statefulset/
[link-18]: https://cormachogan.com/2019/06/18/kubernetes-storage-on-vsphere-101-failure-scenarios/
[link-19]: https://cormachogan.com/2019/06/20/kubernetes-storage-on-vsphere-101-readwritemany-nfs/
[link-20]: https://dzone.com/articles/data-moats-are-not-just-about-the-data
[link-21]: https://outlast.me/robot-hiveminds-with-network-effects/
[link-22]: https://www.mattlayman.com/blog/2019/teach-kid-code-pygame-zero/
[link-23]: https://blog.1password.com/introducing-support-for-u2f-security-keys/
[link-24]: https://www.cyberciti.biz/faq/use-oathtool-linux-command-line-for-2-step-verification-2fa/
[link-25]: https://developers.redhat.com/blog/2019/05/31/working-with-red-hat-enterprise-linux-universal-base-images-ubi/
[link-26]: https://networkop.co.uk/post/2019-06-naas-p1/
[link-27]: https://networkop.co.uk/post/2019-06-naas-p2/
[link-28]: https://jpweber.io/blog/a-look-at-tokenrequest-api/
[link-29]: https://www.virtualthoughts.co.uk/2019/06/21/removing-unused-pks-loadbalancer-ip-allocations-in-nsx-t/
[link-30]: https://opensource.com/article/19/6/cryptography-basics-openssl-part-2
[link-99]: https://twitter.com/scott_lowe
