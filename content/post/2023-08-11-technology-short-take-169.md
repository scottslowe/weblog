---
author: slowe
categories: Information
comments: true
date: 2023-08-11T15:30:00-06:00
tags:
- AWS
- Docker
- Encryption
- GitHub
- Hardware
- Intel
- Kubernetes
- Networking
- Pulumi
- RedHat
- Security
- SSH
- Storage
- Terraform
- Virtualization
title: 'Technology Short Take 169'
url: /2023/08/11/technology-short-take-169/
---

Welcome to Technology Short Take #169! Prior to [the recent Spousetivities post][xref-1], it had been a few months since I posted on the site; life has been busy, and it hasn't left much time for blogging. Hopefully things will settle down soon, but until then I'll continue to do the best I can to share useful information with folks. Hopefully something I've included in this Technology Short Take proves to be useful to someone. OK, let's get on to the content!<!--more-->

## Networking

* Navendu Pottekkat offers readers [a comprehensive guide to API gateways, Kubernetes gateways, and service meshes][link-3]. It's a good breakdown of the technologies, how they're related, how they're different, and the intended/ideal use cases for each.

## Security

* [Backdoors in SSH public keys][link-7]? Oh my. Be wary, friends.
* Alessandro Brucato [updates on SCARLETEEL 2.0 enhancements and changes][link-8].
* Daniel Gzrelak walks readers through what appears to be a very large (potential) hole when [configuring GitHub OIDC integration with AWS][link-12].
* [Here we go again][link-16]. As a side note, I am curious to know what other CPU architectures, if any, are affected. Will something like this spark a (larger) migration to ARM-based architectures?

## Cloud Computing/Cloud Management

* Has your team started saying, "It works in my container"? If they have, then it may be worth your time to review [this set of tips][link-1] by Raju Dawadi.
* If you need a foundational review of Amazon ECS, [this article][link-2] is a good starting point. If I could somehow find a way to make more hours in the day, I'd write a Pulumi program that automates all these steps to accompany Akshar's step-by-step write-up.
* Amrut Prabhu shows [using LocalStack via Docker Compose][link-5] to be able to mock AWS services locally.
* Nathan Peck ponders what it would look like to [rethink infrastructure as code from scratch][link-9]. In the end, he comes up with using a general purpose programming language (via CDK in his article) to write higher-level constructs. If only there was [an infrastructure as code product][link-10] that leveraged general purpose programming languages natively...
* Jubril Oyetunji takes readers through [deploying a database cluster on DigitalOcean using Pulumi][link-11].
* Yan Cui [shows][link-14] how to share code between functions in a monorepo when using AWS Lambda.
* Here's one individual's [initial thoughts on the HashiCorp license change][link-17]. (No doubt there are plenty of other reactions!)
* Here's [how to use a PGP public key with Pulumi][link-19] so that IAM user secret keys are encrypted.

## Operating Systems/Applications

* There's been a ton of coverage about the Red Hat changes around RHEL sources and such, so I won't bother going into any detail on that. [This article][link-13] describes the choice that AlmaLinux made on how to move forward, and I for one do hope that the idea of AlmaLinux becoming a "standard" community enterprise Linux turns into more than just an idea.

## Programming/Development

* Emily Nakashima [tries to answer][link-6] the question, "What _is_ tech debt?"
* And since we were just talking about tech debt: Rob Hirschfeld [worries][link-15] that large language models (LLMs) are leading folks into a "tech debt trap."
* John D. Cook discusses [productive constraints][link-20] (why less choice is sometimes good).

## Virtualization

* A few months ago the gVisor project [released a new high-performance gVisor platform][link-4] named Systrap. This platform is slated to become the default platform starting next month (September 2023).
* [Using Firecracker microVMs for multi-tenant Dagger pipelines][link-18] sounds like a cool, nerdy kind of thing to do.

That's all for now! As always, I hope that you find something useful in this post; if you do, take a moment to hit me up and let me know! Or, if you didn't find something useful, take a moment to hit me up and let me know that, too! I'm always open to reader feedback, and would love to hear from you. You can find me [on Twitter][link-21] (I refuse to call it that other name), [on Mastodon][link-22], or [on Bluesky][link-23]. And I hang out in a number of different Slack communities. It's not hard to find me!

[link-1]: https://dwdraju.medium.com/how-it-works-in-my-machine-turns-it-works-in-my-container-1b9a340ca43d
[link-2]: https://medium.com/@raaj.akshar/a-step-by-step-ecs-tutorial-838d60fde8d
[link-3]: https://navendu.me/posts/gateway-and-mesh/
[link-4]: https://gvisor.dev/blog/2023/04/28/systrap-release/
[link-5]: https://refactorfirst.com/localstack-with-docker-compose
[link-6]: https://www.honeycomb.io/anything-but-tech-debt
[link-7]: https://blog.thc.org/infecting-ssh-public-keys-with-backdoors
[link-8]: https://sysdig.com/blog/scarleteel-2-0/
[link-9]: https://nathanpeck.com/rethinking-infrastructure-as-code-from-scratch/
[link-10]: https://www.pulumi.com/
[link-11]: https://everythingdevops.dev/deploying-a-database-cluster-on-digitalocean-using-pulumi/
[link-12]: https://dagrz.com/writing/aws-security/hacking-github-aws-oidc/
[link-13]: https://dissociatedpress.net/2023/07/15/almalinux-makes-its-choice-the-friendly-fork/
[link-14]: https://theburningmonk.com/2019/06/aws-lambda-how-to-share-code-between-functions-in-a-monorepo/
[link-15]: https://devops.com/are-llms-leading-devops-into-a-tech-debt-trap/
[link-16]: https://downfall.page/
[link-17]: https://yehudacohen.substack.com/p/initial-thoughts-about-hashicorp
[link-18]: https://www.felipecruz.es/exploring-firecracker-microvms-for-multi-tenant-dagger-ci-cd-pipelines/
[link-19]: https://kimvanwyk.co.za/accessing-protected-pulumi-outputs-with-a-pgp-key/
[link-20]: https://www.johndcook.com/blog/2023/08/08/productive-constraints/
[link-21]: https://twitter.com/scott_lowe
[link-22]: https://fosstodon.org/@scottslowe
[link-23]: https://bsky.app/profile/scottslowe.bsky.social
[xref-1]: {{< relref "2023-08-07-spousetivities-returns-to-vmware-explore.md" >}}
