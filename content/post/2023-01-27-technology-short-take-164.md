---
author: slowe
categories: Information
comments: true
date: 2023-01-27T09:22:00-06:00
tags:
- Apple
- AWS
- Azure
- DevOps
- Encryption
- ESXi
- Git
- Go
- Hardware
- macOS
- Microsoft
- Networking
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- VMware
- YAML
title: 'Technology Short Take 164'
url: /2023/01/27/technology-short-take-164/
---

Welcome to Technology Short Take #164! I've got another collection of links to articles on networking, security, cloud, programming, and career development---hopefully you find something useful!<!--more-->

## Networking

* William Morgan's [2022 service mesh recap][link-3] captures some of the significant events in service mesh in 2022, although through a Linkerd-colored lens. I do agree that the synergy between service mesh and the Gateway API was a surprise for a lot of folks, but they really are a good match.
* Back in October of last year, Tom Hollingsworth [weighed in on Hedgehog][link-10], the networking company that has set out to commercialize SONiC, a Linux-based NOS used extensively in Azure.
* Ah, [the bygone sounds of yesteryear][link-23]...what a blast from the past!

## Servers/Hardware

* What do you think of [the ThinkPhone][link-1]? (Hat tip to James Kane for bringing this to my attention.)
* I just found this article buried in my list of "articles to include in a future TST": it's [a list of blade server resources][link-12] from "blade server guy" Kevin Houston.

## Security

* Adrian Mouat reviews some of [the security-focused changes in Go 1.20][link-17].
* This article on using [`osquery`][link-27] for [behavioral detection of macOS malware][link-26] was an interesting read.

## Cloud Computing/Cloud Management

* I'm confident most readers have seen more YAML than they would care to see, but I suspect most readers don't fully understand the complexity of YAML. Read [this article][link-6] and you will be enlightened.
* Julien Deswaef argues that [your organization should run its own Mastodon server][link-13] (advice that flies in the face of "outsourcing" non-critical things like e-mail and such to other providers).
* This article by Brian Caffey on [deploying the same web application on ECS Fargate using three different IaC tools][link-16] (CDK, Terraform, and Pulumi) has some really great and useful information in it. Well worth reading, in my opinion.
* Nick Frichette shares some details gathered through a deeper [look at AWS API protocols][link-30].
* Cool to see Koyeb release a Pulumi provider, so that you can use Pulumi programs to interact with Koyeb. You can get more details on Koyeb's Pulumi provider in [this blog post][link-31].

## Operating Systems/Applications

* Dewan Ahmed has [a great run-down on options][link-7] for documentation-as-code.
* Jeff Johnson shows [how to log some information][link-8] being sent between macOS and Apple.
* I'm picking up _so many_ good Git tips from Nick Janetakis. The latest one is [searching all of your Git history][link-15] using `git log`.
* Ever asked yourself "Why Kustomize?" [This article][link-18] may be helpful.
* Marius Sandbu took the time to create [a graphical overview of Azure AD][link-19].
* For something a bit on the lighter side, here's Sean Massey's explanation of [automating Minecraft server builds][link-20].
* [This trick][link-22] for setting Git's `user.email` setting is incredibly useful. (Massive kudos to Patrick Kelso for sharing this tip in one of the Slack communities I frequent!)
* I'm not sure I ever expected to see "regular expression" and "modern" in the same description, but [here it is][link-24]. Anyone played with this yet?
* It would seem that AWS is going to take on Docker Desktop with its Finch project; see [this blog post][link-25] for more details.
* Rory McCune shares some [information on configuring Caddy][link-28], a Go-based open source web server. I played around a bit with [Caddy][link-29] last year, and generally liked it.

## Storage

* Stephen Wagner discusses a situation in which [all VMs are inaccessible after graceful cluster shutdown and restart][link-2].
* Here's a slightly older post from September 2022, in which Chris Evans discusses [Weka 4 and it's positioning as a "data platform."][link-11]

## Programming

* [Here][link-21] is Raul Jordan's list of Rust concepts he wishes he'd learned earlier.

## Virtualization

* Steven Bright shows readers [how to back up their VMware ESXi TPM encryption recovery keys][link-9].
* Jason Boche [shares the resolution][link-14] to an error he encountered upgrading vCenter Server from version 7.0.3 to version 8.0. The error pertained to a mismatched FQDN, where in this instance the mismatch was due to uppercase versus lowercase (it was the former and needs to be the latter). Read his post for full details.

## Career/Soft Skills

* Ioannis Moustakis [shares 44 books for DevOps, site reliability engineering (SRE), and cloud engineers][link-4]. There are some familiar titles here, as well as a number of titles that I did not recognize, and I appreciate the inclusion of some business- and career-focused titles as well. 
* I believe the central thesis of [this article][link-5] to be true: we do our best work when we truly focus.

That's all for now! If you found something useful here, feel free to share a link to this post via your social media channels. I'd also love to hear from you if you have any feedback on how I might improve posts like this (or other content); feel free to contact me [on Twitter][link-99], [on Mastodon][link-98], or on Slack (the [Kubernetes][link-96] and [Pulumi][link-97] Slack communities are where you'll find me most often). Thanks for reading!

[link-1]: https://www.theverge.com/2023/1/7/23543213/lenovo-thinkphone-by-motorola-smartphone-thinkpad
[link-2]: https://www.stephenwagner.com/2023/01/08/vmware-vsan-all-vms-inaccessible-cluster-shutdown-restart/
[link-3]: https://linkerd.io/2022/12/28/service-mesh-2022-recap-ebpf-gateway-api/
[link-4]: https://spacelift.io/blog/devops-books
[link-5]: https://randsinrepose.com/archives/your-best-work-2/
[link-6]: https://ruudvanasseldonk.com/2023/01/11/the-yaml-document-from-hell
[link-7]: https://dev.to/dewanahmed/markdown-asciidoc-or-restructuredtext-a-tale-of-docs-as-code-3nj9
[link-8]: https://lapcatsoftware.com/articles/logging-https.html
[link-9]: https://www.stevenbright.com/2023/01/backing-up-vmware-esxi-tpm-encryption-recovery-keys/
[link-10]: https://networkingnerd.net/2022/10/14/hedgehog-the-network-os-distro/
[link-11]: https://www.architecting.it/blog/weka4-data-platform/
[link-12]: http://www.bladesmadesimple.com/2022/10/the-ultimate-guide-for-blade-server-resources/
[link-13]: https://martinfowler.com/articles/your-org-run-mastodon.html
[link-14]: http://www.boche.net/blog/2022/11/07/vcenter-8-upgrade-and-vami_config_net/
[link-15]: https://nickjanetakis.com/blog/search-all-of-your-git-history-for-code-and-commits-with-2-commands
[link-16]: https://briancaffey.github.io/2023/01/07/i-deployed-the-same-containerized-serverless-django-app-with-aws-cdk-terraform-and-pulumi/
[link-17]: https://whyk8s.substack.com/p/why-kustomize
[link-18]: https://www.chainguard.dev/unchained/go-1-20-is-coming-and-it-brings-even-more-security-by-default
[link-19]: https://msandbu.org/azure-active-directory-security-overview/
[link-20]: https://thevirtualhorizon.com/2023/01/26/how-i-automated-minecraft-server-builds/
[link-21]: https://rauljordan.com/rust-concepts-i-wish-i-learned-earlier/
[link-22]: https://dev.to/absoftware/git-config-user-email-for-repositories-in-subdirectory-140k
[link-23]: https://www.windytan.com/2012/11/the-sound-of-dialup-pictured.html
[link-24]: https://pomsky-lang.org/
[link-25]: https://aws.amazon.com/blogs/opensource/introducing-finch-an-open-source-client-for-container-development/
[link-26]: https://unfinished.bike/behavioral-detection-of-macos-malware-using-osquery
[link-27]: https://www.osquery.io/
[link-28]: https://raesene.github.io/blog/2023/01/21/Fun-with-Caddy-SSRF-Testing/
[link-29]: https://caddyserver.com/
[link-30]: https://frichetten.com/blog/aws-api-protocols/
[link-31]: https://www.koyeb.com/blog/announcing-koyeb-pulumi-provider
[link-96]: https://kubernetes.slack.com
[link-97]: https://pulumi-community.slack.com
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
