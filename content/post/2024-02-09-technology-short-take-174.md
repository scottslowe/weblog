---
author: slowe
categories: Information
comments: true
date: 2024-02-09T11:30:00-06:00
tags:
- AWS
- Cilium
- Docker
- Git
- Hardware
- Kubernetes
- Linux
- Networking
- NSX
- Security
- Storage
- Virtualization
title: 'Technology Short Take 174'
url: /2024/02/09/technology-short-take-174/
---

Welcome to Technology Short Take #174! For your reading pleasure, I've collected links on topics ranging from Kubernetes Gateway API to recent AWS attack techniques to some geeky Linux and Git topics. There's something here for most everyone, I'd say! But enough of my rambling, let's get on to the good stuff. Enjoy!<!--more-->

## Networking

* I want to be Ivan Pepelnjak when I grow up. Why? Read [this article][link-10] on his response to someone wanting to use NSX to create availability zones.
* Nico Vibert has [a tutorial][link-19] that takes readers through using Cilium's Gateway API functionality to do L7 traffic management (HTTP redirects, HTTP rewrites, and HTTP mirroring).

## Security

* Nick Frichette [discusses][link-11] what he calls "one of the more egregious mistakes" that can be made in an AWS environment.
* Cybernews is covering what is being called [the "mother of all breaches."][link-12] The amount of data rumored to be included---26 _billion_ records---is almost impossible to comprehend.
* Martin McCloskey and Christophe Tafani-Dereeper of Datadog Security Labs share [some information on recent attack techniques][link-13] they've observed.
* Lee Holmes shines a light on the [security risks of Postman][link-15], although the risks really could apply to just about _any_ cloud-connected application. (And via this article I learned of [a fork of Kong's Insomnia API client][link-16] that is local-only and privacy-focused. Neat.)
* Snyk shatres some details on some [Docker and `runc` container breakout vulnerabilities][link-17].

## Cloud Computing/Cloud Management

* Darryl Ruggles walks readers through [building a serverless data processor][link-9]. The architecture incorporates AWS Lambda, AWS Step Functions, and Fargate on ECS (with some Rust thrown in there too).
* Eleni Grosdouli explains [how to use ArgoCD with Gateway API in Cilium][link-24]. (Note that the focus here is less about Cilium itself, and more about Gateway API and ArgoCD.)
* Here's [another look at installing Cilium][link-27], this time with a focus on AKS and using a Windows client.

## Operating Systems/Applications

* [Toolbx][link-1] now supports additional distributions; namely, Arch and Ubuntu. Read more details [here][link-2].
* While reading about Toolbx (as a result of the previous bullet), I found out about Distrobox ([project website][link-3], [GitHub repository][link-4]). I plan to try this project out myself, so don't be surprised if you see some Distrobox-related articles appearing on the site in the near future.
* Joshua Byrd shares some information on [running your own Mastodon instance forever, for free][link-5].
* Eduard Tolosa talks about [his move to Wayland][link-6].
* Luc van Donkersgoed talks about [retrieval-augmented generation (RAG) and LLMs][link-7].
* Here's an article on [using GitHub Actions to bulk close a bunch of issues][link-8].
* Hopefully you won't ever run into this particular issue, but if you do...here's some information on [recovering from a `pacman` crash on Arch Linux][link-14].
* Julia Evans helps with [fixing diverged Git branches][link-18].
* Robert Jensen [shines a light][link-20] on an issue running Cilium on KinD; the issue turns out to be a problem in Docker Desktop for Mac, and switching to Colima [fixes the issue][link-21]. Interesting---I am very curious to know the underlying details of why using Colima works.

## Programming/Development

* [This blog post][link-22] announces the release of Pkl (pronounced "pickle"), described as a new "programming language for producing configuration". Unless I'm reading this wrong, this sounds like quite an overlap with [CUE][link-26]. Or am I way wrong here?
* Milas Bowman [examines some new features in Docker Compose][link-25] specifically targeted at improving the development experience.

## Career/Soft Skills

* I think this article [speaks for itself][link-23].

That's all I have for you this time around, but check back in 2-3 weeks for the next Technology Short Take. Until then, feel free to share this article on your favorite social media platform, and I invite you to contact me if you have any feedback about this or any article on my site. You can find me [on the Fediverse][link-30], [on Twitter][link-99], or in a number of different Slack communities. Heck, if you try hard enough you can find my e-mail address on this site and drop me a message that way!

[link-1]: https://containertoolbx.org/
[link-2]: https://debarshiray.wordpress.com/2024/01/20/toolbx-now-offers-built-in-support-for-arch-linux-and-ubuntu/
[link-3]: https://distrobox.it/
[link-4]: https://github.com/89luca89/distrobox/
[link-5]: https://josh.is-cool.dev/running-a-mastodon-instance-entirely-free-forever/
[link-6]: https://www.edu4rdshl.dev/posts/my-move-to-wayland-it-s-finally-ready/
[link-7]: https://lucvandonkersgoed.com/2023/12/11/retrieval-augmented-generation-rag-simply-explained/
[link-8]: https://dev.to/github/how-i-bulk-closed-1000-github-issues-with-github-actions-d3b
[link-9]: https://darryl-ruggles.cloud/serverless-data-processor-using-aws-lambda-step-functions-and-fargate-on-ecs-with-rust
[link-10]: https://blog.ipspace.net/2024/01/vmware-nsx-availability-zones.html
[link-11]: https://hackingthe.cloud/aws/exploitation/Misconfigured_Resource-Based_Policies/misconfigured_iam_role_trust_policy_wildcard_principal/
[link-12]: https://cybernews.com/security/billions-passwords-credentials-leaked-mother-of-all-breaches/
[link-13]: https://securitylabs.datadoghq.com/articles/tales-from-the-cloud-trenches-ecs-crypto-mining/
[link-14]: https://underlap.org/recovering-from-a-pacman-crash-on-arch-linux
[link-15]: https://www.leeholmes.com/security-risks-of-postman/
[link-16]: https://github.com/ArchGPT/insomnium
[link-17]: https://snyk.io/blog/leaky-vessels-docker-runc-container-breakout-vulnerabilities/
[link-18]: https://jvns.ca/blog/2024/02/01/dealing-with-diverged-git-branches/
[link-19]: https://isovalent.com/blog/post/tutorial-redirect-rewrite-and-mirror-http-requests-with-cilium-gateway-api/
[link-20]: https://www.robert-jensen.dk/posts/2024-fixing-cilium-with-kind/
[link-21]: https://github.com/cilium/cilium/issues/30278
[link-22]: https://pkl-lang.org/blog/introducing-pkl.html
[link-23]: https://jmetz.com/2024/02/of-scheiss-and-men/
[link-24]: https://medium.com/@eleni.grosdouli/argocd-deployment-on-rke2-with-cilium-gateway-api-ab1769cc28a3
[link-25]: https://www.docker.com/blog/scaling-docker-compose-up/
[link-26]: https://cuelang.org/
[link-27]: https://observability-360.com/docs/ViewDocument?id=cilium-aks-getting-started
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
