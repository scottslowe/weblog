---
author: slowe
categories: Information
comments: true
date: 2022-01-21T08:15:00
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- AWS
- Apple
- Intel
- OSS
- Kubernetes
- Linux
- macOS
- NSX
- Git
title: 'Technology Short Take 151'
url: /2022/01/21/technology-short-take-151/
---

Welcome to Technology Short Take #151, the first Technology Short Take of 2022. I hope everyone had a great holiday season and that 2022 is off to a wonderful start! I have a few more links than normal this time around, although I didn't find articles in a couple categories. Don't worry---I'll keep my eyes peeled and my RSS reader ready to pull in new articles in those categories for next time. And now for the content!<!--more-->

## Networking

* Mike Fiedler [explores differences][link-6] in ways for container-to-container communication to occur.
* Aidan Steele examines how [VPC sharing could potentially improve security and reduce cost][link-13].
* What do _you_ think [microsegmentation means][link-15]?
* Nick Schmidt talks about [using GitOps with the NSX Advanced Load Balancer][link-28].

## Servers/Hardware

* The Next Platform takes [a look at Amazon's Graviton3 processor][link-3].
* Ru Singh provides [a review][link-4] of her M1-based MacBook Air.
* Here's [a great review of the Framework laptop][link-11].
* Losing to Apple---whose M-series chips are widely regarded as faster and more efficient than Intel's chips---has apparently stung the chip giant into revving up the innovation engine. [These details on their 12th-generation H processors][link-12] shows that Intel appears to be intent to regain the lead. Time will tell how successful they are.
* Chris Evans revisits the discussion regarding [Arm processor architectures in the public cloud][link-22].
* And, speaking of Arm processor architectures in the public cloud, [here's another look][link-23] at Amazon's Graviton2 and Graviton3 chips, as discussed in a pair of re:Invent talks.

## Security

* Kat Traxler [considers the impact of the Log4J vulnerability in cloud-based environments][link-7].
* I was very glad to see [this blog post][link-10] about the financial future of the GnuPG project.
* The story of a developer deliberately polluting their open source projects---as outlined [here][link-19] for the "colors.js" and "faker.js" libraries hosted on NPM---is a software supply chain issue that I suspect many organizations hadn't considered. Now they're going to have to start accounting for this possibility.
* [Cross-platform malware][link-30]. Ugh.
* Orca Security [discusses the "Superglue" vulnerability][link-32] in AWS Glue.

## Cloud Computing/Cloud Management

* Benoît Bouré explains [how to use short-lived credentials to access AWS resources from GitHub Actions][link-14].
* Ivan Velichko has a detailed article on [Kubernetes API resources, kinds, and objects][link-17].
* Sander Rodenhuis wrote an article on [security policies in Kubernetes][link-18]. The post focuses on Otomi, which in turn leverages Open Policy Agent and Gatekeeper.
* Michael Gasch pontificates on Knative's missteps in [this post from June 2021][link-20].
* Yann Léger and Alisdair Broshar [talk about the technology stack behind the Koyeb Serverless Engine][link-21].
* Lydia Leong has a great idea: why not [improve cloud resilience using things that actually work][link-25], instead of chasing [the multi-cloud failover unicorn][link-26]? There's some useful and practical advice here, in my opinion.
* Via the Kubernetes blog, Rory McCune of Aqua Security provides [some guidelines for securing admission controllers][link-31].

## Operating Systems/Applications

* I recently had a need to do a multicast DNS lookup, and [this article][link-1] was critical in figuring out how to use `dig` to do it.
* If you need a quick start for HashiCorp Vault, [this one][link-2] worked really well for me. I found it easier/better than the documentation on the HashiCorp web site, in fact.
* Dennis Felsing shares some thoughts on [switching to macOS after 15 years on Linux][link-5].
* Running Docker on an M1 Max-based system? [This article][link-8] may provide some useful information.
* [Here's some information][link-9] on why Microsoft Exchange stopped delivering e-mail messages on January 1, 2022.
* Mark Brookfield has started a series on automating SSL certificate issuance and renewal using HashiCorp Vault Agent, [here's part 1][link-16].
* Julia Evans has a quick post on [finding a domain's authoritative name servers][link-24]. I was already familiar with this process, but I appreciate that there are lots of folks out there who may not have had to ever do this.
* Here's [a handy list of secret phone codes you can use][link-27]. Some of these I already knew, but a few of them were new to me. Neat.
* The founder of the Nginx project recently stepped away from the project. Read more [here][link-29].
* [BIOS updates without a reboot][link-33], and under Linux first? Yes please!

That's all for now. I hope this was useful in some way! If you have any feedback for me---constructive criticism, praise, suggestions for where I can find more articles (especially if the site supports RSS!), feel free to reach out. I'd love to hear from you! You can reach [me on Twitter][link-99], or hit me up in any one of a number of different Slack communities.

[link-1]: https://www.gabriel.urdhr.fr/2019/04/02/llmnr-mdns-cli-lookup/
[link-2]: https://devopscube.com/setup-hashicorp-vault-beginners-guide/
[link-3]: https://www.nextplatform.com/2022/01/04/inside-amazons-graviton3-arm-server-processor/
[link-4]: https://rusingh.com/not-very-pro-apple-hello-air/
[link-5]: https://hookrace.net/blog/macos-setup/
[link-6]: https://www.miketheman.net/2021/12/28/container-to-container-communication/
[link-7]: https://www.vectra.ai/blogpost/log4j-unique-impact-in-the-cloud
[link-8]: https://www.proudcommerce.com/devops/docker-performance-on-apple-macbook-pro-with-m1-max-processor-status-and-tips
[link-9]: https://rachelbythebay.com/w/2022/01/01/baddate/
[link-10]: https://gnupg.org/blog/20220102-a-new-future-for-gnupg.html
[link-11]: https://luisartola.com/framework-laptop-with-ubuntu-review/
[link-12]: https://www.gsmarena.com/intel_announces_12thgen_h_processors_for_laptops_with_hybrid_design-news-52539.php
[link-13]: https://awsteele.com/blog/2022/01/02/shared-vpcs-are-underrated.html
[link-14]: https://benoitboure.com/securely-access-your-aws-resources-from-github-actions
[link-15]: https://blog.ipspace.net/2022/01/microsegmentation-terminology.html
[link-16]: https://virtualhobbit.com/2022/01/11/automate-ssl-certificate-issuing-and-renewal-with-hashicorp-vault-agent-part-1-configure-vault/
[link-17]: https://iximiuz.com/en/posts/kubernetes-api-structure-and-terminology/
[link-18]: https://hashnode.com/post/security-policies-in-kubernetes-made-easy-ckxz5fu2h02j2u8s13ecy82x0
[link-19]: https://www.bleepingcomputer.com/news/security/dev-corrupts-npm-libs-colors-and-faker-breaking-thousands-of-apps/
[link-20]: https://www.mgasch.com/2021/06/knative-pov/
[link-21]: https://www.koyeb.com/blog/the-koyeb-serverless-engine-from-kubernetes-to-nomad-firecracker-and-kuma
[link-22]: https://www.architecting.it/blog/arm-architecture-cloud/
[link-23]: https://muratbuffalo.blogspot.com/2021/12/graviton2-and-graviton3.html
[link-24]: https://jvns.ca/blog/2022/01/11/how-to-find-a-domain-s-authoritative-nameserver/
[link-25]: https://cloudpundit.com/2021/10/21/improving-cloud-resilience-through-stuff-that-works/
[link-26]: https://cloudpundit.com/2021/10/14/multicloud-failover-is-almost-always-a-terrible-idea/
[link-27]: https://www.tomsguide.com/how-to/how-to-use-secret-codes-on-iphone
[link-28]: https://blog.engyak.co/2022/01/gitops-with-nsx-advanced-load-balancer.html
[link-29]: https://www.nginx.com/blog/do-svidaniya-igor-thank-you-for-nginx/
[link-30]: https://www.intezer.com/blog/malware-analysis/new-backdoor-sysjoker/
[link-31]: https://kubernetes.io/blog/2022/01/19/secure-your-admission-controllers-and-webhooks/
[link-32]: https://orca.security/resources/blog/aws-glue-vulnerability/
[link-33]: https://linuxiac.com/intel-is-bringing-an-important-feature-to-linux-kernel-5-17/
[link-99]: https://twitter.com/scott_lowe
