---
author: slowe
categories: Information
comments: true
date: 2025-01-10T09:00:00-06:00
tags:
- ESXi
- EVPN
- Hardware
- Intel
- Kubernetes
- Linux
- Networking
- Security
- Storage
- Virtualization
- VMware
- Wireless
- Writing
title: 'Technology Short Take 185'
url: /2025/01/10/technology-short-take-185/
---

Welcome to Technology Short Take #185, the first of 2025! I'm excited for the opportunity to continue to bring readers articles and links of interest across data center- and cloud-related technologies (along with some original content along the way). I had originally intended for this post to be my last post of 2024, but personal challenges got in the way. Enough of that, though---on to the content!<!--more-->

## Networking

* Rasika Nayanajith has [a detailed overview of Wi-Fi 7][link-15].
* Just before the end of 2024, John Howard (whose blog I only recently discovered---lots of great content there!) posted a write-up on [using Vault to provision TLS certificates onto a network appliance][link-17].
* Federico Paolinelli shared information on [running Podman pods as systemd units][link-18] in the context of an architecture to terminate EVPN inside Kubernetes nodes. Federico's blog is another one I only recently discovered; there's lots of great networking content here.
* Julio Perez has a nice write-up on [Codespaces for network engineers and educators][link-19]. (Hat tip to [Ivan Pepelnjak](https://ipspace.net) for pointing me to Julio's site.)

## Servers/Hardware

* Bryan Cantrill shares some thoughts on [why Gelsinger was wrong for Intel][link-3].

## Security

* Brian Krebs' article on [the operations of a prolific voice phishing crew][link-6] was both enlightening and frightening.
* ESET has an article with [information on Bootkitty][link-8], which they label as "the first UEFI bootkit designed for Linux systems."

## Cloud Computing/Cloud Management

* I haven't personally used `mirrord`, but this article on [port forwarding with `mirrord`][link-2] piqued my interest. I might have to give this a try soon.
* Rory McCune asks the question, ["When is read-only not read-only?"][link-9] (This is in the context of a Kubernetes RBAC and, in Rory's words, "a possible sharp corner".)
* Frederic Branczyk with Polar Signals [discusses `kubezonnet`][link-14], a tool for identifying and measuring cross-zone pod network traffic in Kubernetes clusters.
* This article is a good write-up on [troubleshooting/identifying a memory leak that wasn't actually there][link-16].
* Dan Slimmon reviews [the metrics that are relevant to latency- and throughput-optimized clusters][link-20].

## Operating Systems/Applications

* [This article from Edward Zitron][link-1] is a scathing indictment of generative AI, but I can't really disagree with any of it. My hope---and the hope of others I've talked to that share the same perspective---is that the fall of GenAI doesn't mean the fall of the entire tech economy.
* Matthew Sanabria shares a list of [tools worth changing to in 2025][link-4]. None of these tools are new to me, but it's nice to hear about another user's direct experience. I will say that I have thus far dismissed LLMs---see my thoughts about generative AI in the previous bullet---but I may reconsider local use of an LLM via [Ollama][link-5]. I don't have a GPU to throw at it, so the experience may be subpar.

## Virtualization

* William Lam kicks off his 2025 blogging content with an article on an [easier way to simulate custom ESXi SMBIOS hardware strings][link-7].

## Career/Soft Skills

* Does social media count as a soft skill? I'm going to say "Yes" so that I can put Virginia Craft's article on [how to quit mainstream social media and join Mastodon][link-10] in this section. Twitter/X is the only "mainstream social media" platform I use, and---if I'm honest---I'm using it less these days in favor of [Mastodon][link-11] and [Bluesky][link-12].
* Something Tom Hollingsworth said in [his recent post looking back at 2024][link-13] really stuck with me: "I want to make sure I’m bringing you the kind of content that you want to read instead of just posting because I need to create something." That really resonated with me. I know my blogging _frequency_ has gone down in recent years, but I don't want to sacrifice blogging _quality_ just for the sake of posting more often. Thanks, Tom---it's nice to know that other writers have the same struggles I do.

That's all for now. If you have any feedback, or if you just want to reach out and say hello, you can find me online in a variety of places. My email address isn't too hard to find, so you're welcome to drop me an email message if that's your preference. Otherwise, find me [on Bluesky][link-29], [on Mastodon][link-30], [on Twitter][link-99], and in a variety of Slack communities. I'd love to hear from you!

[link-1]: https://www.wheresyoured.at/godot-isnt-making-it/
[link-2]: https://metalbear.co/blog/there-and-back-again-port-forwarding-with-mirrord/
[link-3]: https://bcantrill.dtrace.org/2024/12/08/why-gelsinger-was-wrong-for-intel/
[link-4]: https://matthewsanabria.dev/posts/tools-worth-changing-to-in-2025/
[link-5]: https://ollama.com/
[link-6]: https://krebsonsecurity.com/2025/01/a-day-in-the-life-of-a-prolific-voice-phishing-crew/
[link-7]: https://williamlam.com/2025/01/easier-method-to-simulate-custom-esxi-smbios-hardware-strings.html
[link-8]: https://www.welivesecurity.com/en/eset-research/bootkitty-analyzing-first-uefi-bootkit-linux/
[link-9]: https://raesene.github.io/blog/2024/11/11/When-Is-Read-Only-Not-Read-Only/
[link-10]: https://unboundroutes.com/2024/11/29/quit-social-media-join-mastodon/
[link-11]: https://fosstodon.org/@scottslowe
[link-12]: https://bsky.app/profile/scottslowe.bsky.social
[link-13]: https://networkingnerd.net/2025/01/01/a-year-of-consistency-again/
[link-14]: https://www.polarsignals.com/blog/posts/2025/01/09/introducing-kubezonnet
[link-15]: https://mrncciew.com/2024/12/14/wi-fi-7-overview/
[link-16]: https://flakm.com/posts/phantom_leak/
[link-17]: https://www.problemofnetwork.com/posts/vault-tls-with-network-appliances/
[link-18]: https://fedepaol.github.io/blog/2025/01/06/enabling-evpn-termination-with-podman-pods-as-systemd-units/
[link-19]: https://juliopdx.com/2024/11/25/codespaces-for-network-engineers-and-educators/
[link-20]: https://blog.danslimmon.com/2019/03/25/latency-and-throughput-optimized-clusters-under-load/
[link-29]: https://bsky.app/profile/scottslowe.bsky.social
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
