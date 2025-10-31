---
author: slowe
categories: Information
comments: true
date: 2025-10-31T09:00:00-04:00
tags:
- AWS
- EVPN
- Git
- Hardware
- Kubernetes
- Networking
- Pulumi
- RedHat
- Security
- Storage
- Terraform
- Virtualization
- VXLAN
- Xen
title: 'Technology Short Take 189'
url: /2025/10/31/technology-short-take-189/
---

Welcome to Technology Short Take #189, Halloween Edition! OK, you caught me---this Tech Short Take is not scary. I'll try harder next year. In the meantime, enjoy this collection of links about data center-related technologies. Although this installation is lighter on content than I would prefer, I am publishing anyway in the hopes of trying to get back to a somewhat-regular cadence. Here's hoping you find something useful and informative!<!--more-->

## Networking

* Kevin Myers dissects [EVPN/VXLAN interoperability with MicroTik and IP Infusion][link-14].
* Ivan Pepelnjak has a new project: [open source EVPN/VXLAN labs][link-15]. I plan to tackle these myself in the near future!

## Servers/Hardware

* Kevin Houston takes a look at [the availability of Xeon 6 CPUs across blade server vendors][link-1].

## Security

* Security researchers recently published some research on a new microarchitectural exploit called "VMScape." The TL;DR on VMScape is that it allows hypervisor information to leak from a malicious VM. Oops! Olivier Lambert has [a write-up that explains why the Xen hypervisor is not affected by this exploit][link-5]. (Side note: be sure to read the comments---Olivier shares some useful information there.)
* The leaking of source code for F5 appliances by a "nation-state affiliated cyber threat actor" has lead [the CISA to call on all federal agencies to mitigate vulnerabilities in F5 appliances][link-6] due to "an imminent threat to federal networks using F5 devices and software." That's not good.
* In early October of this year, Red Hat [confirmed the breach of one of its GitLab instances][link-10], leading to the theft of nearly 570GB of data across 28,000 repositories. Oof.

## Cloud Computing/Cloud Management

* Iain Smart [reviews some common exploit paths when trying to build multi-tenant Kubernetes clusters][link-4]. As Iain says, multi-tenancy is hard.
* Digital Society discusses [their migration off AWS and Digital Ocean to Hetzner][link-7].
* Just this past week I learned of [Pulumi's resource hooks][link-9], a feature made available back in June of this year.
* Hrittik Roy [discusses vCluster Standalone][link-11], a multi-tenancy solution for Kubernetes that now no longer requires an underlying host cluster.
* Mattis Fjellström discusses Terraform-related options for [managing tags in AWS][link-12].

## Operating Systems/Applications

* Do you, like Cory Doctorow, believe [an AI economic apocalypse is near][link-2]?
* I recently published [a post on Git pre-commit hooks][xref-1]. The next day I wake up to an email from Ivan Pepelnjak, who has---not surprisingly---already explored Git pre-commit hooks and found [a framework for making them easier to work with][link-3]. Ivan uses this framework to perform a variety of checks against his blog posts. Now he has inspired me to go even farther with my pre-commit script!
* Anthropic, along with the UK AI Institute and the Alan Turing Institute, have found that as few as 250 malicious documents can produce a "backdoor" vulnerability in a large language model (LLM). You can get more details, as well as a link to the paper published from their research, by reading [the associated Anthropic blog post][link-8].
* Here is [a handy tool to generate a Mermaid diagram from your Terraform configuration][link-13].
* Familiar with `pwru`? If not, read this [guide to getting started with `pwru`][link-16].

That's it for this time around. As always, I welcome your feedback---I'd love to hear from you! Feel free to reach me on [Twitter/X][link-97], [Mastodon][link-98], or [Bluesky][link-99]. Or email me, that is fine too (my address is here on the site, it is not too hard to find). I truly do enjoy hearing from readers. You can also find me in a variety of Slack communities, so feel free to DM me there if you would prefer. Thanks for reading!

[link-1]: https://bladesmadesimple.com/2025/09/xeon-6-cpus-available-today/
[link-2]: https://pluralistic.net/2025/09/27/econopocalypse/#subprime-intelligence
[link-3]: https://pre-commit.com/
[link-4]: https://blog.amberwolf.com/blog/2025/september/kubernetes_namespace_boundaries/
[link-5]: https://virtualize.sh/blog/vmscape-and-why-xen-dodged-it/
[link-6]: https://www.cisa.gov/news-events/alerts/2025/10/15/cisa-directs-federal-agencies-mitigate-vulnerabilities-f5-devices
[link-7]: https://digitalsociety.coop/posts/migrating-to-hetzner-cloud/
[link-8]: https://www.anthropic.com/research/small-samples-poison
[link-9]: https://www.pulumi.com/blog/resource-hooks/
[link-10]: https://www.bleepingcomputer.com/news/security/red-hat-confirms-security-incident-after-hackers-breach-gitlab-instance/
[link-11]: https://www.vcluster.com/blog/vcluster-standalone-multi-tenancy-kubernetes
[link-12]: https://mattias.engineer/blog/2025/managing-aws-tags/
[link-13]: https://github.com/RoseSecurity/Terramaid
[link-14]: https://stubarea51.net/2025/09/22/evpn-vxlan-interop-ipv4-ipv6-mikrotik-ip-infusion/
[link-15]: https://blog.ipspace.net/2025/10/vxlan-evpn-labs/
[link-16]: https://bitpuff.io/tutorials/networking/pwru/intro/
[link-97]: https://x.com/scott_lowe
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://bsky.app/profile/scottslowe.bsky.social
[xref-1]: {{< relref "2025-10-20-using-git-pre-commit-hooks.md" >}}
