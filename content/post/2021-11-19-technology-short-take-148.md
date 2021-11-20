---
author: slowe
categories: Information
comments: true
date: 2021-11-19T21:25:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Kubernetes
- Linux
- Python
- Apple
- Intel
- Microsoft
- Encryption
- Envoy
- Docker
title: 'Technology Short Take 148'
url: /2021/11/19/technology-short-take-148/
---

Welcome to Technology Short Take #148, aka the Thanksgiving Edition (at least, for US readers). I've been scouring RSS feeds and various social media sites, collecting as many useful links and articles as I can find: from networking hardware and networking CI/CD pipelines to Kernel TLS and tricks for improving your working memory. That's quite the range! I hope that you find something useful here.<!--more-->

## Networking

* Here's [an article][link-1] on the recently-introduced `pwru` tool, which aims to help with tracing network packets in the Linux kernel. It seems like it may be a bit too debug-level to be useful to the average person, but I have yet to lay hands on it myself and find out for sure.
* Matt Mullen has a pretty lengthy (and informative) article on [using the Python `requests` module to work with REST APIs][link-3]. Good stuff here.
* While reading Tom Hollingsworth's article [regarding "slower" Wi-Fi on the M1-based MacBook Pro laptops][link-5] (a good read, BTW), he shared a link to this post about [drag racing the bus][link-6]. Hilarious---but very true. Also found via Tom's article was [this post on Wi-Fi throughput][link-7], which I found to be a very informative read.
* Ivan Pepelnjak [explains the concepts of graceful restart][link-18], and then goes on to discuss [graceful restart and routing protocol convergence][link-22].
* I haven't had a chance to read all the articles yet, but this six-part series (so far?) on building a network CI/CD pipeline looks to be quite informative. Start [here][link-30] with part 1.

## Servers/Hardware

* Ars Technica [takes a look at the new Intel "Alder Lake" CPUs][link-15]. Their conclusion: they're insanely fast, but also insanely power-hungry.

## Security

* Microsoft found a macOS vulnerability that could bypass System Integrity Protection (SIP). More details are in [this Microsoft blog post][link-8].
* It's unfortunate that a security researcher is having such a hard time with Apple's Security Bounty program; see [this article][link-20]. (Hat tip to [Michael Tsai][link-21].)

## Cloud Computing/Cloud Management

* For all the Internet's distributed nature, it's funny how some aspects still end up being dependent on smaller, less distributed things. Dark Reading [takes a look][link-16] at one such instance: the dependence on Let's Encrypt and what happened when one of their root certificates recently expired.
* Dean Lewis runs through [updating node resources when using TKG on vSphere][link-19] (the same basic process is used for AWS and Azure as well). Since machine templates are immutable, the key here (and what Dean illustrates in his article) is creating a new template and then pointing the MachineDeployment to the new template. This is all documented on the Cluster API site, but it's useful to see an example of it as found in Dean's article.
* Want to do a deep dive into the inner workings of custom resource validation in Kubernetes? Daniel Mangum [has you covered][link-24].
* Thinking of using Envoy's Lua filter? [This post][link-26] by Jean-Marie Joly has some great information.
* Kief Morris [tackles the "snowflakes as code" antipattern][link-27], where separate instances of infrastructure code are used to manage different environments.
* Michael Marie-Julie [shares some details][link-28] on how The Fork structures their AWS presence (and the tools they use).
* Joaquín Menchaca takes a closer look at [using Istio with AKS][link-31].

## Operating Systems/Applications

* Here's a neat trick (and tool) for [making custom folder icons on macOS][link-4].
* Hubert Maslowski crafted a post on [the Kerberos single-sign on app extension for macOS][link-9].
* I thought it was cool to come across some [OpenBSD][link-14]-related blog posts. First, APNIC has two posts on OpenBSD; [part 1][link-11] provides some background, while [part 2][link-12] provides the "why" in "Why use OpenBSD?" Both posts are apparently based on/adapted from [this post][link-13].
* The folks at NGINX talk about [using Kernel TLS (kTLS) to improve performance][link-25]. Unrelated side note: why is it "Kernel TLS" (note the capitalization) but is abbreviated "kTLS" (again, note the capitalization)?

## Virtualization

* Carlos Mestre del Pino talks about [automated deployment of Tanzu Community Edition on VMware Cloud on AWS][link-2] using Terraform and PowerCLI. (Hat tip to Robert Kloosterhuis for sharing this link in the vExpert Slack.)
* It's kind of neat to see other folks using mind maps, like [this mind map][link-23] that Eric Sloof created for vRealize Automation SaltStack Config (which, by the way, has got to be the most convoluted name for a piece of software).
* David Herron shares his motivation for---and experience with---[exchanging Docker Desktop for Multipass][link-29].

## Career/Soft Skills/Other

* Although [this article][link-10] is titled "What Kids Need to Know About Their Working Memory," I'd say the article is equally applicable to adults.
* Ever heard of the "tyranny of the urgent"? If not, [this post][link-17] is for you. Even if you have heard of it, you may find some useful tips!

I have more links and articles to share, but I'll stop here...for now, anyway! I do hope I've managed to share something useful for readers. If you have any questions or any comments, please don't hesitate to contact me. Reach out to [me on Twitter][link-99], or find me on any one of several Slack communities (like [the Kubernetes Slack community][link-32]). Enjoy!

[link-1]: https://brainbit.io/posts/pwru/
[link-2]: https://www.mestredelpino.com/automated-tanzu-community-edition-deployment-on-vmware-cloud-on-aws
[link-3]: https://blog.networktocode.com/post/using-python-requests-with-rest-apis/
[link-4]: https://thisdevbrain.com/how-to-create-a-custom-macos-folder-icon/
[link-5]: https://networkingnerd.net/2021/11/05/is-the-m1-macbook-pro-wi-fi-really-slower/
[link-6]: https://sc-wifi.com/2014/07/08/drag-racing-the-bus/
[link-7]: https://divdyn.com/wi-fi-throughput/
[link-8]: https://www.microsoft.com/security/blog/2021/10/28/microsoft-finds-new-macos-vulnerability-shrootless-that-could-bypass-system-integrity-protection/
[link-9]: https://hmaslowski.com/home/f/kerberos-single-sign-on-sso-app-extension-for-macos
[link-10]: https://intrepidednews.com/what-kids-need-to-know-about-their-working-memory-deborah-farmer-kris/
[link-11]: https://blog.apnic.net/2021/10/28/openbsd-part-1-how-it-all-started/
[link-12]: https://blog.apnic.net/2021/11/05/openbsd-part-2-why-use-openbsd/
[link-13]: https://bsdly.blogspot.com/2021/09/what-every-it-person-needs-to-know.html
[link-14]: https://www.openbsd.org/
[link-15]: https://arstechnica.com/gadgets/2021/11/intels-alder-lake-big-little-cpu-design-tested-its-a-barn-burner/
[link-16]: https://www.darkreading.com/dr-tech/how-to-avoid-another-lets-encrypt-like-meltdown
[link-17]: https://toggl.com/blog/tyranny-of-the-urgent
[link-18]: https://blog.ipspace.net/2021/09/graceful-restart.html
[link-19]: https://veducate.co.uk/tkg-kubectl-scale-vertically/
[link-20]: https://habr.com/en/post/579714/
[link-21]: https://mjtsai.com/blog/2021/10/29/denis-tokarevs-four-zero-days/
[link-22]: https://blog.ipspace.net/2021/10/graceful-restart-convergence.html
[link-23]: https://www.ntpro.nl/blog/archives/3649-SaltStack-Mind-Map.html
[link-24]: https://danielmangum.com/posts/how-kubernetes-validates-custom-resources/
[link-25]: https://www.nginx.com/blog/improving-nginx-performance-with-kernel-tls/
[link-26]: https://medium.com/safetycultureengineering/edge-routing-with-envoy-and-lua-621f3d776c57
[link-27]: https://infrastructure-as-code.com/book/2021/11/19/snowflakes-as-code.html
[link-28]: https://medium.com/thefork/creating-our-piece-of-cloud-in-aws-fd4e30571682
[link-29]: https://techsparx.com/software-development/docker/docker-desktop/multipass.html
[link-30]: https://juliopdx.com/2021/10/20/building-a-network-ci-cd-pipeline-part-1/
[link-31]: https://joachim8675309.medium.com/istio-service-mesh-on-aks-1b6ed16f6890
[link-32]: https://kubernetes.slack.com
[link-99]: https://twitter.com/scott_lowe
