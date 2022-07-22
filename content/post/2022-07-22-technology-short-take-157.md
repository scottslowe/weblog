---
author: slowe
categories: Information
comments: true
date: 2022-07-22T10:30:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- NAT
- Envoy
- Vagrant
- Azure
- macOS
- Apple
- CLI
- Git
- VMware
- vSphere
title: 'Technology Short Take 157'
url: /2022/07/22/technology-short-take-157/
---

Welcome to Technology Short Take 157! I hope that this collection of links I've gathered is useful to someone out there. In particular, the "Career/Soft Skills" section is a bit bigger than usual this time around, as is the "Security" section.<!--more-->

## Networking

* Interested in understanding how NAT Traversal works? David Anderson's post on [how NAT Traversal works][link-6] should help.
* This happened a couple of months ago, but I don't think I've linked to it in a Technology Short Take: the Envoy Proxy open source project [announced][link-7] Envoy Gateway, a "new member of the Envoy Proxy family aimed at significantly decreasing the barrier to entry when using Envoy for API Gateway (sometimes known as 'north-south') use cases".
* This is a slightly older article from Ivan Pepelnjak on [using netsim-tools to build Vagrant boxes][link-16], but let's be real---his stuff is kind of timeless anyway, right?

## Security

* Tal Maor, along with Nirit Tyomkin, describe [moving laterally between Azure AD-joined machines][link-9].
* Phil Stokes of SentinelOne describes some of the early [security changes found in macOS "Ventura"][link-10] (i.e., macOS 13).
* Falco recently (at the start of June) released version 0.32.0, which included a number of fairly significant changes. Read more about the release [here][link-12].
* I particularly like how the authors of this post on [the principle of ephemerality][link-14] included a "homework/call to action" section for each of the major areas discussed in the article. Well done.
* Matthias Keller [shares a technique][link-28] for bypassing the IP address hiding feature of Apple's iCloud Private Relay service.

## Cloud Computing/Cloud Management

* The Kubewarden project---which aims to enable policy authors to write Kubernetes policies in any programming language that can compile down to WebAssembly---just achieved two different milestones. First, the project was [accepted into the CNCF Sandbox][link-1]; and second, the project [released Kubewarden 1.0.0][link-2]. Congrats on both milestones!
* Michelle Nguyen talks about Pixie's recent addition of [the ability to export custom data in OpenTelemetry format][link-3] via a new plugin system.
* Matt Rickard shares some thoughts on [when it is appropriate to use Kubernetes][link-4].
* Dan Lorenc [takes the cover off OCI Artifacts][link-8].
* This [multi-tenancy page][link-13] was recently added to the Kubernetes documentation; I say it is a welcome addition.

## Operating Systems/Applications

* I believe I shared this a short while ago on Twitter: [this article][link-5] talks about `gmailctl`, a command-line utility for managing Gmail filters using a configuration file. This looks handy enough that I _almost_ wish I liked Gmail.
* Julia Evans has a post discussing [some "new-ish" command-line tools][link-15]; it might be worth checking out. I use a fair number of these (and referenced a few of them in [my 2018 "Supercharging My CLI" blog post][xref-1]).
* [This][link-21] is useful to know.
* Rob Harrop has a series on using Cue to generate Envoy configurations; part 1 of the series---along with links to the other posts in the series---is available [here][link-22].
* Mark Dominus has a two part series ([part 1][link-24], [part 2][link-25]) on "things I wish everyone knew about Git."
* Here are some notes on [macOS `zsh` configuration][link-29].

## Programming

* Paul Johnston explains why he believes [serverless systems aren't software systems][link-11]. I must admit I initially took umbrage with the title, but Paul does admit that the title of the article is a little click-baity (is that a word?). Regardless, Paul does effectively point out some of the differences in building "traditional" software systems versus serverless systems.

## Virtualization

* In the event you're interested in diving deep into the bowels of vSphere, there's [this episode of the Unexplored Territory podcast][link-18].
* Good to see VMware finally add support for `cloud-init`, the _de facto_ standard in public cloud environments for quite some time. For more details, William Lam has you covered; see [this post][link-19] and [this follow-up][link-20].

## Career/Soft Skills

* Constantin Gonzalez shares some thoughts on [how to find good opportunities][link-17] in your career or life.
* I'm a big fan of Cal Newport's books, but for some reason never really read a lot of the stuff on his blog. However, [this article on the Einstein principle][link-23] recently popped up for me, and even though it's a much older article I still enjoyed it and found some useful advice in it.
* I really enjoyed this article by Jan Schaumann on [learning by lurking][link-26]. There's some good stuff in this article, like a link to this article on [asking the duck][link-27].

That's it for this Technology Short Take. Your feedback is always welcome; feel free to contact [me via Twitter][link-99], or on any one of a number of different Slack communities. I'd be happy to chat with you.

[link-1]: https://www.kubewarden.io/blog/2022/06/cncf-sandbox-inclusion/
[link-2]: https://www.kubewarden.io/blog/2022/06/v1-release/
[link-3]: https://blog.px.dev/plugin-system/
[link-4]: https://matt-rickard.com/dont-use-kubernetes-yet/
[link-5]: https://opensource.com/article/22/5/gmailctl-linux-command-line-tool
[link-6]: https://tailscale.com/blog/how-nat-traversal-works/
[link-7]: https://blog.envoyproxy.io/introducing-envoy-gateway-ad385cc59532
[link-8]: https://dlorenc.medium.com/oci-artifacts-explained-8f4a77945c13
[link-9]: https://medium.com/@talthemaor/moving-laterally-between-azure-ad-joined-machines-ed1f8871da56
[link-10]: https://www.sentinelone.com/blog/apples-macos-ventura-7-new-security-changes-to-be-aware-of/
[link-11]: https://pauldjohnston.medium.com/serverless-systems-arent-software-systems-41ee0f826055
[link-12]: https://falco.org/blog/falco-0-32-0/
[link-13]: https://kubernetes.io/docs/concepts/security/multi-tenancy/
[link-14]: https://blog.chainguard.dev/the-principle-of-ephemerality/
[link-15]: https://jvns.ca/blog/2022/04/12/a-list-of-new-ish--command-line-tools/
[link-16]: https://blog.ipspace.net/2022/02/netsim-build-vagrant-boxes.html
[link-17]: https://constantin.glez.de/2022/06/26/how-find-good-opportunities/
[link-18]: https://frankdenneman.nl/2022/07/01/unexplored-territory-podcast-episode-19-discussing-numa-and-cores-per-sockets-with-the-main-cpu-engineer-of-vsphere/
[link-19]: https://williamlam.com/2022/06/using-the-new-vsphere-guest-os-customization-with-cloud-init-in-vsphere-7-0-update-3.html
[link-20]: https://williamlam.com/2022/06/quick-tip-prepare-vmware-photon-os-for-use-with-vsphere-guest-os-customization-and-cloud-init.html
[link-21]: https://daringfireball.net/2022/06/require_a_passcode_to_unlock_your_iphone
[link-22]: https://www.robharrop.dev/posts/cue-envoy/part-1-schema-definition/
[link-23]: https://www.calnewport.com/blog/2007/10/10/the-einstein-principle-accomplish-more-by-doing-less/
[link-24]: https://blog.plover.com/prog/git/tips.html
[link-25]: https://blog.plover.com/prog/git/tips-2.html
[link-26]: https://www.netmeister.org/blog/learning-by-lurking.html
[link-27]: https://hwrnmnbsol.livejournal.com/148664.html
[link-28]: https://m-keller.com/2022-07-21-icloud-private-relay-dns-leak.html
[link-29]: https://mybyways.com/blog/macos-zsh-configuration
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2018-11-29-supercharging-my-cli.md" >}}
