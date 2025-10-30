---
author: slowe
categories: Information
comments: true
date: 2023-04-07T09:00:00-05:00
tags:
- AWS
- Azure
- Docker
- ESXi
- EVPN
- Hardware
- macOS
- Networking
- NSX
- Security
- SSH
- Storage
- Virtualization
- VMware
title: 'Technology Short Take 167'
url: /2023/04/07/technology-short-take-167/
---

Welcome to Technology Short Take #167! This Technology Short Take is a tad shorter than the typical one; I've been busy recently and my intake volume of content has gone down, thus resulting in fewer links to share with all of you! I opted to go ahead and publish a shorter Technology Short Take instead of making everyone wait around for a longer one. In any case, here's hoping that I've included something useful for you!<!--more-->

## Networking

* Jeff McLaughlin discusses what he calls [the "war on expertise."][link-5]
* Ivan Pepelnjak has some very useful and very applicable information on [VRF-aware DHCP relaying][link-6].
* Michał Iwańczuk shares some information on [EVPN inline mode in NSX-T][link-9].
* Harikrishnan T wraps up a 4-part series on [stateful active-active gateways in NSX][link-10]. Links to the previous three posts are embedded in the article.

## Servers/Hardware

* Manoj Kumar provides [a beginner's guide to Trusted Platform Module (TPM)][link-1].

## Security

* GitHub recently had to [change their RSA SSH host key][link-8]; be sure to check out the details.
* Google warns users about [exploitable flaws in popular Android phones][link-11].
* Rory McCune examines some [container security fundamentals][link-12] that arise from the fact that "containers are just processes."

## Cloud Computing/Cloud Management

* Laurent Leconte [takes AWS CDK for spin][link-3] by deploying a simple Lambda function written in Python.
* Marco Troisi explains [how being serverless sometimes means not using Lambda][link-17]. I'll admit that his article tagline ("Amazingly, Lambda can sometimes make you less Serverless") did catch my attention---as I'm sure it was intended to do! The article does a good job of explaining what's meant by the tagline, though. I will take one small exception: Marco says "Part of being Serverless means be able to choose the right service for the job at hand." Personally, I think that's part of being an IT professional (or should be!) and isn't restricted to Serverless.
* Josh Pollara recaps the experience of [migrating from AWS to Fly.io][link-18].
* Paweł Stadnicki provides an overview of [provisoning an Azure Static WebApp using .NET Interactive and the Pulumi Automation API][link-19].

## Operating Systems/Applications

* In [Technology Short Take 166][xref-1], I mentioned Nick Schmidt's article on D2. He has another pair of posts; in the first post he dives deeper into [using D2 for generating vSphere diagrams][link-2] and discusses some of the pros and cons of D2; in the second post he shows [using D2 for network diagrams][link-7]. (Unfortunately, I have yet to have any time to try using D2 myself. Sigh.)
* Howard Oakley has uncovered some changes to the macOS Gatekeeper/app notarization process; see more details in [this blog post][link-4].
* Thomas Heinen examines some approaches for dealing with [multi-architecture Docker image builds][link-13].
* This article provides a [comparison of Apache APISIX 3.0 and Kong API Gateway 3.0][link-14]. (Be aware that the authors of the article have an APISIX-based product.)

## Programming/Development

* Yeva Byzek with Confluent has a two-part series on "streaming applications"; that is, applications that leverage stream processing via Kafka. (No, this is not related to streaming video. Sorry.) Part 1 provides an overview of [a tutorial for developing streaming applications][link-15], while part 2 discusses [testing your streaming application][link-16].

## Storage

* Al Rasheed takes readers on a quick journey through [setting up Synology Cloud Sync][link-21].

## Virtualization

* William Lam takes a look at [ESXi on the latest Intel NUC][link-20].

That's all for now! As always, feel free to contact me if you have any questions, feedback, or suggestions for how I can improve this post or the site in general. [Find me on Mastodon][link-98], [find me on Twitter][link-99], or find me in one of several different Slack communities. Have a great weekend!

[link-1]: https://corpit.org/trusted-platform-module-tpm-beginners-guide/
[link-2]: https://blog.engyak.co/2023/03/d2-diagramming-vsphere
[link-3]: https://lau.rent/posts/python-lambda-with-dependencies.html
[link-4]: https://eclecticlight.co/2023/03/13/ventura-has-changed-app-quarantine-with-a-new-xattr/
[link-5]: https://subnetzero.info/2023/03/07/the-war-on-expertise/
[link-6]: https://blog.ipspace.net/2023/03/netlab-vrf-dhcp-relay.html
[link-7]: https://blog.engyak.co/2023/03/d2-diagramming-networks
[link-8]: https://github.blog/2023-03-23-we-updated-our-rsa-ssh-host-key/
[link-9]: https://www.safekom.pl/blog/en/vmware-en/nsx-en/nsx-t-evpn-inline-mode/
[link-10]: https://vxplanet.com/2023/04/04/nsx-4-0-1-stateful-active-active-gateway-part-4-edge-sub-clusters-and-failure-domains/
[link-11]: https://techcrunch.com/2023/03/16/google-warning-samsung-chips-flaws-android/
[link-12]: https://securitylabs.datadoghq.com/articles/container-security-fundamentals-part-1/
[link-13]: https://www.tecracer.com/blog/2023/03/docker-architecture-intel-arm-both.html
[link-14]: https://api7.ai/blog/apisix-vs-kong-3-0
[link-15]: https://www.confluent.io/blog/stream-processing-part-1-tutorial-developing-streaming-applications/
[link-16]: https://www.confluent.io/blog/stream-processing-part-2-testing-your-streaming-application/
[link-17]: https://www.theserverlessmindset.com/p/how-much-lambda-do-you-need
[link-18]: https://terrateam.io/blog/flying-away-from-aws
[link-19]: https://dev.to/cognipla/pulumi-and-net-interactive-i-5epm
[link-20]: https://williamlam.com/2023/03/esxi-on-intel-nuc-13-pro-arena-canyon.html
[link-21]: https://alarasheedblog.wordpress.com/2023/04/06/synology-cloud-sync/
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2023-03-10-technology-short-take-166.md" >}}
