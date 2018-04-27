---
author: slowe
categories: Information
comments: true
date: 2018-04-27T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NSX
- Linux
- Kubernetes
- Ansible
- Windows
- vSphere
- Vagrant
- Linux
title: 'Technology Short Take 98'
url: /2018/04/27/technology-short-take-98/
---

Welcome to Technology Short Take #98! Now that I'm starting to get settled into my new role at Heptio, I've managed to find some time to pull together another collection of links and articles pertaining to various data center technologies. Feedback is always welcome!<!--more-->

## Networking

* VMware has released a PowerCLI preview/fling for NSX-T; Kyle Ruddy has a write-up [here][link-2]. Looks like this preview provides some high-level cmdlets for NSX-T that weren't available before.
* Cilium, the open source project working to bring eBPF-powered networking and security to Kubernetes environments, has [hit the 1.0 release][link-16]. I will freely admit that I am a fan of what the Cilium folks are doing.
* What's that? Don't know what eBPF is? Or XDP? Not to worry, the nice folks over at Netronome have [a post that explains it all][link-17].
* People do all kinds of interesting things with Raspberry Pis; here's an article by Scott Helme on [using a Pi to secure DNS traffic][link-21].

## Servers/Hardware

* This is more of a follow-up to one of my own articles than a pointer to someone else's article. After continued use (including on business trips) of my Lenovo ThinkPad X1 Carbon running Fedora 27, I continue to be impressed by the battery life and overall performance and quality of the laptop. Well done, Lenovo!

## Security

* Michael Ducy [describes a setup][link-4] leveraging Falco and Fluentd (as part of the EFK stack) to do Kubernetes security logging.
* Sysdig has [a Kubernetes security guide][link-5] available that may be worth reviewing. I haven't dug into it yet (see what I did there?), so I can't validate how accurate/useful the recommendations may be.
* [This article on CloudFront hijacking][link-12] is worth a read, in my opinion.

## Cloud Computing/Cloud Management

* There's not much here yet, but it will be interesting to see how [this "K8s by example" GitBook][link-1]---which is, as the author admits, just a collection of various command-line snippets---evolves.
* Liz Rice has a nice write-up on [using Kubernetes in Vagrant with `kubeadm`][link-3].
* Gergely Nemeth shares how to leverage [skaffold][link-7] so you can [use Kubernetes for local development][link-6]. Skaffold is a tool I've been meaning to try out, so finding Gergely's post was helpful.
* Sandeep Dinesh provides [a high-level overview][link-9] of NodePort versus LoadBalancer versus Ingress in a Kubernetes environment.
* [Here's YAKI][link-14] (Yet Another Kubernetes Introduction). (Note: I reserve the right to write YAKI of my own at some point.)
* VMware has [launched a SIG][link-15] for Kubernetes on VMware.

## Operating Systems/Applications

* Tom Whiston has started what appears to be a series of blog posts about using Atomic Workstation, which takes the immutable infrastructure approach to Linux on a workstation (desktop or laptop). Part 1 is [available here][link-10].
* Given how useful I find `jq` (see [this post][xref-1] for more details), I thought [a YAML equivalent][link-13] would be super-handy. Alas, it just converts to JSON and pipes it to `jq`.
* Jeff Geerling shows [how to use Ansible's YAML callback plugin to create a better experience at the command line][link-18]. I've heard of callback plugins but wasn't sure how to use them, and Jeff's article was very helpful.
* I found [this article on the "end of Windows"][link-20] to be an interesting read. Your mileage may vary, of course.

## Storage

* The Kubernetes blog has some [more details][link-11] on the Container Storage Interface (CSI) in Kubernetes 1.10, which has moved from alpha to beta as part of the 1.10 release.

## Virtualization

* VMware recently GA'd vSphere 6.7, and---as would be expected---there's lots of blogger coverage. I can't possibly mention all of it here, but here are a few that jumped out at me: Andrea Mauro's [overview][link-22]; [this look][link-23] by Ward Vissers; Magnus Andersson's [quick review][link-24]; Simon Sharwood's [thoughts][link-25] over at El Reg; and David Stamen's [post][link-26]. For more vSphere 6.7 information, Eric Siebert has you covered with his [vSphere 6.7 link-o-rama][link-27].

## Career/Soft Skills

* Reading Noah Abraham's [experience as a new Kubernetes contributor][link-8] is helpful for folks like me (who are still getting their coding skills up to speed).
* [This approach][link-19] is interesting, to say the least.

OK then, let's call that a wrap! I hope that something I shared was useful, or at least caused you to think about something in a new way. Until next time, enjoy!

[link-1]: https://furikuri.gitbooks.io/k8s-by-example/content/
[link-2]: https://blogs.vmware.com/PowerCLI/2018/04/powercli-nsx-t-fling.html
[link-3]: https://medium.com/@lizrice/kubernetes-in-vagrant-with-kubeadm-21979ded6c63
[link-4]: https://sysdig.com/blog/kubernetes-security-logging-fluentd-falco/
[link-5]: https://sysdig.com/blog/kubernetes-security-guide/
[link-6]: https://nemethgergely.com/using-kubernetes-for-local-development/index.html
[link-7]: https://github.com/GoogleContainerTools/skaffold
[link-8]: https://infosiftr.com/2018/04/09/joining-kubernetes-release-team-completely-new-contributor/
[link-9]: https://medium.com/google-cloud/kubernetes-nodeport-vs-loadbalancer-vs-ingress-when-should-i-use-what-922f010849e0
[link-10]: https://www.nine.ch/en/engineering-logbook/dockerising-my-workstation-with-atomic-part-1
[link-11]: https://kubernetes.io/blog/2018/04/10/container-storage-interface-beta/
[link-12]: https://disloops.com/cloudfront-hijacking/
[link-13]: https://github.com/kislyuk/yq
[link-14]: https://medium.com/@nodearth/kubernetes-from-a-to-z-intro-ad425051cf59
[link-15]: https://blogs.vmware.com/cloudnative/2018/04/24/kubernetes-vmware-sig/
[link-16]: https://cilium.io/blog/2018/04/24/cilium-10
[link-17]: https://www.netronome.com/blog/bpf-ebpf-xdp-and-bpfilter-what-are-these-things-and-what-do-they-mean-enterprise/
[link-18]: https://www.jeffgeerling.com/blog/2018/use-ansibles-yaml-callback-plugin-better-cli-experience
[link-19]: https://vickylai.com/verbose/delete-old-tweets-ephemeral/
[link-20]: https://stratechery.com/2018/the-end-of-windows/
[link-21]: https://scotthelme.co.uk/securing-dns-across-all-of-my-devices-with-pihole-dns-over-https-1-1-1-1/
[link-22]: https://vinfrastructure.it/2018/04/vmware-vsphere-6-7-is-ga/
[link-23]: http://www.wardvissers.nl/2018/04/17/vmware-vsphere-6-7/
[link-24]: http://vcdx56.com/2018/04/vmware-vsphere-6-7-released/
[link-25]: https://www.theregister.co.uk/2018/04/17/vsphere_6_7/
[link-26]: http://davidstamen.com/2018/04/17/upgrading-to-vmware-vsphere-6.7/
[link-27]: http://vsphere-land.com/news/vsphere-6-7-link-o-rama.html
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
