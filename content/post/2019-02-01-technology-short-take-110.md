---
author: slowe
categories: Information
comments: true
date: 2019-02-01T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Cilium
- Kubernetes
- Azure
- Microsoft
- Ansible
- VMware
- vSphere
- Linux
- Ubuntu
title: 'Technology Short Take 110'
url: /2019/02/01/technology-short-take-110/
---

Welcome to Technology Short Take #110! Here's a look at a few of the articles and posts that have caught my attention over the last few weeks. I hope something I've included here is useful for you also!<!--more-->

## Networking

* Via [Kirk Byers][link-1] (who is himself a _fantastic_ resource), I read a couple of articles on network automation that I think readers may find helpful. First up is a treatise from Mircea Ulinic [on whether network automation is needed][link-2]. Next is an older article from Patrick Ogenstad that provides [an introduction to ZTP][link-3] (Zero Touch Provisioning).
* The folks over at Cilium took a look at a recent CNI benchmark comparison and unpacked it a bit. There's some good information in [their article][link-4].
* I first ran into Forward Networks a few years ago at Fall ONUG in New York. At the time, I was recommending that they explore integration with NSX. Fast-forward to this year, and the company [announces support for NSX][link-20] and (more recently) [support for Cisco ACI][link-21]. The recent announcement of their GraphQL-based Network Query Engine (NQE)---more information is available in [this blog post][link-22]---is also pretty interesting to me.

## Servers/Hardware

* Patrick Kennedy over at Serve The Home shares the story of [the Ultra EPYC AMD-powered Sun Ultra 24 workstation][link-14]. Fun read!

## Security

* Maarten Goet has a few recommendations on [securing Kubernetes on Microsoft Azure][link-16].

## Cloud Computing/Cloud Management

* Lior Kamrat has an article on [securing Azure Container Registry for CI/CD pipelines using Azure service principals][link-8].
* Christopher Damerau explains [how to use Ansible to manage VMware resources][link-10]. The article is a bit high-level, and most readers will need to seek out other resources for more details.
* Graham Moore shares some of his experiences and recommendations after [90 days of AWS EKS in production][link-11].
* Tobias Zimmergren shows [how to enable Azure Monitor for your AKS clusters][link-17].
* And while we're on the topic of AKS, AKS developer Weinong Wang shares [how to access control plane logs for AKS][link-18]. (Note: Weinong doesn't mention in his article that a specific feature flag must be enabled on the Azure subscription in order to see AKS audit logs, but [the official docs do][link-19].)
* Panagiotis Georgiadis explains [how to enable PSP (Pod Security Policies) with Traefik][link-23].

## Operating Systems/Applications

* Jorge Castro shares some informal notes on [using Clear Linux as an everyday DevOps/cloud-native/Kubernetes client][link-6].
* Dylan Tientcheu has [a list][link-7] of "super secret" Visual Studio Code hacks to improve productivity.
* Jessie Frazelle's [post on pipes][link-13] captures many of my own thoughts perfectly.
* Aleksa Sarai dives deep into `tar` and its challenges when used for container image formats in [this article][link-15] on the path to OCI v2 images.
* Megan O'Keefe shares [a Kubernetes developer workflow for macOS][link-24] that includes a list of applications, themes, tools, and shortcuts. There's some handy tips/ideas in here.

## Storage

* Jeff Hunter launches what appears to be a series on vSAN capacity management and monitoring with part 1, found [here][link-5].

## Virtualization

* Myles Gray [explains how to create][link-9] an Ubuntu 18.04 LTS cloud image that will work with Guest OS Customization in vSphere.

## Career/Soft Skills

* I've talked before about how I use mind maps as a brainstorming or thought exploration process. After reading [this article from XMind on 9 mind mapping mistakes beginners make][link-12], I realized I make some of these mistakes as well. Here's to improving!

OK, that's all for now. I'll have another Tech Short Take in a few weeks, and hopefully I can finally publish some of the original content I've been working on. Feel free to [contact me on Twitter][link-99] if you'd like to chat, if you have a questions, or if you have suggestions for content I should include in the future. Thanks!

[link-1]: https://pynet.twb-tech.com/
[link-2]: https://mirceaulinic.net/2019-01-09-do-we-need-network-automation/
[link-3]: https://networklore.com/ztp-tutorial/introduction/
[link-4]: https://cilium.io/blog/2018/12/03/cni-performance/
[link-5]: https://blogs.vmware.com/virtualblocks/2019/01/14/vsan-capacity-management-and-monitoring-part-1/
[link-6]: https://github.com/castrojo/clearlinux-systemd/wiki
[link-7]: https://medium.freecodecamp.org/here-are-some-super-secret-vs-code-hacks-to-boost-your-productivity-20d30197ac76
[link-8]: http://thewalkingdevs.io/2019/01/21/acr-spn/
[link-9]: https://blah.cloud/kubernetes/creating-an-ubuntu-18-04-lts-cloud-image-for-cloning-on-vmware/
[link-10]: https://christopherdamerau.com/ansible-part-ii/
[link-11]: https://kubedex.com/90-days-of-aws-eks-in-production/
[link-12]: https://www.xmind.net/blog/en/2019/01/mindmapping-mistakes-most-beginners-make/
[link-13]: https://blog.jessfraz.com/post/for-the-love-of-pipes/
[link-14]: https://www.servethehome.com/introducing-the-ultra-epyc-amd-powered-sun-ultra-24-workstation/
[link-15]: https://www.cyphar.com/blog/post/20190121-ociv2-images-i-tar
[link-16]: https://medium.com/@maarten.goet/securing-kubernetes-on-microsoft-azure-are-your-container-doors-wide-open-bb6e879cec5d
[link-17]: https://zimmergren.net/enable-monitoring-with-azure-monitor-log-analytics-for-aks/
[link-18]: https://medium.com/@weinong/azure-kubernetes-service-control-plane-logs-f8ffa449fd
[link-19]: https://docs.microsoft.com/en-us/azure/aks/view-master-logs
[link-20]: https://www.prnewswire.com/news-releases/forward-networks-announces-support-for-vmware-nsx-data-center-300779095.html
[link-21]: https://www.prnewswire.com/news-releases/forward-networks-announces-support-of-cisco-aci-300784087.html
[link-22]: https://www.forwardnetworks.com/network-query-engine/
[link-23]: https://kubic.opensuse.org/blog/2019-01-24-traefik/
[link-24]: https://medium.com/@mo_keefe/a-kubernetes-development-workflow-for-macos-8c41669a4518
[link-99]: https://twitter.com/scott_lowe
