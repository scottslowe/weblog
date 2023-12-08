---
author: slowe
categories: Information
comments: true
date: 2023-12-08T14:30:00-06:00
tags:
- Ansible
- AWS
- Azure
- GCP
- Hardware
- IaC
- Kubernetes
- Linux
- Networking
- Pulumi
- Security
- SSH
- Storage
- Terraform
- Virtualization
- VMware
- Web
- Windows
title: 'Technology Short Take 171'
url: /2023/12/08/technology-short-take-171/
---

Welcome to Technology Short Take #171! This is the next installation in my semi-regular series that shares links and articles from around the interwebs on various technology areas of interest. Let the linking begin!<!--more-->

## Networking

The networking section this time around is focused on application level protocols...but hey, they're still networking protocols, right?

* [This post][link-16] is the first of a three-part series on the core concepts of HTTP/3.
* Although the title of this article says "for Go programmers," I still found it to be [a well-written and understandable explanation of CORS][link-18] (and I'm no Go programmer---yet).

## Security

* [Undetectable cryptomining technique on Azure][link-8]? Yikes.
* [Stealing cryptographic keys from SSH][link-9]? Double yikes. (Fortunately, the fix for this is easy---don't use RSA keys.)
* [A UEFI flaw][link-17], affecting both Windows _and_ Linux, that leverages malicious images? Triple yikes.
* Nick Frichette shows [how to enumerate a principal ARN from an AWS unique identifier][link-13].

## Cloud Computing/Cloud Management

* Veronica Gnilitska [shared some thoughts][link-1] on why Masterpoint investigated Crossplane but ended up sticking it out with Terraform.
* Trevor Roberts Jr. takes readers through [how to allow GCP resources to assume AWS IAM roles][link-3]---all using Pulumi, of course!
* Bill Shetti, a friend and former colleague of mine, authored this piece on [best practices for instrumenting OpenTelemetry][link-4].
* Ami Mahloof leads readers through [using Component Resources to refactor their Pulumi code][link-5].
* Martin [goes through][link-7] setting up a basic AKS cluster and adding WebApplication Routing to the cluster. His choice of tools? Pulumi with C#.
* [This article][link-10] from Amrutha Chennepalli hits all my favorite haunts: Kubernetes, Kuma Mesh, and Pulumi. Nice! (Although one thing did throw me off: Kuma needs PostgreSQL now??)
* AxeMind lays out what they believe to be [the ideal blueprint][link-12] for a serverless project deployed with AWS CDK.
* Þórarinn Sigurðsson [talks about Garden's plugin for Pulumi][link-14].
* [This article][link-15] is a comprehensive---and yet approachable---deep dive on using Dagger for CI/CD with an IaC repository.

## Operating Systems/Applications

* Decisions, decisions---[which immutable Linux to use][link-6]?

## Programming/Development

* Edward Loveall lays out a powerful argument to [make sure GitHub doesn't become the only option for version control][link-11]. (The concerns expressed in this article are why, even before I'd read this article, I've started ensuring that my repositories exist on multiple upstream platforms.)

## Virtualization

* Allen from Humbled Geeks shows [how to create a VMware vSwitch using Ansible][link-2].

That's all for this time! I know it's a bit shorter than the typical Technology Short Take, but hopefully you'll find something useful here nevertheless. If you have any feedback for me---ways to improve this series, additional technology areas I should consider including, good sources of articles and links, etc.---feel free to reach out anytime. I'm active in a number of Slack communities, and I can also be found [on the Fediverse][link-30] and [on Twitter][link-99]. Thanks for reading!

[link-1]: https://masterpoint.io/updates/passing-on-crossplane/
[link-2]: https://humbledgeeks.com/how-to-create-a-vswitch-in-vmware-ansible/
[link-3]: https://www.trevorrobertsjr.com/blog/aws-iam-gcp-sa/
[link-4]: https://www.elastic.co/blog/best-practices-instrumenting-opentelemetry
[link-5]: https://medium.com/@amimahloof/refactoring-pulumi-code-ccf7ac6c2531
[link-6]: https://baez.link/what-immutable-linux-to-use
[link-7]: https://martinjt.me/2023/09/11/creating-an-aks-cluster-with-webapplication-routing-using-pulumi/
[link-8]: https://thehackernews.com/2023/11/researchers-uncover-undetectable-crypto.html
[link-9]: https://arstechnica.com/security/2023/11/hackers-can-steal-ssh-cryptographic-keys-in-new-cutting-edge-attack/
[link-10]: https://blog.zelarsoft.com/kuma-mesh-multi-zone-deployment-on-kubernetes-with-pulumi-fbae7db5e206
[link-11]: https://blog.edwardloveall.com/lets-make-sure-github-doesnt-become-the-only-option
[link-12]: https://awstip.com/structuring-your-serverless-project-with-aws-cdk-a-comprehensive-guide-98b1f31cb3ed
[link-13]: https://hackingthe.cloud/aws/enumeration/enumerate_principal_arn_from_unique_id/
[link-14]: https://garden.io/blog/pulumi-plugin
[link-15]: https://blog.matiaspan.dev/posts/exploring-dagger-building-a-ci-cd-pipeline-for-iac/
[link-16]: https://www.smashingmagazine.com/2021/08/http3-core-concepts-part1/
[link-17]: https://arstechnica.com/security/2023/12/just-about-every-windows-and-linux-device-vulnerable-to-new-logofail-firmware-attack/
[link-18]: https://eli.thegreenplace.net/2023/introduction-to-cors-for-go-programmers/
[link-30]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
