---
author: slowe
categories: Information
comments: true
date: 2022-08-12T09:00:00-06:00
tags:
- AWS
- BGP
- DNS
- Docker
- ESXi
- Fusion
- GitHub
- Hardware
- Kubernetes
- Linux
- macOS
- Networking
- NSX
- Security
- Storage
- Terraform
- Virtualization
- VMware
title: 'Technology Short Take 158'
url: /2022/08/12/technology-short-take-158/
---

Welcome to Technology Short Take #158! What do I have in store for you this time around? Well, you'll have to read the whole article to find out for sure, but I have links to articles on...well, lots of different topics! DNS, BGP, hardware-based security, Kubernetes, Linux---they're all in here. Hopefully I've managed to find something useful for someone.<!--more-->

## Networking

* Shank Mohan explains some details on [NSX-T Tier-1 Service Router (SR) placement][link-6].
* See this page for instructions on [how to enable BBR on Debian 10][link-11].
* Christopher Vierheller supplies a concise guide on [how to add custom symbols to EVE-NG][link-16].
* Alex Neihaus provides readers with a walkthrough for [adding BGP routes to AWS security groups][link-23].
* [Via Russ White][link-28], [RFC 9199][link-29] was drawn to my attention ("Considerations for Large Authoritative DNS Server Operators").

## Servers/Hardware

* Gabriel Sieben ruminates on [the potential dangers of Microsoft Pluton][link-21], a new security chip co-developed by Microsoft and AMD.

## Security

* Jason Avery walks readers through [using Atomic Red Team to test Falco rules in Kubernetes environments][link-1]. This is pretty neat.
* Also on the Sysdig blog, Miguel Hernández recaps his KubeCon EU 2022 talk on [how attackers used an exposed Prometheus server to exploit Kubernetes clusters][link-2].
* Kat Traxler discusses [abusing the AWS S3 replication service to exfiltrate data][link-5].
* A "highly evasive" Linux malware named OrBit has emerged; see [here][link-12] for more details.
* [This article][link-31] is an interesting discussion on how to treat failure when it comes to breaches or security vulnerabilities. I like the author's focus on keeping the discussion positive.

## Cloud Computing/Cloud Management

* There's a fair chance you may not have had the opportunity to work with the `kubectl patch` command, but never fear: Matt Bargenquast [has you covered][link-4].
* Mehul Arora [discusses ephemeral containers][link-9], some neat new functionality that should hit the stable status in Kubernetes 1.25.
* Chip Zoller shows readers how to combine a few different components for [attesting image scans][link-15] (to protect against vulnerabilities).
* Ivan Velichko takes readers on a journey of [how Kubernetes reinvented virtual machines (in a good sense)][link-17].
* Lee Briggs has [a bit of a rant][link-18] on imperative, declarative, and idempotent, and why this seems to confuse a lot of people.
* Eduardo Minguez has an article on the Sysdig blog on [how to apply security at the source using GitOps][link-20].
* Piotr Minkowski takes readers through [managing Kubernetes clusters with Terraform and ArgoCD][link-22].
* Robert Guske [shares][link-25] a couple of scripts that are useful for demoing Knative.
* Ricard Bejerano makes the case that [Terraform should have remained stateless][link-27]. I'm not sure I agree with some of his points...but I need to ponder the topic a bit more.

## Operating Systems/Applications

* Jorge Castro tears into [the "wasting disk space" argument][link-7] against the Flatpak application package format.
* Interested in hardening macOS? See [this list of guidelines][link-8] from Richard Bejarano.
* Here's an article on [updating running Docker containers using Watchtower][link-13].

## Programming

* Are you ready to [give up GitHub][link-19]?

## Storage

* For your fix of storage-related links, J Metz has relatively [recently published Storage Short Take #47][link-14].

## Virtualization

* Edwin Weijdema [shares][link-3] how he uses VMware Fusion 12 on his MacBook Pro (Intel-based, I'm assuming) to run a mobile lab with nested ESXi hypervisors.
* Howard Oakley takes a look at [virtualization on Apple silicon Macs][link-10].
* William Lam confirms that [ESXi 7.x will be the last version to officially support macOS virtualization][link-26].

## Career/Soft Skills

* I don't remember how [this article][link-24] got in front of me, but I wanted to share it here because I know that job burnout is real. If this article helps even one person, then it will be worth including it here.
* I appreciated this post on [learning a technical subject][link-30]. A lot of this resonated with me; I'd be curious to know if readers feel the same way.

And that's a wrap! Thanks for reading, and feel free to reach out to me if you have any feedback, corrections (mistakes do creep in from time to time!), suggestions for improvement, or links you think I should include in the next Technology Short Take. You can reach [me on Twitter][link-99], or find me in any number of Slack communities (the Kubernetes Slack community is one I frequently visit, among others).

[link-1]: https://sysdig.com/blog/atomic-red-team-falco/
[link-2]: https://sysdig.com/blog/exposed-prometheus-exploit-kubernetes-kubeconeu/
[link-3]: https://vmguru.com/2022/07/how-to-create-a-mobile-lab-with-vmware-fusion/
[link-4]: https://bargenqua.st/posts/kubectl-patching/
[link-5]: https://www.vectra.ai/blogpost/abusing-the-replicator-silently-exfiltrating-data-with-the-aws-s3-replication-service
[link-6]: https://www.lab2prod.com.au/2022/07/nsx-t-determinstic-sr-placement.html
[link-7]: https://www.ypsidanger.com/wasting-disk-space/
[link-8]: https://www.bejarano.io/hardening-macos/
[link-9]: https://metalbear.co/blog/getting-started-with-ephemeral-containers/
[link-10]: https://eclecticlight.co/2022/07/04/virtualisation-on-apple-silicon-macs-1-how-well-does-it-work/
[link-11]: https://wiki.crowncloud.net/?How_to_enable_BBR_on_Debian_10
[link-12]: https://www.esecurityplanet.com/threats/evasive-linux-malware/
[link-13]: https://ostechnix.com/automatically-update-running-docker-containers/
[link-14]: https://jmetz.com/2022/07/storage-short-take-47/
[link-15]: https://neonmirrors.net/post/2022-07/attesting-image-scans-kyverno/
[link-16]: https://packet-warrior.net/p/how-to-add-custom-symbols-in-eve-ng/
[link-17]: https://iximiuz.com/en/posts/kubernetes-vs-virtual-machines/
[link-18]: https://leebriggs.co.uk/blog/2022/07/20/nobody-knows-what-declarative-is
[link-19]: https://sfconservancy.org/blog/2022/jun/30/give-up-github-launch/
[link-20]: https://sysdig.com/blog/gitops-iac-security-source/
[link-21]: https://gabrielsieben.tech/2022/07/25/the-power-of-microsoft-pluton-2/
[link-22]: https://piotrminkowski.com/2022/06/28/manage-kubernetes-cluster-with-terraform-and-argo-cd/
[link-23]: https://www.yobyot.com/aws/bgp-routes-aws-security-group/2022/08/02/
[link-24]: https://commoncog.com/g/burnout/
[link-25]: https://rguske.github.io/post/demoing-knative-serving-and-eventing-using-demo-magic-scripts/
[link-26]: https://williamlam.com/2022/08/vsphere-esxi-7-x-will-be-last-version-to-officially-support-apple-macos-virtualization.html
[link-27]: https://www.bejarano.io/terraform-stateless/
[link-28]: https://rule11.tech/rfc9199-lessons-in-large-scale-service-deployment/
[link-29]: https://www.ietf.org/rfc/rfc9199.html
[link-30]: https://muratbuffalo.blogspot.com/2021/12/learning-technical-subject.html
[link-31]: https://blog.engyak.co/2022/07/the-role-of-trust-and-failure-in.html
[link-99]: https://twitter.com/scott_lowe
