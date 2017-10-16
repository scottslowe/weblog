---
author: slowe
categories: Information
comments: true
date: 2017-08-25T11:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Git
- AWS
- SSH
- Microsoft
- Azure
- Kubernetes
- Docker
- Linux
- Terraform
- Fedora
- VMware
- vSphere
- CLI
title: 'Technology Short Take 86'
url: /2017/08/25/technology-short-take-86/
---

Welcome to Technology Short Take #86, the latest collection of links, articles, and posts from around the web, focused on major data center technology areas. Enjoy!<!--more-->

## Networking

* Csilla Bessenyei has a [series of articles on step-by-step automation][link-11] that you might find helpful. I found the articles at step 5, [the benefits of Git for network engineers][link-10], but the previous four articles also look helpful.
* James Governor [provides a useful explanation][link-22] of a service mesh in the context of Istio and Linkerd; this is helpful for those of you out there who've heard the terms/names but haven't had time to really dig in and understand what's happening here.
* Speaking of service mesh: a couple related articles passed across my desk(top) recently. The first of these is an article on [using Traefik with Kubernetes and Let's Encrypt][link-24] (throwing in a Consul back-end for the fun of it). The second is [this post on Istio plus Linkerd][link-25], which talks about how to use Linkerd in conjunction with Istio to build a service mesh. (Do I need to talk about how understanding a service mesh is a good thing for networking professionals? That might be a good blog post topic right there!)

## Servers/Hardware

* Werner Ruegg has [a post][link-26] on an alternative hardware solution for vSphere home labs.

## Security

* RedLock shares some [guidelines around AWS access key deletion][link-6] (in the event that an access key gets compromised). Given the increasing reliance on AWS, good security posture is a must.
* Here's [a handy CLI tool][link-19] for working with AWS IAM configurations.
* Moritz Heiber of ThoughtWorks has a veritable treatise on [using AWS with security as a first-class citizen][link-20]. There is a _wealth_ of information in this lengthy post. I think I'm going to have to read it multiple times just to digest all the information here.
* Andson Tung [shares how][link-29] a Dirty Cow container exploit sticks around even after the container is destroyed. Good reason to make sure your systems and your images are patched!

## Cloud Computing/Cloud Management

* Diego Roberto dos Santos (via Fedora Magazine) [shared][link-4] a simple but useful trick for combining some Bash shell programming and the AWS CLI to upload the same SSH key to a list of AWS regions. I think this is a useful tip, though I will point out that someone on Twitter commented that this felt like using the same password everywhere. It's up to you to decide whether this is a security risk or not.
* Microsoft's Azure team is doing some very interesting (in my opinion) things. First, there is July's [announcement of Azure Container Instances (ACI)][link-16], which---as far as I've been able to tell---leverages hypervisor isolation for containers while offering Container-as-a-Service (CaaS) functionality. Add to that [an ACI connector for Kubernetes][link-17] that allows Kubernetes to use ACI underneath, and then add to that [a CNI plugin][link-18] to allow containers to connect to Azure VNETs. This is definitely something I'd like to explore further.
* Here's a new acronym I just coined: YAKD. It stands for Yet Another Kubernetes Deployer, and I thought this up after finding [this article on `kube-spawn`][link-23], a tool for starting up a local multi-node Kubernetes cluster (primarily for testing and development). I guess it's not really fair of me to make this statement, given that `kube-spawn` is focused on local Kubernetes clusters for testing and development. It does seem, though, that Kubernetes deployment tools abound.

## Operating Systems/Applications

* Here's [how to run Steam in a systemd-nspawn container][link-5].
* Toni Willberg provides a set of instructions for [running Fedora 26 on Azure][link-7].
* Shawn Hartsock shows [how to build SSHFS on Photon OS][link-9].
* Jeff Geerling---author of _Ansible for DevOps_---has [a post][link-12] on how various Ansible configuration files may conflict with one another.
* Need a comparison of CloudFormation versus Terraform? No worries, Andreas Wittig [has you covered][link-21]. (Personally, I'm a Terraform fan.)
* The Open Containers Initiative (OCI) has finally [released version 1.0][link-28] of its Runtime Specification (for building a container runtime) and Image Format Specification (for container images). It will be interesting now to see what (if anything) Docker does in response.

## Storage

Nothing this time around. Feel free to submit something for inclusion in the next Technology Short Take.

## Virtualization

* Emad Younis has a three-part series (so far) on vSphere 6.5 upgrade considerations ([part 1][link-13], [part 2][link-14], and [part 3][link-15]).
* Did you catch the official announcement about the future of vCenter Server for Windows? Martin Yip has all the details [here][link-27].

## Career/Soft Skills

* Julia Evans has a few nice tips on [learning at work][link-3].
* A number of folks have recently been jumping on the "static site" movement, moving away from WordPress and other platforms in favor of static site generators such as Jekyll or Hugo. Cody De Arkland is one, having recently [migrated his blog from WordPress to Hugo][link-8]. Grant Orchard also recently migrated to Jekyll as well. I love how folks are using these migrations as a vehicle for expanding their skill sets.

That's it for now. Next week is VMworld in Las Vegas, NV, and I'll be there live-blogging the keynotes (no sessions though, unfortunately---VMware employees generally can't get into sessions). However, I'll be around, so be sure to look me up if you're going to be there. Thanks for reading!



[link-1]: https://napalm-automation.net/yang-for-dummies/
[link-2]: https://rclone.org
[link-3]: https://jvns.ca/blog/2017/08/06/learning-at-work/
[link-4]: https://fedoramagazine.org/ssh-key-aws-regions/
[link-5]: http://ludiclinux.com/Nspawn-Steam-Container/
[link-6]: https://blog.redlock.io/aws-access-key-security-best-practices
[link-7]: http://www.willberg.fi/2017/07/running-fedora-26-on-azure.html
[link-8]: https://www.thehumblelab.com/migrating-to-hugo-aws/
[link-9]: https://medium.com/@hartsock/photon-os-with-sshfs-from-source-941a12cc554c
[link-10]: https://networkerandcoder.wordpress.com/2017/08/16/step-five-the-benefits-of-git-for-network-engineers/
[link-11]: https://networkerandcoder.wordpress.com/automation-step-by-step-series/
[link-12]: https://www.jeffgeerling.com/blog/2017/slow-ansible-playbook-check-ansiblecfg
[link-13]: https://blogs.vmware.com/vsphere/2017/05/vsphere-6-5-upgrade-considerations-part-1.html
[link-14]: https://blogs.vmware.com/vsphere/2017/07/vsphere-6-5-upgrade-considerations-part-2.html
[link-15]: https://blogs.vmware.com/vsphere/2017/08/vsphere-6-5-upgrade-considerations-part-3.html
[link-16]: https://azure.microsoft.com/en-us/blog/announcing-azure-container-instances/
[link-17]: https://github.com/Azure/aci-connector-k8s
[link-18]: https://github.com/azure/azure-container-networking
[link-19]: https://github.com/99designs/iamy
[link-20]: https://www.thoughtworks.com/insights/blog/using-aws-security-first-class-citizen
[link-21]: https://cloudonaut.io/cloudformation-vs-terraform/
[link-22]: https://redmonk.com/jgovernor/2017/05/31/so-what-even-is-a-service-mesh-hot-take-on-istio-and-linkerd/
[link-23]: https://kinvolk.io/blog/2017/08/introducing-kube-spawn-a-tool-to-create-local-multi-node-kubernetes-clusters/
[link-24]: https://blog.deimos.fr/2017/08/20/kubernetes-with-traefik-and-lets-encrypt/
[link-25]: https://buoyant.io/2017/07/11/linkerd-istio/
[link-26]: http://blog.rueegg.com/?p=94
[link-27]: https://blogs.vmware.com/vsphere/2017/08/farewell-vcenter-server-windows.html
[link-28]: https://www.opencontainers.org/announcement/2017/07/19/open-container-initiative-oci-releases-v1-0-of-container-standards
[link-29]: https://dzone.com/articles/a-dirty-cow-container-exploit-sticks-around-even-a
