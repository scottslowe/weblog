---
author: slowe
categories: Information
comments: true
date: 2021-12-31T09:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Docker
- Apple
- macOS
- Microsoft
- Azure
- Kubernetes
- Linux
- KVM
- OSS
- Certification
title: 'Technology Short Take 150'
url: /2021/12/31/technology-short-take-150/
---

Welcome to Technology Short Take #150! This is the last Technology Short Take of 2021, so hopefully I'll close the year out "with a bang" with this collection of links and articles on various technology areas. Bring on the content!<!--more-->

## Networking

* Ivan Pepelnjak has a post on [running network automation tools in a container][link-28]. In fact, he's already built some container images, and the post has information on running tools from his prebuilt container image. Well worth reading!
* Tom Hollingsworth [likens][link-29] networking disaggregation to "cutting the cord" and switching away from cable.

## Servers/Hardware

* Derek Seaman [chronicles his adventures][link-2] in getting dual displays working with his M1 Pro-based 14" MacBook Pro.
* Glenn Fleishman [offers more information on USB cables and standards than you probably cared to know][link-33].

## Security

* Nicholas Weaver (no, not [that Nick Weaver][link-6]) [discusses the Log4Shell vulnerability][link-5].
* The Log4J vulnerability and associated exploits has been on many folks' minds, so it's only natural that many security companies have been looking into how to mitigate this attack vector. Aqua Security has a write-up on some of their analysis [here][link-20].
* This is an older post, but it doesn't look like I've linked to it before, so I thought I'd include it here. Some Azure folks from Microsoft have published [a threat matrix for Kubernetes][link-12]. This is useful, in my opinion, because now platform operators have a "starting point" on the specific threats they need to try to mitigate.
* I found [this article on NSO/Pegasus][link-31] a very interesting read. There's also some great information in [this Google Project Zero blog post][link-32].

## Cloud Computing/Cloud Management

* This is an older article, but resurfaced (on my radar, at least) recently---it's [a collection of serverless best practices][link-3] from Paul Johnston.
* Cormac Hogan looks at using [cert-manager][link-15] to [secure LDAP communication in TKG clusters][link-14]. Although a lot of this is, in theory, portable to any Kubernetes cluster, keep in mind that some of the commands shared in Cormac's post are specific to the way TKG does things.
* Dmitri Lerko shows how to use labels to do [dynamic alert routing with Prometheus and AlertManager][link-17] (in other words, routing alerts to the correct team based on labels).
* Tim O'Reilly [takes a look at Web3][link-25].
* Kevin Swiber shows [how to use Postman to deploy an application onto a Kubernetes cluster][link-27].

## Operating Systems/Applications

* Jérôme Petazzoni lists some [anti-patterns when building container images][link-4]. I like that Jérôme also pointed out that these anti-patterns aren't always bad (another way of saying sometimes you need to do things differently to satisfy your particular requirements).
* Will Thompson digs a bit deeper into [Flatpak disk usage and deduplication][link-7]. This article is apparently in response to [this other article][link-8] that is critical of Flatpak.
* Filippo Valsorda [speculates on how open source maintainers need to evolve into professional maintainers][link-11]. This is a topic that comes up every time a notable vulnerability or flaw is found in some piece of heavily-used open source software; the prompt this time, as far as I can tell, is the Log4j vulnerability. Regardless of the prompt, I do agree with many of the things Filippo says in this article.
* Beth Marshall (aka "Beth the Tester") provides [a reference guide to blocks in Postman Flows][link-13].
* Who's interested in [a graphical desktop companion app for Podman][link-21]?
* I don't know...I'm not sure if JSON can _ever_ be described as "for humans" (see [this site][link-23]).
* LaunchBar is, in my opinion, an extremely useful macOS application. Making it even more useful is third-party actions like [this one][link-26] for easily looking up times in cities around the world. (There's a whole collection of actions in this repository.)

## Storage

* Jeff Geerling describes [his backup plan][link-1]. (Jeff, if you aren't aware, is a fairly prolific YouTube content creator.)

## Virtualization

* This article provides some in-depth information on [optimizing gVisor to run at scale][link-9].
* Here's [how to enable UEFI support on KVM][link-16].
* Thomas Leonard has an interesting blog on [using virtualization to isolate Xwayland][link-18].
* Via [this Phoronix article][link-19], QEMU 6.2 is available. New features in QEMU 6.2 include, among other things, support for Apple Silicon-based virtualization hosts (to run AArch64 guests).
* I have a genuine question here (not trolling or being sarcastic): what is the use case/intended audience for [Cloud Hypervisor][link-24]? I'm not really understanding where this fits into the overall virtualization landscape.

## Career/Soft Skills

* Katarina Brookfield recently shared her experience with the Certified Kubernetes Administrator (CKA) exam; you can find her notes [here][link-10]. Oh, and while we're at it---you may find Curtis Collicutt's [experience with the Certified Kubernetes Security (CKS) specialist exam][link-30] helpful as well.
* [This post][link-22] has a list of "10 ways to prove yourself during remote work." It seems to be primarily targeted at leadership-type roles, but readers may find a few useful tips here.

That's a wrap! Not only for this Technology Short Take, but also for 2021. I hope that 2022 brings you much success and joy. As always, feel free to contact [me on Twitter][link-99] if you'd like to provide feedback, ask a question, or just say hello. I'd love to hear from you!

[link-1]: https://www.jeffgeerling.com/blog/2021/my-backup-plan
[link-2]: https://www.derekseaman.com/2021/11/my-journey-for-dual-displays-with-my-m1-pro-mac.html
[link-3]: https://pauldjohnston.medium.com/serverless-best-practices-b3c97d551535
[link-4]: http://jpetazzo.github.io/2021/11/30/docker-build-container-images-antipatterns/
[link-5]: https://www.lawfareblog.com/whats-deal-log4shell-security-nightmare
[link-6]: https://twitter.com/lynxbat
[link-7]: https://blogs.gnome.org/wjjt/2021/11/24/on-flatpak-disk-usage-and-deduplication/
[link-8]: https://ludocode.com/blog/flatpak-is-not-the-future
[link-9]: https://gvisor.dev/blog/2021/12/02/running-gvisor-in-production-at-scale-in-ant/
[link-10]: http://advecti.io/2021/my-cka-exam-experience-and-tips/
[link-11]: https://blog.filippo.io/professional-maintainers/
[link-12]: https://www.microsoft.com/security/blog/2020/04/02/attack-matrix-kubernetes/
[link-13]: https://beththetester.com/2021/11/27/postman-flows-a-block-reference-guide/
[link-14]: https://cormachogan.com/2021/11/24/securing-ldap-with-tls-certificates-in-tkg-v1-4/
[link-15]: https://cert-manager.io
[link-16]: https://www.howtoforge.com/enable-uefi-support-on-kvm-virtualization/
[link-17]: https://tech.loveholidays.com/dynamic-alert-routing-with-prometheus-and-alertmanager-f6a919edb5f8
[link-18]: https://roscidus.com/blog/blog/2021/10/30/xwayland/
[link-19]: https://www.phoronix.com/scan.php?page=news_item&px=QEMU-6.2-Released
[link-20]: https://blog.aquasec.com/real-world-log4j-attacks-analysis
[link-21]: https://iongion.github.io/podman-desktop-companion/
[link-22]: https://enterprisersproject.com/article/2021/10/remote-work-how-prove-yourself
[link-23]: https://json5.org/
[link-24]: https://www.cloudhypervisor.org/
[link-25]: https://www.oreilly.com/radar/why-its-too-early-to-get-excited-about-web3/
[link-26]: https://github.com/Ptujec/LaunchBar/tree/master/Timezones
[link-27]: https://hackernoon.com/how-to-automate-kubernetes-deployments-with-postman
[link-28]: https://blog.ipspace.net/2021/12/network-automation-container.html
[link-29]: https://networkingnerd.net/2021/12/10/is-disaggregation-going-to-be-cord-cutting-for-the-enterprise/
[link-30]: https://serverascode.com//2021/07/27/thoughts-on-the-cks-exam.html
[link-31]: https://arstechnica.com/information-technology/2021/12/the-secret-uganda-deal-that-has-brought-nso-to-the-brink-of-collapse/
[link-32]: https://googleprojectzero.blogspot.com/2021/12/a-deep-dive-into-nso-zero-click.html
[link-33]: https://tidbits.com/2021/12/03/usbefuddled-untangling-the-rats-nest-of-usb-c-standards-and-cables/
[link-99]: https://twitter.com/scott_lowe
