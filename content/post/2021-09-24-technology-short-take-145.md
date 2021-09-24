---
author: slowe
categories: Information
comments: true
date: 2021-09-24T12:00:00-06:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Envoy
- NSX
- Cisco
- Apple
- Azure
- Microsoft
- AWS
- VMware
- Kubernetes
- macOS
- Ubuntu
- Docker
title: 'Technology Short Take 145'
url: /2021/09/24/technology-short-take-145/
---

Welcome to Technology Short Take #145! What will you find in this Tech Short Take? Well, let's see...stuff on Envoy, network automation, network designs, M1 chips (and potential open source variants!), a bevy of security articles (including a couple on very severe vulnerabilities), Kubernetes, AWS IAM, and so much more! I hope that you find something useful here. Enjoy!<!--more-->

## Networking

* Adam Kotwasinski walks readers through [deploying Envoy and Kafka to collect broker-level metrics][link-1].
* Ivan Pepelnjak [shares some links and thoughts][link-8] on configuring the NSX-T firewall with a CI/CD pipeline built on GitHub Actions and Terraform Cloud.
* Author "JulioPDX" (I couldn't find the author's real name on any of their online profiles) has an article on [integrating Nornir with FastAPI][link-15].
* Vincent Bernat provides [some feedback on Cisco pyATS and Genie Parser][link-17].
* Russ White shares some [thoughts on the collapsed spine][link-21] network design.
* Justin Pietsch talks about [simplifying networks and the resulting engineering trade-offs][link-30].

## Servers/Hardware

* Howard Oakley of The Eclectic Light Company [discusses some details][link-9] on Apple's M1 chip and what it does differently than other chips. Also included in this post are links to other articles with even more details---very helpful.
* Are open source M1-style chips a possibility? [This article][link-27] seems to think so.

## Security

* The last several weeks haven't been very nice to Azure with respect to security issues. First there was a vulnerability in the CosmosDB database that, according to [this Reuters article][link-4], exposed "keys that control access to databases held by thousands of companies." Following that incident came [news][link-5] of "Azurescape," billed as the first cross-account container takeover in the public cloud. Finally, I recently saw [this news about a "minor privilege escalation"][link-6] within Azure AD.
* Colm MacCárthaigh [discusses AWS SIGv4 and SIGv4A][link-18] and some of the details and differences between the two.
* The AWS WorkSpaces client had a remote code execution flaw (versions before 3.1.9 are affected). See more details [here][link-19].
* This isn't good. Better patch your vCenter Server instances, as VMware released [a security advisory with a long list of CVEs][link-22], including one with a severity score of 9.8/10.

## Cloud Computing/Cloud Management

* For reasons that I won't go into here (maybe later), I was recently pointed toward the vcluster project (for running virtual Kubernetes clusters in a namespace of an actual cluster). See [the vcluster web site][link-2] or [the GitHub repository][link-3] for more information. If I do end up using/testing it, I'll share more information via a future blog post.
* Nathan Peck has a really interesting article on [using Inlets for access to ECS Anywhere from anywhere][link-10].
* Sebastian Radloff has a two-part series on Gatekeeper and Kubernetes ([part 1][link-12], [part 2][link-13]). Although I've been doing some work with [Open Policy Agent (OPA)][link-14] and Envoy, I'm still wrapping my head around Gatekeeper and how all the CRDs fit together.
* Eric Shanks outlines how to [configure a private registry for Tanzu Kubernetes Clusters][link-23].
* Lydia Leong takes aim at the saying "The cloud is just someone else's computer" in [this post on cloud risk and resilience][link-24].
* Ben Kehoe [takes AWS to task][link-25] for shortcomings in the AWS IAM documentation. He also takes some time to explain [principals in AWS IAM][link-26], apparently in a single-handed effort to address some of the documentation woes. Thank you, Ben!

## Operating Systems/Applications

* This is a quite old post (from 2014!), but it may still be useful for folks who are now switching over to macOS: [here's a list of eight terminal-based utilities the author feels every user should know][link-7].
* James Kindon has [a post on Citrix UPM and Microsoft FSLogix][link-11]. This space is _way_ outside my area of expertise, but in reviewing James' article he provides some guidelines to help folks decide which of these two solutions is most appropriate based on certain requirements.
* Canonical has announced an extension to the lifecycle for Ubuntu 14.04 LTS "Trusty Tahr" and Ubuntu 16.04 LTS "Xenial Xerus", making it a total of ten years. See [their announcement][link-20] for details.
* Quentin Monnet has [a post on `bpftool`][link-29] that aims to expose some commands and features of using this tool for working with eBPF.

## Storage

* Rather than trying to curate my own list of storage-related links this time around, I'll point you to [this list instead][link-28], curated by none other than Dr. J Metz himself.

## Virtualization

* Arnon Rotem-Gal-Oz writes about [replacing Docker Desktop with HyperKit and Minikube][link-16].

That's all for this time around! If you have any feedback for me---additional sites I should monitor for content, or other topics I don't cover that you think would be useful to readers---I'd love to hear from you! The easiest way to get [in touch with me is via Twitter][link-99], but I'm also accessible via e-mail (my address isn't too hard to find) or Slack (I frequent several different Slack communities). Feel free to reach out.

[link-1]: https://adam-kotwasinski.medium.com/deploying-envoy-and-kafka-8aa7513ec0a0
[link-2]: https://www.vcluster.com/
[link-3]: https://github.com/loft-sh/vcluster
[link-4]: https://www.reuters.com/technology/exclusive-microsoft-warns-thousands-cloud-customers-exposed-databases-emails-2021-08-26/
[link-5]: https://unit42.paloaltonetworks.com/azure-container-instances/
[link-6]: https://www.netspi.com/blog/technical/cloud-penetration-testing/escalating-azure-privileges-with-the-log-analystics-contributor-role/
[link-7]: http://www.mitchchn.me/2014/os-x-terminal/
[link-8]: https://blog.ipspace.net/2021/09/nsxt-firewall-ci-cd-pipeline.html
[link-9]: https://eclecticlight.co/2021/08/24/whats-in-an-m1-chip-and-what-does-it-do-differently/
[link-10]: https://nathanpeck.com/ingress-to-ecs-anywhere-from-anywhere-using-inlets/
[link-11]: https://jkindon.com/2021/09/16/citrix-upm-and-fslogix-containers/
[link-12]: https://itnext.io/running-gatekeeper-in-kubernetes-and-writing-policies-part-1-fcc83eba93e3
[link-13]: https://medium.com/@sebradloff/running-and-writing-gatekeeper-policies-in-kubernetes-part-2-1c49c1c683b2
[link-14]: https://www.openpolicyagent.org/docs/latest/
[link-15]: https://juliopdx.com/2021/09/01/integrating-nornir-with-fastapi/
[link-16]: https://arnon.me/2021/09/replace-docker-with-minikube/
[link-17]: https://vincent.bernat.ch/en/blog/2021-pyats-genie-parser
[link-18]: https://shufflesharding.com/posts/aws-sigv4-and-sigv4a
[link-19]: https://rhinosecuritylabs.com/aws/cve-2021-38112-aws-workspaces-rce/
[link-20]: https://ubuntu.com/blog/ubuntu-14-04-and-16-04-lifecycle-extended-to-ten-years
[link-21]: https://rule11.tech/thoughts-on-the-collapsed-spine/
[link-22]: https://www.vmware.com/security/advisories/VMSA-2021-0020.html
[link-23]: https://theithollow.com/2021/09/22/configure-a-private-registry-for-tanzu-kubernetes-clusters/
[link-24]: https://cloudpundit.com/2021/09/22/the-cloud-is-not-just-someone-elses-computer/
[link-25]: https://ben11kehoe.medium.com/i-trust-aws-iam-to-secure-my-applications-i-dont-trust-the-iam-docs-to-tell-me-how-f0ec4c119e79
[link-26]: https://ben11kehoe.medium.com/principals-in-aws-iam-38c4a3dc322a
[link-27]: https://www.digitaltrends.com/computing/secrets-of-apple-m1-has-been-exposed-for-open-source/
[link-28]: https://jmetz.com/2021/09/storage-short-take-36/
[link-29]: https://qmonnet.github.io/whirl-offload/2021/09/23/bpftool-features-thread/
[link-30]: https://elegantnetwork.github.io/posts/simplify-networks-examples/
[link-99]: https://twitter.com/scott_lowe
