---
author: slowe
categories: Information
comments: true
date: 2018-05-11T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Kubernetes
- AWS
- Linux
- vSphere
title: 'Technology Short Take 99'
url: /2018/05/11/technology-short-take-99/
---

Welcome to Technology Short Take 99! What follows below is a collection of various links and articles about (mostly) data center-related technologies. Hopefully something I've included will be useful. Here goes!<!--more-->

## Networking

* David Gee makes the connection between [coffee and network automation][link-5]. No, really. It's worth reading.
* Matt Oswalt, one of the co-authors of our recently-released network automation book from O'Reilly, recently tackled the topic of [running Kubernetes with Tungsten Fabric][link-14] (formerly known as OpenContrail). A network engineer using AWS and CloudFormation? Yep, get used to it folks---it's where the industry is headed.
* Vince Power [provides a high-level overview][link-22] of some of the key principles underlying Kubernetes networking.

## Servers/Hardware

Sorry, I don't have anything for you. Feel free to send me links you'd like me to consider for inclusion in the next Tech Short Take!

## Security

* Mike Foley talks about [new support for Virtualization Based Security (VBS) and Credential Guard][link-1] in vSphere 6.7.
* Chris Short examines [some of the security-related aspects][link-6] of the adoption of containers.
* The Kubernetes community reflects on [fixing the subpath volume vulnerability in Kubernetes][link-7].
* Michael Ducy of Sysdig [outlines a configuration][link-16] combining Sysdig Falco, NATS, and Kubeless for "active Kubernetes security." While the post is clearly Sysdig-oriented (as would be fully expected), it's also cool to see how powerful the assembly of various open source projects can be.

## Cloud Computing/Cloud Management

* Trond Hindenes shares a bit on how his company is [using Traefik as a Kubernetes ingress controller for both internal and external traffic][link-2].
* Typhoon, which describes itself as a "free and minimal Kubernetes distribution," has [announced support][link-3] for Typhoon on Fedora Atomic systems.
* I haven't tried it yet, but [Click][link-4] looks somewhat interesting.
* You may have noticed that Rancher Labs recently announced the GA of version 2.0 of Rancher. Check out [the announcement blog post][link-8] for more details.
* Alen Komlien discusses the idea of [a Kubernetes descheduler][link-11]. My take: "static" scheduling that occurs at the start of a pod's lifecycle is useful (and Kubernetes is doing reasonably well here), but "dynamic" scheduling that accounts for a greater portion of the pod's lifecycle _and_ the infrastructure underneath it is even more powerful. This is a lesson VMware learned years ago with Distributed Resource Scheduler (DRS).
* [This is a pretty in-depth article][link-18] (to me, at least), but it did help me better understand Custom Resource Definitions (CRDs) and the role of controllers in Kubernetes.

## Operating Systems/Applications

* Robert Paprocki of Kong discusses [how to design a scalable rate limiting algorithm][link-15], then proceeds to show how the Kong API gateway could be used to implement such an algorithm.
* As the use of APIs for everything increases, API tools like Postman become ever more useful---like this example of [using Postman to audit AWS infrastructure][link-17].
* Thomas Graf explains [why the Linux kernel community is replacing `iptables` with BPF][link-19]. He gives a great overview of BPF along the way, so if you're unfamiliar with BPF this may be a good read.
* This [practical introduction to container terminology][link-20] by Scott McCarty has a decidedly "Red Hat" feel to it, but is otherwise useful for folks who are new to the container space and need some terminology defined for them.
* Brendan Burns uses the term "serverless" in a slightly different way than it is commonly used; in [this article][link-21], he seems to use the term to refer primarily to "container-as-a-service"-type offerings---like Azure Container Instances (ACI) or AWS Fargate---instead of the more common link to functions-as-a-service. Along the way, he explains the virtual kubelet project as well, so if you're unfamiliar with that effort this article will help.

## Storage

Nothing this time around, but I'll see what I can find to include next time!

## Virtualization

* Nigel Poulton's quick review of [gVisor][link-13] (see [his thoughts here][link-12]) confirms my prediction some time ago that the lines between "VMs" and "containers" will continue to blur, and that we'll see a spectrum of isolation options emerging. Which isolation option should you use? Well, that will depend on what you're trying to achieve, right? _Right?_
* William Lam [discusses the new MAC learning functionality present in vSphere 6.7][link-23] which addresses some of the overhead of nested ESXi configurations.

## Career/Soft Skills

* Last week while at Interop ITX, I chatted with Keith Townsend regarding my recent career shift. If you've been wondering about _why_ I made this shift, give [the video of our chat][link-9] a look, and then feel free to [hit me up on Twitter][link-24].
* And speaking of career shifts, you might find Massimo's recent introspection of [his first 6 months at AWS][link-10] to be informative as well.

OK, that's it for now. As always, feel free to [hit me up on Twitter][link-24] if you have questions or suggestions for links I should consider including in future Technology Short Takes. Here's hoping you found something helpful!

[link-1]: https://www.yelof.com/2018/05/01/introducing-support-for-virtualization-based-security-and-credential-guard-in-vsphere-6-7/
[link-2]: https://medium.com/@trondhindenes/using-traefik-as-a-kubernetes-ingress-controller-for-both-internal-and-external-traffic-c06e4177314
[link-3]: https://typhoon.psdn.io/announce/#april-26-2018
[link-4]: https://databricks.com/blog/2018/03/27/introducing-click-the-command-line-interactive-controller-for-kubernetes.html
[link-5]: http://ipengineer.net/2018/04/describing-network-automation-automate-coffee/
[link-6]: https://www.devsecopsdays.com/articles/numbersdontlie
[link-7]: https://kubernetes.io/blog/2018/04/04/fixing-subpath-volume-vulnerability/
[link-8]: https://rancher.com/blog/2018/2018-05-01-rancher-ga-announcement-sheng-liang/
[link-9]: https://www.ctodose.com/blog/2018/5/8/beyond-virtualization-why-scott-lowe-joined-heptio
[link-10]: http://www.it20.info/2018/05/my-first-6-months-at-aws/
[link-11]: https://akomljen.com/meet-a-kubernetes-descheduler/
[link-12]: http://blog.nigelpoulton.com/gvisor-containers/
[link-13]: https://cloudplatform.googleblog.com/2018/05/Open-sourcing-gVisor-a-sandboxed-container-runtime.html
[link-14]: https://keepingitclassless.net/2018/05/up-running-kubernetes-tungsten-fabric/
[link-15]: https://konghq.com/blog/how-to-design-a-scalable-rate-limiting-algorithm/
[link-16]: https://sysdig.com/blog/active-kubernetes-security-falco-nats-kubeless/
[link-17]: http://blog.getpostman.com/2017/12/19/audit-your-aws-infrastructure-with-postman/
[link-18]: https://medium.com/@trstringer/create-kubernetes-controllers-for-core-and-custom-resources-62fc35ad64a3
[link-19]: https://cilium.io/blog/2018/04/17/why-is-the-kernel-community-replacing-iptables/
[link-20]: https://developers.redhat.com/blog/2018/02/22/container-terminology-practical-introduction/
[link-21]: https://thenewstack.io/the-future-of-kubernetes-is-serverless/
[link-22]: http://blog.wercker.com/how-does-kubernetes-work
[link-23]: https://www.virtuallyghetto.com/2018/04/native-mac-learning-in-vsphere-6-7-removes-the-need-for-promiscuous-mode-for-nested-esxi.html
[link-24]: https://twitter.com/scott_lowe
