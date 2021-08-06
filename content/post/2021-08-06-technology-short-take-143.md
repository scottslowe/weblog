---
author: slowe
categories: Information
comments: true
date: 2021-08-06T08:45:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Envoy
- Kubernetes
- Go
- Intel
- AWS
- CAPI
- NSX
- Automation
- Docker
- CLI
- macOS
- ESXi
title: 'Technology Short Take 143'
url: /2021/08/06/technology-short-take-143/
---

Welcome to Technology Short Take #143! I have what I think is an interesting list of links to share with you this time around. Since taking my new job at Kong, I've been spending more time with Envoy, so you'll see some Envoy-related content showing up in this Technology Short Take. I hope this collection of links has something useful for you!<!--more-->

## Networking

* Here's a quick look at [using Envoy as a load balancer in Kubernetes][link-9].
* Back in April of this year, Patrick Ogenstad [announced Netrasp][link-16], a Go package for writing network automation tooling in Go.
* Eric Sloof shows readers [how to use the "Applied To" feature in NSX-T][link-20] to potentially improve resource utilization.
* Michael Kashin explains [how he built a DIY (Do It Yourself) SD-WAN using Envoy and Wireguard][link-21].

## Servers/Hardware

* Travis Downs [explores a recent Intel microcode update][link-12] that may have negatively impacted performance.

## Security

* I saw [this blog post][link-6] about [Curiefense][link-7], an open source Envoy extension to add WAF (web application firewall) functionality to Envoy. 
* [This post][link-8] on using SPIFFE/SPIRE, Kubernetes, and Envoy together shows how to implement mutual TLS (mTLS) for a simple application. As a learning resource, I thought this post was helpful. However, I wouldn't recommend trying to cobble together something like this for a production environment. If you need mTLS in production, use a service mesh that supports this sort of functionality.

## Cloud Computing/Cloud Management

* Jeremy Cowan shows [how to use Cluster API to provision an AWS EKS cluster][link-1].
* Hart Hoover drew my attention to [this post regarding API removals][link-3] in the upcoming Kubernetes 1.22 release.
* I really enjoy [these AWS open source news and updates posts][link-11]. The only way Ricardo could make it better would be by providing an RSS/Atom feed for the posts!
* John Arundel is [excited about CUE][link-14].
* Having recently needed to dig into [Open Policy Agent (OPA)][link-18], I took renewed interest in this slightly older article by Chip Zoller that [compares OPA/Gatekeeper with Kyverno][link-17]. Chip does a good job of calling out the advantages and disadvantages of each solution. If I had one criticism, it would be Chip's use of "programming language" instead of DSL for Rego---but that's truly nitpicking what otherwise is a very useful article for folks trying to determine which policy engine they should consider and evaluate.
* Via Alex Mitelman's [Systems Design Weekly 015][link-27], I was pointed to [this AWS article on multi-site active-active architectures][link-28]. This is a thorny topic full of design considerations, and this AWS article discusses a few of them. It's a good starting point for thinking about operating your own active-active architecture.

## Operating Systems/Applications

* This post on [decoding JWTs from the command-line][link-4] has a killer `jq` "incantation" (that's a perfect word for this command). Or, one could just use something like [this][link-5].
* It turns out that upgrading macOS on an M1-based Mac is perhaps a bit more complicated than it might seem. See [here][link-10] for more details.
* `git undo`? I can get onboard with that. More information is available [here][link-13].
* Nick Janetakis has a video that explains [why you should put braces around your variables when shell scripting][link-15].
* Justin Chadell introduces [support for heredocs inside Dockerfiles][link-19]. I can see this being enormously useful, at the cost of some (potential) added complexity. (Although, if we're fair, the lack of such support resulted in some complex workarounds on its own.)
* Greg Ferro shares [a useful CLI tip for increasing mouse tracking speed on macOS][link-24].

## Storage

* Chris Evans [evaluates the "HCI market segment"][link-22] following some recent industry moves. While the entire article is good, I particularly enjoyed this comment from Chris regarding "disaggregated HCI" (a term that is a pet peeve of mine): "...effectively breaking the model that HCI was initially meant to represent."
* Greg Schulz talks a bit about [the role of TCP Offload Engines (TOEs) in NVMe over Fabrics][link-26].

## Virtualization

* William Lam [takes a look][link-23] at running ESXi on the NUC 11 Extreme, aka "Beast Canyon." (Don't you just love product code names?)
* Frank Denneman reminds users that [CPU pinning is not an exclusive right to a CPU core][link-25].
* Via [William's post on `configstorecli`][link-29], I also saw this post by Duncan Epping on [renaming a virtual switch on vSphere 7.0U2 and higher][link-30].

## Career/Soft Skills

* Nick Korte [applies the idea of "cheat days" to your career][link-2].

And with that, I'll wrap this up. As always, I love to hear from readers, so feel free to engage with [me on Twitter][link-99] or find me on any one of a number of different Slack communities. Have a great weekend!

[link-1]: https://jicowan.medium.com/provisioning-an-eks-cluster-with-capi-df3502e5f2b4
[link-2]: https://blog.thenetworknerd.com/2021/07/13/no-cheat-days/
[link-3]: https://kubernetes.io/blog/2021/07/14/upcoming-changes-in-kubernetes-1-22/
[link-4]: https://prefetch.net/blog/2020/07/14/decoding-json-web-tokens-jwts-from-the-linux-command-line/
[link-5]: https://github.com/mike-engel/jwt-cli
[link-6]: https://www.reblaze.com/blog/adding-web-security-to-envoy/
[link-7]: https://www.curiefense.io/
[link-8]: https://zoemccormick.medium.com/getting-started-with-envoy-spiffe-and-kubernetes-32f34cda5f35
[link-9]: https://blog.markvincze.com/how-to-use-envoy-as-a-load-balancer-in-kubernetes/
[link-10]: https://eclecticlight.co/2021/07/18/last-week-on-my-mac-the-perils-of-m1-ownership/
[link-11]: https://blog.beachgeek.co.uk/posts/aws-open-source-news-and-updates-76-2e23/
[link-12]: https://travisdowns.github.io/blog/2021/06/17/rip-zero-opt.html
[link-13]: https://blog.waleedkhan.name/git-undo/
[link-14]: https://bitfieldconsulting.com/golang/cuelang-exciting
[link-15]: https://nickjanetakis.com/blog/why-you-should-put-braces-around-your-variables-when-shell-scripting
[link-16]: https://networklore.com/hello-netrasp/
[link-17]: https://neonmirrors.net/post/2021-02/kubernetes-policy-comparison-opa-gatekeeper-vs-kyverno/
[link-18]: https://www.openpolicyagent.org
[link-19]: https://www.docker.com/blog/introduction-to-heredocs-in-dockerfiles/
[link-20]: https://www.ntpro.nl/blog/archives/3621-Speed-up-your-NSX-T-firewall-with-Applied-to.html
[link-21]: https://networkop.co.uk/post/2021-02-diy-sdwan/
[link-22]: https://www.architecting.it/blog/hci-market-segment/
[link-23]: https://williamlam.com/2021/07/esxi-on-intel-nuc-11-extreme-beast-canyon.html
[link-24]: https://etherealmind.com/mouse-tracking-speed-on-macos-fixing/
[link-25]: https://frankdenneman.nl/2021/06/11/cpu-pinning-is-not-an-exclusive-right-to-a-cpu-core/
[link-26]: https://storageioblog.com/toe-nvmeof-tcp-performance-line-boost-performance-reduce-costs/
[link-27]: https://mitelman.engineering/system-design-weekly/015/
[link-28]: https://aws.amazon.com/blogs/architecture/disaster-recovery-dr-architecture-on-aws-part-iv-multi-site-active-active/
[link-29]: https://williamlam.com/2021/07/introduction-to-the-new-esxi-configuration-store-cli-configstorecli.html
[link-30]: https://www.yellow-bricks.com/2021/06/14/how-do-i-change-the-name-of-a-vswitch-with-vsphere-7-0-u2-and-higher/
[link-99]: https://twitter.com/scott_lowe
