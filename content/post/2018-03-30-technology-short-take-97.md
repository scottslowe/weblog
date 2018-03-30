---
author: slowe
categories: Information
comments: true
date: 2018-03-30T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Istio
- Microsoft
- VMware
- NSX
- Docker
- Linux
- Vagrant
- Windows
- Azure
- VirtualBox
- AWS
title: 'Technology Short Take 97'
url: /2018/03/30/technology-short-take-97/
---

Welcome to Technology Short Take 97! This Tech Short Take marks the end of an era (sort of); it's the last Tech Short Take published while I'm a VMware employee (today is my last day; see [here][xref-1] for more details). But enough about me---let's talk some tech! This Short Take may be a bit longer than some, so buckle up.<!--more-->

## Networking

* Vadim Eisenberg has two articles that describe how to integrate external services into an Istio service mesh. The first article covers [integrating external web services][link-5]; the second article discusses [consuming external TCP services][link-6].
* Matt Oswalt tackles the topic of unit testing (specifically with regard to network automation) with a post on [unit testing JunOS with JSNAPy][link-11].
* This is an older post (more than 2 years old), but as far as I can tell the concepts are still quite applicable. Sean Howard talks about [using the NSX Service Compose to create a more elegant ruleset][link-14] for enforcing security policy.
* Jérôme Petazzoni [test drives AppSwitch][link-24], the "network stack from the future." I also recently received an invitation to give AppSwitch a look, so stay tuned for my feedback as well.
* Chris Young shares some details on a little side project he did to show [how to use the Arista eAPI to do dynamic VLAN assignment based on MAC address][link-26]. While this specific use case may not be something worth implementing, the example of how to use eAPI from Python may prove useful for other use cases.

## Servers/Hardware

* Thomas Maurer has [a "first impressions" post][link-20] on the Microsoft Surface Book 2. (I must admit I've been considering adding a Microsoft device to my collection.)

## Security

* Nitsan Bin-Nun of Twistlock provides [a deep dive on two recent (and severe) Kubernetes vulnerabilities][link-1]. I definitely recommend reviewing this post and taking action to ensure your Kubernetes deployments are not at risk.
* Dan Walsh has a bit of a rant on [SELinux and blocking access to the Docker socket][link-2].
* And while we're discussing the Docker socket, the inimitable Jessie Frazelle shares some useful details on [building container images securely on Kubernetes][link-4].
* And while I'm talking about Jessie Frazelle: check out [her guide to running Tor Socks5 and Privoxy containers][link-10]. (Yes, I know privacy and anonymity aren't the same as security...work with me here.)
* If you're running an etcd cluster, _please_ make sure to secure access to it. Giovanni Collazo [shares some disturbing information][link-3] about unsecured etcd clusters.
* I've long said that if you're going to use Docker, you need to exercise caution about blindly throwing some container on the "FROM" line of your `Dockerfile`. If you don't know how that image was built, this is no different than downloading a compiled executable from the Internet and running it. (Good luck with that.) In any case, this is something that [Tern is intended to help address][link-27].

## Cloud Computing/Cloud Management

* William Lam has a series of posts on VMware Pivotal Container Service (PKS), which at the time of this writing was up to 4 posts in the series. William provides an overview in [part 1][link-15], discusses client tools in [part 2][link-16], reviews NSX-T integration in [part 3][link-17], and actually kicks off the PKS installation with Ops Manager and BOSH in [part 4][link-18].
* Gerald Venzi shows [how to use Vagrant and VirtualBox to deploy a local Kubernetes cluster][link-25], using some Kubernetes support recently added to an Oracle GitHub repository of Vagrant boxes.
* Thorsten Hans walks through [upgrading a Kubernetes cluster on AKS using the Azure CLI][link-29]. I myself have done this a couple of times---it's really straightforward.
* Trying to decide between GKE, AKS, and EKS? Tirumarai Selvan has [a comparison][link-30].
* Richard Li [discusses][link-33] strategies for ingress in Kubernetes and the tradeoffs associated with each approach.

## Operating Systems/Applications

* Eben Freeman [shows how][link-7] to integrate Istio, Envoy, and Honeycomb for detailed application statistics.
* Antonio Murdaca outlines [how to use `kubeadm` to bootstrap a Kubernetes cluster with CRI-O][link-8] (instead of Docker). I haven't tried this myself yet---I'm waiting for CRI-O to get a bit more mature so that I don't have to compile it from source (although this _is_ an older article, so we might be there by now).
* If I ever get my hands on a Windows 10 box (I'm seriously considering adding a Windows 10 box to my home office setup), I'm going to try [this process][link-9]. Hmmm...maybe I could install Windows 10 on my quad-proc Mac Pro, then run macOS virtualized on it...that might be something to try one day (as if I'll ever have time for that!).
* Ansible 2.5 was recently released; check out [this post][link-12] detailing some of the new features and functionality.
* Dusty Mabe [talks about][link-13] using BTRFS snapshots to snapshot and rollback entire installations of Fedora, and (rightfully) mentions Atomic Workstation as a (probably) better alternative. Still, the idea is pretty cool.
* [Flatcar Linux][link-19] is a "friendly fork" of Container Linux (of CoreOS).
* Windows Server 2019 is now in preview (see [this blog post][link-21]), and will include Kubernetes support. Also see [this VentureBeat article][link-23].
* Nick Joyce walks through a few tips to build more [minimal Docker containers for Python applications][link-22].
* JJ Asghar provides some direct steps to [getting PowerCLI 10+ working on Ubuntu Linux][link-28].

## Storage

No links for you this time, but I'll stay alert for something to include next time. If you have links you'd like to see included in the next Technology Short Take, send them my way!

## Virtualization

Nothing this time around---it seems like all the "cool kids" are talking about cloud, containers, and Kubernetes these days! Don't worry, though, I'll keep my eyes peeled for some content to include here next time around.

## Career/Soft Skills

* Finding yourself procrastinating more than usual recently? There can be a lot of different reasons; [this post][link-31] by Leo Babauta explores some reasons and some antidotes for procrastination.
* Guess I should've read [this post by Greg Ferro][link-32] on why you will never make serious money working for a startup before I took a job at a startup. (Oops! That's a hint toward Monday's announcement!)

It's time to wrap up. Have a great weekend, everyone!

[link-1]: https://www.twistlock.com/2018/03/21/deep-dive-severe-kubernetes-vulnerability-date-cve-2017-1002101/
[link-2]: https://danwalsh.livejournal.com/78373.html
[link-3]: https://elweb.co/the-security-footgun-in-etcd/
[link-4]: https://blog.jessfraz.com/post/building-container-images-securely-on-kubernetes/
[link-5]: https://istio.io/blog/2018/egress-https.html
[link-6]: https://istio.io/blog/2018/egress-tcp.html
[link-7]: https://honeycomb.io/blog/2017/09/istio-envoy-and-honeycomb/
[link-8]: http://www.projectatomic.io/blog/2017/06/using-kubeadm-with-cri-o/
[link-9]: https://www.howtogeek.com/289594/how-to-install-macos-sierra-in-virtualbox-on-windows-10/
[link-10]: https://blog.jessfraz.com/post/tor-socks-proxy-and-privoxy-containers/
[link-11]: https://keepingitclassless.net/2018/02/unit-testing-junos-jsnapy/
[link-12]: https://www.ansible.com/blog/ansible-2.5-traveling-space-and-time
[link-13]: https://dustymabe.com/2017/12/17/fedora-btrfssnapper-the-fedora-27-edition/
[link-14]: http://nsxperts.com/?p=65
[link-15]: https://www.virtuallyghetto.com/2018/03/getting-started-with-vmware-pivotal-container-service-pks-part-1-overview.html
[link-16]: https://www.virtuallyghetto.com/2018/03/getting-started-with-vmware-pivotal-container-service-pks-part-2-pks-client.html
[link-17]: https://www.virtuallyghetto.com/2018/03/getting-started-with-vmware-pivotal-container-service-pks-part-3-nsx-t.html
[link-18]: https://www.virtuallyghetto.com/2018/03/getting-started-with-vmware-pivotal-container-service-pks-part-4-ops-manager-bosh.html
[link-19]: https://www.flatcar-linux.org
[link-20]: https://www.thomasmaurer.ch/2018/03/first-impressions-surface-book-2/
[link-21]: https://cloudblogs.microsoft.com/windowsserver/2018/03/20/introducing-windows-server-2019-now-available-in-preview/
[link-22]: https://blog.realkinetic.com/building-minimal-docker-containers-for-python-applications-37d0272c52f3
[link-23]: https://venturebeat.com/2018/03/20/windows-server-2019-will-feature-linux-and-kubernetes-support/
[link-24]: https://jpetazzo.github.io/2018/03/13/appswitch-hyperlay-network-stack-future/
[link-25]: https://geraldonit.com/2018/03/26/deploying-a-kubernetes-cluster-with-vagrant-on-virtual-box/
[link-26]: https://kontrolissues.net/2018/03/22/playing-with-arisa-eapi-dynamic-vlan-assignment-ish/
[link-27]: https://blogs.vmware.com/opensource/2018/03/13/open-source-tern-makes-containers-compliant/
[link-28]: http://jjasghar.github.io/blog/2018/03/22/powercli-10+-on-linux/
[link-29]: https://thorsten-hans.com/upgrading-a-kubernetes-cluster-on-aks-using-azure-cli-603c9be7b369
[link-30]: https://blog.hasura.io/gke-vs-aks-vs-eks-411f080640dc
[link-31]: https://zenhabits.net/antidotes/
[link-32]: http://etherealmind.com/will-never-make-serious-money-working-startup/
[link-33]: https://blog.getambassador.io/kubernetes-ingress-nodeport-load-balancers-and-ingress-controllers-6e29f1c44f2d
[xref-1]: {{< relref "2018-03-26-time-to-evolve.md" >}}
