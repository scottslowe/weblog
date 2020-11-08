---
author: slowe
categories: Information
comments: true
date: 2020-07-17T17:00:00-07:00
tags:
- Hardware
- Networking
- Security
- Storage
- Virtualization
- OVS
- Kubernetes
- Automation
- Linux
- Apple
- macOS
- AWS
- Azure
- CLI
- Docker
- Go
title: 'Technology Short Take 129'
url: /2020/07/17/technology-short-take-129/
---

Welcome to Technology Short Take #129, where I've collected a bunch of links and references to technology-centric resources around the Internet. This collection is (mostly) data center- and cloud-focused, and hopefully I've managed to curate a list that has some useful information for readers. Sorry this got published so late; it was supposed to go live this morning!<!--more-->

Note there is a slight format change debuting in this Tech Short Take. Moving forward, I won't include sections where I have no content to share, and I'll add sections for content that may not typically appear. This will make the list of sections a bit more dynamic between Tech Short Takes. Let me know if you like this new approach---feel free to contact [me on Twitter][link-99] and provide your feedback.

Now, on to the good stuff!

## Networking

* Chip Zoller [takes a look at Antrea][link-4], a new Kubernetes CNI (Container Networking Interface) plugin that leverages Open vSwitch (OVS).
* Ivan Pepelnjak has a fantastic article on [adapting network design to acommodate automation][link-6]. This is a great read overall, but one sentence in particular _really_ caught my eye: "30 years ago I was able to write a complete multitasking operating system in Z80 assembly code. I can no longer do that, and nobody is willing to pay me for that skill set. The world has moved on, and so did I." Great quote!
* Jon Langemak is blogging again, and he jumps back into the "blogging saddle" with a post on [working with `tc` on Linux systems][link-19].

## Servers/Hardware

* Since Apple's announcement at WWDC 2020 about the transition to ARM-based CPUs, there's been a lot of analysis and discussion of what this means. [This article][link-17] captures a lot of the same thoughts I've been having, and it's why I wonder whether Apple's foothold in the enterprise will survive the CPU transition.

## Security

* SecurityWeek [discusses][link-18] a recent bypass of some of macOS' privacy protections.
* With the recent discovery that many iOS apps are "snooping" on the clipboard (as exposed by the iOS 14 beta), some security professionals are advocating users to turn off Handoff, a mechanism that---among other things---provides a shared clipboard between macOS and iOS devices. [This article][link-23] by Quincy Larson both provides some background for the recommendation as well as instructions on how to turn off Handoff.

## Cloud Computing/Cloud Management

* For newcomers to Kubernetes, [this `kubectl` post][link-1] by Lukasz Bartnicki may prove helpful.
* Daniel Weibel discusses [choosing a worker node size][link-5], and weighs some of the impacts of various approaches.
* [This post][link-10] takes a closer look at the PodTopologySpread scheduling plugin, a feature promoted to beta with the Kubernetes 1.18 release.
* I have two posts related to policies in Kubernetes this time around. The first, by Guarav Chaware, discusses [using Open Policy Agent (OPA) to implement Pod Security Policies (PSPs)][link-11]. The second, by Jason Price, [discusses how Square implemented PSPs in their environment][link-12].
* This post walks readers through [using AWS API Gateway as an ingress controller with Amazon EKS][link-13].
* Jon Owings [shares a command-line trick][link-14] combining `kubectl` and `xargs` to check multiple contexts.
* Michael Kashin digs deeper into [the anatomy of the "kubernetes.default"][link-15] service and its importance to Kubernetes.
* Bruno Hildenbrand shares [20 tips for managing Linux VMs on Azure][link-24].

## Operating Systems/Applications

* Team colleague Matt Bagnara writes about [NixOS and Enlightenment][link-2] after a short test drive.
* Kevin Campusano discusses [Linux development in Windows 10 with Docker and WSL 2][link-7]. Although I am not a Windows fan (I've been a macOS and/or Linux user since 2003, _before_ Apple switched to Intel processors), I must say that the work that Microsoft is doing with WSL 2 appears---from the perspective of an outside user---to be pretty impressive.
* Alex Ellis takes a look at [building containers without Docker][link-9].
* Former colleague (he recently moved into a new role) Jamie Duncan talks about using `buildx` and container manifests to build container images for multiple CPU architectures in [this blog post][link-16]. As Jamie points out, this sort of workflow _could_ be useful when Apple moves to ARM-based CPUs (as I mentioned above, I'm not convinced that Apple's foothold in the enterprise will survive the architecture transition).
* This is an older post, but may still be useful: here's [how to hide mounted volumes][link-20] from showing up in macOS Finder and the desktop.
* I've [written about `jq` before][xref-1]. If you're interested in mastering this very useful tool, [this][link-21] may come in handy.
* Hans Kruse [writes about "throw-away WSL2 environments."][link-22]
* [This article][link-29] opened my eyes to some interesting possibilities on macOS. I've already added Karabiner Elements and Hammerspoon to my system, and I'm exploring how best to leverage them in my daily workflow.

## Programming

Recognizing that many formerly infrastructure-focused folks are being pulled into software development and programming, this is another section that will sometimes make its way into my Tech Short Takes.

* I've been working with Go a fair amount over the last few weeks, and one of the things I didn't understand (and perhaps still don't _fully_ grasp) is when the appropriate time is to use a pointer. [This article][link-30] by Dylan Meeus helps provide some guidelines.

## Virtualization

* Virtualization proponents have said for years that the use of a hypervisor would enable new ways of protecting VMs from security threats/risks. [It seems the industry is finally getting there.][link-28]

## Serverless

I am by no means a serverless expert (not even close!), but I have been finding an increasing number of serverless-related articles popping up in my timelines, RSS feeds, etc. As I mentioned above, this will be one of those sections that will appear from time to time as I find content.

* Emily Shea has a guide to [getting started with serverless on AWS][link-8].
* Jason Lengstorf of Netlify, via a guest blog post on the Postman site, provides [an introduction to serverless functions][link-25] (mostly in the context of Netlify Functions). The article includes---as one might anticipate---some use of Postman to test said serverless functions.
* I came acoss [the Real World Serverless podcast][link-26], a podcast hosted by Yan Cui. If you're looking for more serverless content, this may be a good resource. (I haven't yet had the time to listen to any episodes.)
* Nick Korte continues his series on Azure Functions with [this installation on CI/CD with Azure Functions][link-27].

## Career/Soft Skills

* Snir David writes about how [written communication is a remote work superpower][link-3]. The "TL;DR" from his article is that asynchronous communication is better suited for remote work than instant messaging/chat and audio/video calls. This echoes a sentiment that I've had for quite a while (and that I believe I've shared here before).

OK, that's enough for this time around! I hope that you have found something useful or educational, and I hope that some of the new types of content that I'll include moving forward are also useful. Your feedback as a reader is always welcome, so contact [me on Twitter][link-99] and let me know what you think. Thanks!

[link-1]: https://knowledgepill.it/posts/kubernetes_kubectl_client_config/
[link-2]: https://bagnaram.github.io/blog/2020/06/20/nix-enlightenment
[link-3]: https://snir.dev/blog/remote-async-communication/
[link-4]: https://neonmirrors.net/post/2020-06/antrea-the-ubiquitous-cni/
[link-5]: https://itnext.io/architecting-kubernetes-clusters-choosing-a-worker-node-size-b3729cc0c78f
[link-6]: https://blog.ipspace.net/2020/06/adapting-network-design-for-automation.html
[link-7]: https://www.endpoint.com/blog/2020/06/18/linux-development-in-windows-10-docker-wsl-2
[link-8]: https://emshea.com/post/serverless-getting-started
[link-9]: https://blog.alexellis.io/building-containers-without-docker/
[link-10]: https://kubernetes.io/blog/2020/05/introducing-podtopologyspread/
[link-11]: https://www.infracloud.io/kubernetes-pod-security-policies-opa/
[link-12]: https://developer.squareup.com/blog/kubernetes-pod-security-policies/
[link-13]: https://aws.amazon.com/blogs/containers/api-gateway-as-an-ingress-controller-for-eks/
[link-14]: https://blog.2vcps.io/2020/06/29/kubectl-check-all-the-contexts/
[link-15]: https://networkop.co.uk/post/2020-06-kubernetes-default/
[link-16]: https://medium.com/@jamieeduncan/easily-making-container-images-for-multiple-platforms-6fbf097c9742
[link-17]: https://bmalehorn.com/arm-mac/
[link-18]: https://www.securityweek.com/macos-privacy-protections-bypass-disclosed-after-apple-fails-release-fix
[link-19]: http://www.dasblinkenlichten.com/working-with-tc-on-linux-systems/
[link-20]: https://www.idownloadblog.com/2016/12/02/how-to-hide-mounted-volumes-from-desktop-finder/
[link-21]: https://codefaster.substack.com/p/mastering-jq-part-1-59c
[link-22]: https://hanskruse.eu/post/2020-07-04-throw_away_wsl_environments/
[link-23]: https://www.freecodecamp.org/news/turn-off-universal-clipboard-handoff-mac-iphone/
[link-24]: https://blog.hildenco.com/2020/07/20-tips-to-manage-linux-vms-on-azure.html
[link-25]: https://blog.postman.com/serverless-functions-the-fast-way/
[link-26]: https://realworldserverless.com/
[link-27]: http://blog.thenetworknerd.com/2020/07/03/from-vs-code-to-azure-pipelines-basic-ci-cd-with-azure-functions/
[link-28]: https://www.zdnet.com/article/microsofts-project-freta-this-new-free-service-spots-rootkits-lurking-in-cloud-vms/
[link-29]: https://blog.craftlab.hu/how-to-become-a-modern-magician-productivity-tips-for-devs-on-macos-7a886c43d870
[link-30]: https://medium.com/@meeusdylan/when-to-use-pointers-in-go-44c15fe04eac
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-11-11-handy-cli-tool-json.md" >}}
