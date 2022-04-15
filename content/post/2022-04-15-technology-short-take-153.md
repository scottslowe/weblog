---
author: slowe
categories: Information
comments: true
date: 2022-04-15T08:45:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OVS
- Kubernetes
- Linux
- Azure
- SSH
- VMware
- Envoy
- JSON
- Git
title: 'Technology Short Take 153'
url: /2022/04/15/technology-short-take-153/
---

Welcome to Technology Short Take #153! My personal and professional life has kept me busy over the last couple of months, so things have been quiet here on the blog. I've still been collecting links to share with you, though, and here's the latest collection. I hope you're able to find something useful here!<!--more-->

## Networking

* [This article][link-6] contains some good information on IPv6 for those who are just starting to get more familiar with it, although toward the end it turns into a bit of an advertisement.
* Want to understand `kube-proxy`, a key part of Kubernetes networking, a bit better? [Start here][link-15]. Arthur Chiao's [post on cracking `kube-proxy`][link-27] is also an excellent resource---in fact, there's so much information packed in there you may need to read it more than once.
* Xavier Avrillier [walks][link-3] readers through using Antrea (a Kubernetes CNI built on top of Open vSwitch---a topic I've touched on a time or two) to provide on-premise load balancing in Kubernetes.

## Servers/Hardware

* Cabling is hardware, right? What happens to submarine cables when there are massive events, like a volcanic eruption? Ulrich Speidel [shares][link-11] some of the findings after the volcanic eruption in Tonga.

## Security

* Although Linux is often considered to be superior to Windows and macOS with regard to security, it is not without [its own security flaws][link-5]. I, personally, would posit that Linux _can_ be more secure than other platforms, but it requires proper configuration and can still be undone by a loose nut behind the keyboard.
* Ouch---[another Azure vulnerability][link-8] that allows cross-account access.

## Cloud Computing/Cloud Management

* Diego Sucaria shows [how to use an SSH SOCKS proxy to access private Kubernetes clusters][link-1]. This is an interesting approach that, honestly, I hadn't considered. (Warning: I'm not sure the configuration as described in the article will actually work, but the concept is sound.) If you're unfamiliar with SSH SOCKS proxying, read [this][link-19] to get up to speed (or use your favorite web search engine, there are many articles out there on this topic).
* Version 3.8.0 of Helm adds the ability to store and work with charts in container (OCI, Open Container Initiative) registries (instead of Helm repositories). More information is available in [this Helm blog post][link-2].
* Daniel Helfand [plays around with vclusters and Carvel][link-7].
* Here's [a set of 15 principles for designing and deploying scalable applications on Kubernetes][link-9].
* Fatima Silveira has a good article on [using the Kubernetes Horizontal Pod Autoscaler][link-14].
* Ajeet Singh Raina shares a list of [the top 200 Kubernetes tools for DevOps engineers][link-16].
* Levent Ogut takes [a deep dive into Kubernetes init containers][link-18].

## Operating Systems/Applications

* Steven Bright shows [how to deploy Salt minions automatically using VMware Tools][link-4]. (Note that this page doesn't seem to render properly in Firefox, although Chrome and Safari seem fine.)
* I found this article to be [a good overview of WebAssembly][link-10].
* Matt Oswalt takes readers though [a fairly in-depth look][link-12] at sockets and address binds in Linux.
* If you are an absolute beginner with Envoy Proxy, [this tutorial][link-13] will help you get started. However, it quickly runs out of steam, and you'll need to progress to more advanced examples and documentation to continue your learning journey.
* I asked a question about Git, Git tags, and releasing versions of a project on Twitter the other day (here's [the tweet][link-21]), and [this article on Git branching][link-22] was shared with me. Perhaps you'll find it useful as well!
* I've been digging into OIDC/OAuth 2.0 lately, and learning about JWTs (JSON Web Tokens, see [here][link-24] for an introduction). I found this article telling folks to [stop overloading JWTs with permissions claims][link-23] a while ago, but at the time I didn't fully understand all the concepts or the implications. Now that I've had some time and some exposure, this makes a lot more sense to me---and I agree with the author!
* Speaking of JWTs: they don't score so well in Thomas Ptacek's [survey of API tokens][link-25]. There's a lot to unpack in Ptacek's article; I suspect I'll be coming back to read it again later as my knowledge in this area grows. Some of it is still too advanced for me right now.

## Storage

Nothing here this time around---maybe [go check J Metz' blog instead][link-30]?

## Virtualization

* If you're a PowerCLI user, [here are some scripts you may find useful][link-17].
* [This][link-28] is such an invaluable resource. And it's [publicly available on GitHub][link-29], although not licensed with a typical open source license.

## Career/Soft Skills

* Have you ever had one of those days where you just aren't feeling like working? I think we all have. Here is [an article from the Harvard Business Review][link-20] on some strategies for motivating yourself when that happens.
* Although written from the perspective of a developer seeking to improve their knowledge in a particular programming language, I think the ideas and principles laid out in this article on [how developers remember things][link-26] are applicable to folks learning new things in other IT disciplines as well.

That's it for this time around. I'll be back soon with more original content as well as more Short Takes! In the meantime, feel free to reach out to me---my e-mail address isn't hard to find, I'm present in a number of Slack communities, and you can always hit [me on Twitter][link-99] (DMs are open). Thanks for reading!

[link-1]: https://diegosucaria.info/how-to-access-a-private-eks-cluster-from-your-local-machine-without-a-vpn/
[link-2]: https://helm.sh/blog/storing-charts-in-oci/
[link-3]: https://www.vxav.fr/2022-03-11-on-premise-layer-2-service-type-loadbalancer-with-antrea/
[link-4]: https://www.stevenbright.com/2022/03/deploy-salt-minions-automatically-using-vmware-tools/
[link-5]: https://arstechnica.com/information-technology/2022/03/linux-has-been-bitten-by-its-most-high-severity-vulnerability-in-years/
[link-6]: https://pansift.com/blog/how-to-fix-ipv6-connectivity/
[link-7]: https://carvel.dev/blog/carvel-vcluster/
[link-8]: https://orca.security/resources/blog/autowarp-microsoft-azure-automation-service-vulnerability/
[link-9]: https://elastisys.com/designing-and-deploying-scalable-applications-on-kubernetes/
[link-10]: https://www.fermyon.com/blog/how-to-think-about-wasm
[link-11]: https://blog.apnic.net/2022/03/01/when-volcanoes-go-bang-submarine-cables-do-what/
[link-12]: https://oswalt.dev/2022/02/non-local-address-binds-in-linux/
[link-13]: https://adamtheautomator.com/envoy-proxy/
[link-14]: https://itnext.io/kubernetes-for-dummies-deployment-auto-scaling-28ad0f9da1df
[link-15]: https://mayankshah.dev/blog/demystifying-kube-proxy/
[link-16]: https://dev.to/ajeetraina/top-200-kubernetes-tools-for-devops-engineer-like-you-3h7e
[link-17]: https://www.hollebollevsan.nl/usefull-powercli-scripts/
[link-18]: https://loft.sh/blog/kubernetes-init-containers/
[link-19]: https://linuxtect.com/create-socks-proxy-via-ssh-tunnelling/
[link-20]: https://hbr.org/2018/11/how-to-keep-working-when-youre-just-not-feeling-it
[link-21]: https://twitter.com/scott_lowe/status/1513557942255206401
[link-22]: https://nvie.com/posts/a-successful-git-branching-model/
[link-23]: https://sdoxsee.github.io/blog/2020/01/06/stop-overloading-jwts-with-permission-claims
[link-24]: https://jwt.io/introduction/
[link-25]: https://fly.io/blog/api-tokens-a-tedious-survey/
[link-26]: https://www.learncoderetain.com/2021/06/28/how-developers-remember-things/
[link-27]: https://arthurchiao.art/blog/cracking-k8s-node-proxy/
[link-28]: https://www.vmwareopsguide.com/
[link-29]: https://github.com/TheNewStellW/vmware-operations-guide
[link-30]: https://jmetz.com/2022/03/storage-short-take-41/
[link-99]: https://twitter.com/scott_lowe
