---
author: slowe
categories: Information
comments: true
date: 2022-04-29T09:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- macOS
- Docker
- Git
- Pulumi
- Linux
- KVM
- OPA
- Istio
- Kubernetes
title: 'Technology Short Take 154'
url: /2022/04/29/technology-short-take-154/
---

Welcome to Technology Short Take #154! My link of links and articles from around the Internet is a bit light on networking and virtualization this time around, but heftier in the security, cloud, and OS/application sections. I hope that I've managed to include something that you'll find useful. Enjoy the content!<!--more-->

## Networking

* Lucas Pardue and Christopher Wood share [a primer on proxies][link-8]. (Hat tip to Matt Oswalt for putting this on my Twitter timeline.)
* Ivan Pepelnjak talks about the 1.2.1 release of netsim-tools in [this blog post][link-26].

## Servers/Hardware

* It seems like silicon photonics is something the industry has been talking about for years, but something that never seems to come to fruition. The Next Platform recently chatted with Andy Bechtolsheim about it; you can read the results of that discussion [here][link-6].

## Security

* Daniel Holback [talks about the addition of fuzzing to the Flux controllers][link-4], to improve the security posture of the Flux project.
* Wojciech Reguła [shares][link-5] a trick for bypassing the macOS TCC protection using older applications.
* It appears that [a well-known cryptomining botnet has started targeting Docker][link-9]. (Hat tip to Teri Radichel for sharing this and making it appear on my Twitter timeline.)
* GitHub recently shared details on [a Git security vulnerability][link-14].
* The idea of a software bill of materials (SBOM) is gaining traction; [here's an article][link-20] on a tool by Anchore that claims to be able to help.
* Liam Galvin shows [a couple of ways][link-24] that Rego (the language behind Open Policy Agent) can be used maliciously.

## Cloud Computing/Cloud Management

* Richard Seroter shares some data [comparing container size and startup latency for serverless apps][link-10] written in various programming languages.
* I came across [this post announcing Snowcat][link-22], a security scanner for Istio. I suspect that as service meshes continue to proliferate, I suspect we'll see more tools like this emerge.
* Here's [a walkthrough of FluxCD on Tanzu Community Edition][link-23]. Most of this, if not all of this, is also possible on non-Tanzu Kubernetes clusters; obviously the Tanzu CLI commands and the use of `kapp` (from Carvel) may be different, but the basic concepts are all the same.
* Vladislav Supalov [shares some things he wished he'd known earlier about Pulumi][link-25]. Nothing bad here, in my opinion, just things that come from using a general-purpose programming language instead of a more constrained domain-specific language.
* Also on the topic of Pulumi: Martin Thwaites discusses [using multiple Pulumi projects with custom backends][link-29].

## Operating Systems/Applications

* Scott Hanselman insists that [this one e-mail rule][link-1] will change your life.
* Julia Ferraioli [reminds][link-2] readers that the e-mail address you use in a Git repository does, in fact, matter.
* In the event you need to understand [why you should avoid `:latest` when working with Docker images][link-3], David Norton has you covered. I don't know about it killing puppies, but it's definitely _not_ a good idea.
* Jorge Castro encourages folks to [just kill the silly myths][link-11] (regarding desktop Linux).
* Web views in macOS? See [here][link-15] for details on how to inspect them.
* Katarina Brookfield shares details with readers on [automating updates to her Hugo-based site using GitLab CI/CD][link-16].
* Poorna Gaddehosur has an update on [getting Linux-based eBPF programs to run with eBPF on Windows][link-17].
* When I first saw [this article][link-19], I was thinking DRM was "Digital Rights Management" and was I really caught off-guard. (Hint: DRM means something entirely different in the context of the article.)
* Andreas Sommer shares some [best practices for Grafana dashboards][link-21].

## Storage

* Howard Oakley has [a great post explaining FileVault][link-12]. (Hat tip to Stephen Foskett for putting this in my Twitter timeline.)
* Also from Howard, [here's an explanation][link-13] of why you may be getting less-than-promised storage throughput for USB-C storage on your M1-based Macs.

## Virtualization

* Eric Sloof has some information on [how to configure the vRealize Orchestrator plugin for vCloud Director][link-28].
* QEMU 7.0 was recently released; [here's][link-30] Michael Larabel's write-up for Phoronix.

## Career/Soft Skills

* Robin Moffatt, a developer advocate at Confluent, recently wrote about [hanging up his boarding passes and jetlag (for now)][link-7]. I have a lot of respect for Robin taking the time to do some serious introspection, and deciding to put family first. Kudos.
* I love [this motivational post][link-27] by Ivan Pepelnjak on why you (yes, you!) should keep blogging. And thanks for the shout-out at the end of that post, Ivan!

That's all for now! I'll be back in a couple weeks with another Technology Short Take, but until then feel free to contact me if you have links to share, interesting articles you've found, or if you just want to say hello! You can find me on any number of Slack communities, or you can contact [me on Twitter][link-99]. Thanks for reading!

[link-1]: https://www.hanselman.com/blog/one-email-rule-have-a-separate-inbox-and-an-inbox-cc-to-reduce-email-stress-guaranteed
[link-2]: https://www.juliaferraioli.com/blog/2022/your-git-email-matters/
[link-3]: https://platformers.dev/log/2022/latest-literally-kills-puppies/
[link-4]: https://fluxcd.io/blog/2022/02/security-more-confidence-through-fuzzing/
[link-5]: https://wojciechregula.blog/post/macos-red-teaming-bypass-tcc-with-old-apps/
[link-6]: https://www.nextplatform.com/2022/03/10/talking-silicon-photonics-signal-and-noise-with-andy-bechtolsheim/
[link-7]: https://rmoff.net/2022/04/07/hanging-up-my-boarding-passes-and-jetlagfor-now/
[link-8]: https://blog.cloudflare.com/a-primer-on-proxies/
[link-9]: https://www.crowdstrike.com/blog/lemonduck-botnet-targets-docker-for-cryptomining-operations/
[link-10]: https://seroter.com/2022/04/18/measuring-container-size-and-startup-latency-for-serverless-apps-written-in-c-node-js-go-and-java/
[link-11]: https://www.ypsidanger.com/lets-just-kill-the-silly-myths/
[link-12]: https://eclecticlight.co/2022/04/23/explainer-filevault/
[link-13]: https://eclecticlight.co/2022/04/18/m1-thunderbolt-ports-dont-fully-support-usb-3-1-gen-2/
[link-14]: https://github.blog/2022-04-12-git-security-vulnerability-announced/
[link-15]: https://blog.jim-nielsen.com/2022/inspecting-web-views-in-macos/
[link-16]: http://advecti.io/2022/13-automating-hugo-k8s-deployment-with-gitlab-cicd/
[link-17]: https://cloudblogs.microsoft.com/opensource/2022/02/22/getting-linux-based-ebpf-programs-to-run-with-ebpf-for-windows/
[link-18]: https://blog.dowhile0.org/2022/04/22/fedora-36-a-brave-new-drm-kms-only-world/
[link-19]: https://blog.dowhile0.org/2022/04/22/fedora-36-a-brave-new-drm-kms-only-world/
[link-20]: https://anchore.com/sbom/how-to-generate-an-sbom-with-free-open-source-tools/
[link-21]: https://andidog.de/blog/2022-04-21-grafana-dashboards-best-practices-dashboards-as-code
[link-22]: https://www.praetorian.com/blog/introducing-snowcat/
[link-23]: https://www.vxav.fr/2022-04-23-fluxcd-in-tanzu-community-edition-0.12/
[link-24]: https://lia.mg/posts/malicious-rego/
[link-25]: https://vsupalov.com/pulumi-learnings/
[link-26]: https://blog.ipspace.net/2022/04/netsim-tools-release-1.2.1.html
[link-27]: https://blog.ipspace.net/2022/04/keep-blogging.html
[link-28]: https://www.ntpro.nl/blog/archives/3668-Configure-the-vRealize-Orchestrator-Plug-in-for-vCloud-Director.html
[link-29]: https://martinjt.me/2021/03/04/pulumi-multiple-projects-with-custom-backends/
[link-30]: https://www.phoronix.com/scan.php?page=news_item&px=QEMU-7.0-Released
[link-99]: https://twitter.com/scott_lowe
