---
author: slowe
categories: Information
comments: true
date: 2021-12-10T11:50:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- macOS
- Kubernetes
- AWS
- Pulumi
- Azure
- Automation
- Docker
- CLI
title: 'Technology Short Take 149'
url: /2021/12/10/technology-short-take-149/
---

Welcome to Technology Short Take #149! I'll have one more Technology Short Take in 2021, scheduled for three weeks from now (on the last day of the year!). For now, though, I have a small collection of articles and links for your reading pleasure---not as many as I usually include in a Technology Short Take, but better than nothing at all (I hope!). Enjoy!<!--more-->

## Networking

* Thomas Graf recently shared [how eBPF will eliminate sidecars in service mesh architectures][link-12] (he also announces the Cilium Service Mesh beta in the same post). I have many thoughts here---but I will reserve those thoughts until I've had time to do a bit more reading and research.
* Ivan tackles the topic of [CI/CD and testing in network automation][link-17].

## Security

* Dan Lorenc [dives deep into Fulcio][link-4].
* From the Not Surprised Department, some folks are starting to take a harder look at the timelines for security patches for older versions of macOS compared to newer versions. Ars Technica has [a write-up][link-7] on what's been observed so far.
* Rory McCune of Aqua [shares some new security-related features][link-11] in the Kubernetes 1.23 release.

## Cloud Computing/Cloud Management

* I shared this via Twitter as well, but this is so useful I wanted to include it here. Ivan Velichko wrote up [his container learning path][link-5] and shared it for others who may need to follow a similar path. I wish I'd had this years ago!
* Xavier Avrillier digs into the Cluster API Provider for vSphere in [this post][link-8].
* Baptiste Collard has a post on [Kubernetes controllers for AWS load balancers][link-3]. One takeaway from this post for me was that the new AWS load balancer controller uses a _ton_ of annotations.
* Tetrate has a post on [the new WASM-based extensions that are available in Istio 1.12][link-9].
* Michael Heap shares [how to deploy a Kong Gateway data plane with Pulumi][link-1].
* In [this post][link-6], Piotr talks about the "next big shift" in infrastructure as code, but what he's really discussing is [Crossplane][link-10]. I really dig Crossplane, but I don't know if I would refer to it as the "next big shift" in infrastructure as code.
* I was recently digging into OpenTelemtry a little bit and found [this page][link-13] helpful (among several others on this site).
* In the event you missed this in the barrage of AWS re:Invent announcements, I thought it was worth pointing out that [you can now use Amazon ECR Public to get Docker Official images][link-16].

## Operating Systems/Applications

* Olivier Miossec shows how to use container groups to [run multiple containers in a single Azure Container Instance][link-2].
* Running Kong Gateway or Kuma service mesh in your environment and also using Grafana? I recently learned that there are [a series of Grafana dashboards][link-14] you can use!
* [Here's a handy Bash tip][link-19] from Nick Janetakis.

## Virtualization

* Eric Sloof [drew my attention][link-15] to a VMware white paper on paravirtual RDMA devices on vSphere.
* James Hamilton discusses [running Xen on AWS Nitro to support legacy instances][link-18].

That's all for now. I hope you have a great weekend! If you have feedback for me, or if you just want to say hi, hit [me on Twitter][link-99] or find me on any of the various Slack communities I frequent ([the Kubernetes Slack community][link-98] is one great option). Thanks for reading!

[link-1]: https://konghq.com/blog/kong-gateway-pulumi/
[link-2]: https://dev.to/omiossec/using-azure-container-instance-with-multiple-containers-43fd
[link-3]: https://baptistout.net/posts/two-kubernetes-controllers-for-managing-aws-nlb/
[link-4]: https://chainguard.dev/posts/2021-11-12-fulcio-deep-dive
[link-5]: https://iximiuz.com/en/posts/container-learning-path/
[link-6]: https://hackernoon.com/infrastructure-as-code-the-next-big-shift-is-here
[link-7]: https://arstechnica.com/gadgets/2021/11/psa-apple-isnt-actually-patching-all-the-security-holes-in-older-versions-of-macos/
[link-8]: https://www.vxav.fr/2021-11-21-understanding-kubernetes-cluster-api-provider-vsphere-capv/
[link-9]: https://www.tetrate.io/blog/istio-wasm-extensions-and-ecosystem/
[link-10]: https://crossplane.io/
[link-11]: https://blog.aquasec.com/kubernetes-version-1.23-security-features
[link-12]: https://isovalent.com/blog/post/2021-12-08-ebpf-servicemesh
[link-13]: https://opentelemetry.lightstep.com/core-concepts/context-propagation/
[link-14]: https://grafana.com/orgs/konghq
[link-15]: https://www.ntpro.nl/blog/archives/3652-New-Technical-White-Paper-VMware-Paravirtual-RDMA-for-High-Performance-Computing.html
[link-16]: https://www.docker.com/blog/news-from-aws-reinvent-docker-official-images-on-amazon-ecr-public/
[link-17]: https://blog.ipspace.net/2021/12/ci-cd-network-automation-tests.html
[link-18]: https://perspectives.mvdirona.com/2021/11/xen-on-nitro-aws-nitro-for-legacy-instances/
[link-19]: https://nickjanetakis.com/blog/get-the-last-argument-of-the-last-run-command-in-your-shell-with-dollar-underscore
[link-98]: https://kubernetes.slack.com
[link-99]: https://twitter.com/scott_lowe
