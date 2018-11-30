---
author: slowe
categories: Information
comments: true
date: 2018-11-30T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- AWS
- Ubuntu
- Linux
- VMware
title: 'Technology Short Take 107'
url: /2018/11/30/technology-short-take-107/
---

Welcome to Technology Short Take #107! In response to my request for feedback in the last Technology Short Take, a few readers responded in favor of a more regular publication schedule even if that means the articles are shorter in length. Thus, this Tech Short Take may be a bit shorter than usual, but hopefully you'll still find something useful.<!--more-->

## Networking

* Matt Oswalt [shares some details][link-3] on how NRE Labs implements "curriculum-as-code."

## Servers/Hardware

* Christian Kellner [provides][link-1] a brief reminder that not all USB-C ports are Thunderbolt ports, and updates everyone on the status of `bolt` (Linux utility for working with Thunderbolt ports and peripherals).

## Security

* Troy Hunt has a good article on [security measures other than just passwords][link-11], explaining some of the differences between multi-factor authentication and multi-step authentication (for example). Highly recommended reading.

## Cloud Computing/Cloud Management

* Another post from Matt Oswalt, also related to the NRE Labs post I mentioned above, discusses [troubleshooting NGINX Ingress rewrites in Kubernetes][link-5].
* Arush Salil reviews [using ingress on Kubernetes with `cert-manager`][link-6].
* Michael Hausenblas has an informative article with [information on Kubernetes RBAC defaults][link-7].
* This post is a couple months old (a lifetime in the cloud-native world): [the AWS Service Operator][link-8] enables users/developers to consume a select set of AWS services via Kubernetes YAML manifests.
* And speaking of YAML manifests, I saw [this tool][link-9] mentioned somewhere among my various feeds. It's described as a "layering tool" that allows users to extend officially published YAML documents with local extensions/additions.
* There's some good stuff in Michael Hausenblas' [AppOps Reloaded #102][link-12] (as always).
* [AWS Outposts][link-19] is Amazon's move into the hybrid cloud market. It will be available next year, and will come in a "native AWS" flavor as well as a VMware flavor (see [William Lam's post][link-20] for more details on the VMware flavor). This is a pretty significant market move, and I believe this move impact the technology industry in a variety of ways. For those of us in the IT field, we are definitely living in interesting times.

## Operating Systems/Applications

* Konstantin Suvorov helps bring [readability to long Jinja2 chains][link-2] via YAML block scalar syntax. A simple tip, yes, but useful!
* Major Hayden provides some ways to [prevent Ubuntu 16.04 from starting daemons when a package is installed][link-10].
* Kevin Fleming discusses the idea of [using a systemd-nspawn container to help with system recovery][link-14]. I've been interested in the use of systemd-nspawn for a while, but this is an interesting use case I hadn't considered.

## Storage

* J Metz has launched his own, storage-focused "Short Takes" series; the first of these is found [here][link-4].

## Virtualization

* The big news in virtualization recently was [AWS' announcement of Firecracker][link-15], a lightweight VMM (virtual machine monitor) targeted at creating "microVMs" that are ideally suited for serverless environments. Apparently, Firecracker is part of the technology stack behind Lambda and Fargate. More information is also available via [Jeff Barr's blog post][link-16], the [Firecracker web site][link-17], and the [Firecracker GitHub repository][link-18].

## Career/Soft Skills

* This article by Phil Estes on [4 tips for learning Golang][link-13] may prove useful to folks who, like myself, are interested in becoming more fluent in Go.

In the immortal words of Porky Pig, th-th-that's all folks! As always, feel free to [hit me up on Twitter][link-99] with your feedback, suggestions, or corrections. Thanks for reading!

[link-1]: https://christian.kellner.me/2018/10/24/thunderbolt-port-guide-t480s-force-power/
[link-2]: https://ansibledaily.com/add-beauty-to-long-jinja2-chains/
[link-3]: https://networkreliability.engineering/2018/11/how-nre-labs-implements-curriculum-as-code/
[link-4]: https://jmetz.com/2018/11/storage-short-take-1/
[link-5]: https://keepingitclassless.net/2018/11/troubleshooting-nginx-ingress-rewrites-in-kubernetes/
[link-6]: https://blog.kubernauts.io/ingress-on-kubernetes-with-cert-manager-ad7d413d76f0
[link-7]: https://dev.to/mhausenblas/on-some-defaults-in-kubernetes-rbac-270l
[link-8]: https://aws.amazon.com/blogs/opensource/aws-service-operator-kubernetes-available/
[link-9]: https://github.com/google/kasane
[link-10]: https://major.io/2016/05/05/preventing-ubuntu-16-04-starting-daemons-package-installed/
[link-11]: https://www.troyhunt.com/beyond-passwords-2fa-u2f-and-google-advanced-protection/
[link-12]: https://tinyletter.com/mhausenblas/letters/appops-reloaded-102
[link-13]: https://opensource.com/article/18/11/learning-golang
[link-14]: https://opensource.com/article/18/11/systemd-nspawn-system-recovery
[link-15]: https://aws.amazon.com/blogs/opensource/firecracker-open-source-secure-fast-microvm-serverless/
[link-16]: https://aws.amazon.com/blogs/aws/firecracker-lightweight-virtualization-for-serverless-computing/
[link-17]: https://firecracker-microvm.github.io/
[link-18]: https://github.com/firecracker-microvm/firecracker
[link-19]: https://aws.amazon.com/outposts/
[link-20]: https://cloud.vmware.com/community/2018/11/28/vmware-cloud-aws-outposts-cloud-managed-sddc-data-center/
[link-99]: https://twitter.com/scott_lowe
