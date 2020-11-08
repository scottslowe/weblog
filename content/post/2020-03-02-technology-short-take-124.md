---
author: slowe
categories: Information
comments: true
date: 2020-03-02T09:00:00-07:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Kubernetes
- Windows
- Linux
- NSX
- Cumulus
- macOS
- VMware
- vSAN
- SSH
- Blogging
title: 'Technology Short Take 124'
url: /2020/03/02/technology-short-take-124/
---

Welcome to Technology Short Take #124! It seems like the natural progression of the Tech Short Takes is moving toward monthly articles, since it's been about a month since my last one. In any case, here's hoping that I've found something useful for you. Enjoy! (And yes, normally I'd publish this on a Friday, but I messed up and forgot. So, I decided to publish on Monday instead of waiting for Friday.)<!--more-->

## Networking

* Said van de Klundert has a post on [screen scraping basics for network engineers][link-2]. For network engineers just getting started with network automation, I suspect that the information shared here may prove quite useful.
* Ronald de Jong takes readers through using the migration coordinator to [migrate from NSX-v (NSX for vSphere) to NSX-T][link-7]. Ronald's post is fairly detailed, and worth a read if this is a migration you're going to have to undertake.
* For readers who may be new to networking, Daniel Wray has [a write-up on network cabling][link-9].
* Redpill Linpro [talks to readers][link-14] about their new routers running Cumulus Linux. That's cool.

## Servers/Hardware

* This article _is_ about hardware, just not the hardware I'd typically talk about in this section---instead, it's about Philips Hue light bulbs. There's [a security vulnerability][link-25], so don't let your smart home get hacked.

## Security

* Nick Buraglio [introduces][link-12] readers to `sshuttle`. I'm pretty sure I'd heard of this utility before, but somehow it fell off my radar/map. Nick's article has now placed it firmly back on my map, and I hope to be able to test it soon to see how/if it will work for me.
* I recently found [this page][link-13], and thought it was a great idea---trying to bring improved security to the masses. Good for the EFF.
* [This article][link-27] from Bruce Schneier is worth reading.

## Cloud Computing/Cloud Management

* Chip Zoller makes the argument in favor of [running Kubernetes in/on virtual machines][link-4].
* Colleague and teammate Eric Shanks takes readers through what's necessary to do [Active Directory authentication for Kubernetes clusters][link-5].
* Here's a great article from Daniele Polencic on [templating YAML in Kubernetes with real code][link-6]. This is an area where I've dabbled a little bit, but I'm looking forward to using this article to dive a bit deeper and sharpen my skills.
* Christian Posta [talks][link-16] about how microservices are not the only approach to software design, and then walks through the recent shift in Istio---the open source service mesh---from a microservices-based architecture to a more monolithic approach.
* Massimo Re Ferre walks readers [through his first CDK experience][link-28] (using TypeScript).
* If you're looking for some more Kubernetes-on-vSphere content, a couple teammates have put up some good content recently you may enjoy. Timmy Carr has a post on [deploying HA clusters using Cluster API for vSphere][link-31], while Eric Shanks also tackled the topic of [highly-available Kubernetes on vSphere][link-32]. Both are good articles.

## Operating Systems/Applications

* Someone on the Kubernetes Slack instance recently had a question about how CoreDNS was configured. [This explanation of the Corefile][link-1] proved helpful (as a starting point, at least).
* And while we're talking about CoreDNS, here's Sam McGeown's post on [running CoreDNS for name resolution in the lab][link-17].
* Berk Ulsoy [discusses WSL][link-3] (Windows Subsystem for Linux), providing a "Linux veteran's" perspective.
* I haven't tested [this][link-10] yet, but given the quality of the content I've seen on this site I have no reason to doubt the efficacy or veracity. If this works on my Lenovo ThinkPad X1 Carbon, that'll be tremendously useful. (I need to move it back to Windows from Linux. Don't ask.)
* My MacBook Pro doesn't have a Touch Bar so I can't test [this][link-15], but it is kind of cool.
* Here's [one approach][link-20] to an informative and useful terminal/prompt configuration. Personally, I prefer [iTerm2][link-21], [Bash][link-22], and [`powerline-go`][link-23], but that's just me.
* [Not cool][link-24], Wacom.

## Storage

* Colleague and friend Jase McCarty provides readers with information on [running the vSAN Witness appliance on Workstation or Fusion][link-11]. As Jase points out, it isn't officially supported, so keep that in mind. (Side note: you'll probably start seeing more Pure Storage-focused content from Jase [moving forward][link-26].)

## Virtualization

* Wil van Antwerpen [helps][link-8] readers with the migration of their virtual machines running Windows 7 to Windows 10. Specifically, Wil addresses an incompatibility between the BusLogic SCSI adapter that many VMs may be using and Windows 10, which doesn't provide a driver for said adapter.

## Career/Soft Skills

* Steve Athanas tackles [the importance of learning in IT in this post][link-18].
* And speaking of learning, if you're into podcasts [here's a list of cloud computing podcasts you may find helpful][link-19].
* Joe Emison tackles the position that [technical best practices are cyclical][link-29]. I agree with Joe's final conclusion (I'll leave it to readers to go read the article to determine what that final conclusion is).
* Christian Mohn shares his [Hugo and Visual Studio (Code) workflow][link-30] for blogging. (I'm sharing this because this seem to be a combination gaining lots of traction.)

Well, that's it for now. [Hit me on Twitter][link-99] if you have any comments or questions. Thanks for reading!

[link-1]: https://coredns.io/2017/07/23/corefile-explained/
[link-2]: https://saidvandeklundert.net/2020-01-17-python-screen-scraping-basics/
[link-3]: https://ulsoy.org/blog/experiencing-wsl-as-a-linux-veteran-part-1/
[link-4]: https://neonmirrors.net/post/2020-01/why-k8s-on-vms/
[link-5]: https://theithollow.com/2020/01/21/active-directory-authentication-for-kubernetes-clusters/
[link-6]: https://learnk8s.io/templating-yaml-with-code
[link-7]: https://my-sddc.net/migrate-nsx-for-vsphere-to-nsx-t/
[link-8]: https://www.vimalin.com/blog/windows-7-to-windows-10-upgrade/
[link-9]: https://network-wizard.webnode.com/l/network-cabling/
[link-10]: https://www.cyberciti.biz/faq/linux-find-windows-10-oem-product-key-command/
[link-11]: https://www.jasemccarty.com/blog/vswa-67p01-workstation/
[link-12]: https://forwardingplane.net/2020/01/17/nix4neteng-8-sshuttle/
[link-13]: https://ssd.eff.org/en/module-categories/tool-guides
[link-14]: https://www.redpill-linpro.com/techblog/2020/01/17/new-routers.html
[link-15]: https://mikeohara.ca/blog/tech-tip-authenticating-sudo-with-touch-id-on-macos
[link-16]: https://blog.christianposta.com/microservices/istio-as-an-example-of-when-not-to-do-microservices/
[link-17]: https://www.definit.co.uk/2020/01/running-coredns-for-lab-name-resolution/
[link-18]: https://www.transformationaltechnologist.com/blog1/how-to-learn-new-technology-skills-and-why
[link-19]: https://solutionsreview.com/cloud-platforms/the-13-best-cloud-computing-podcasts-you-should-listen-to/
[link-20]: https://medium.com/@Clovis_app/configuration-of-a-beautiful-efficient-terminal-and-prompt-on-osx-in-7-minutes-827c29391961
[link-21]: http://www.iterm2.com/
[link-22]: https://www.gnu.org/software/bash/
[link-23]: https://github.com/justjanne/powerline-go
[link-24]: https://robertheaton.com/2020/02/05/wacom-drawing-tablets-track-name-of-every-application-you-open/
[link-25]: https://appleinsider.com/articles/20/02/05/philips-hue-smart-bulb-allows-hackers-to-attack-your-network
[link-26]: https://www.jasemccarty.com/blog/gettingbacktowork/
[link-27]: https://www.schneier.com/blog/archives/2020/01/modern_mass_sur.html
[link-28]: http://www.it20.info/2020/02/my-first-cdk-experience-under-the-hood/
[link-29]: https://codeburst.io/are-technical-best-practices-cyclical-like-fashion-916520b52fe2
[link-30]: https://vninja.net/2020/02/12/my-hugo-workflow/
[link-31]: https://www.timcarr.net/posts/cluster-api-vsphere-ha/
[link-32]: https://theithollow.com/2020/01/27/kubernetes-ha-on-vsphere/
[link-99]: https://twitter.com/scott_lowe
