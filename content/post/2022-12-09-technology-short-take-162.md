---
author: slowe
categories: Information
comments: true
date: 2022-12-09T06:30:00-06:00
tags:
- AWS
- Azure
- Cilium
- ESXi
- Git
- GitHub
- Hardware
- Kubernetes
- macOS
- NAT
- Networking
- NSX
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
- VMware
- YAML
title: 'Technology Short Take 162'
url: /2022/12/09/technology-short-take-162/
---

Welcome to Technology Short Take #162! It's taken me a bit longer than I would have liked to get this post assembled, but it's finally here. Hopefully I've managed to find something you'll find useful! As usual, the links below are organized by technology area/discipline, and I've added a little bit of commentary to some of the links where it felt necessary. Enjoy!<!--more-->

## Networking

* With [this announcement][link-16] of Cilium support within AKS, Cilium is now available in managed Kubernetes offerings across all three major cloud providers.
* Leave it to Ivan [to school everyone on Ethernet encapsulations][link-21].
* [This article][link-23] provides a few practical examples of how using VPC Endpoints to reduce network traffic moving through NAT Gateways can help save money.

## Security

* Rory McCune has a series of articles on PCI compliance in containerized and Kubernetes environments. These are worth a read if security and compliance are your jam (see [here][link-8], [here][link-9], [here][link-10], [here][link-11], [here][link-12], [here][link-13], and [here][link-14]). I suspect more are in the works, so stay tuned to his site!
* Persistent malware in ESXi hypervisor environments? Ugh! See [here][link-15] for more details.
* The corny (cheesy?) food references in the title of [this article][link-17] are almost too much. Hey, at least they're having fun with it.
* Chris Farris shares [some tips for securing GitHub organizations][link-26]. The article is a tad focused on Steampipe, but there are general takeaways that I think are useful.
* [This article][link-29] is an interesting look at Internet scanning.

## Cloud Computing/Cloud Management

* [This][link-1] was a neat article that came out of one of Pulumi's recent "Pulumi Challenges."
* Dave Hall has an article about [tracking infrastructure using Terraform and AWS SSM Parameter Store][link-6].
* Jim Counts' [beginner's guide to Pulumi CI/CD pipelines][link-7] provides an overview of Pulumi and a guide on using it with Azure DevOps. (Note: this article is a couple years old, so keep that in mind---some things may have changed with both Pulumi and Azure DevOps since this article was published.)
* Engin Diri's article on [continuous cluster audit scanning with Trivy][link-19] is a "two-for-one" article: you get to see some Pulumi YAML to create a Kubernetes cluster on Civo, _and_ you get to see writing policies for the Trivy Operator. Nice.
* Ricardo Sueiras captured some great links on open source at AWS in [this newsletter][link-20].
* I shared this via Twitter, but wanted to include it here because I think it's a really cool use case. Muhammad Bhatti [shares an example][link-27] of using Pulumi code in an AWS Lambda to create a mechanism for running containers on-demand.
* Apparently due to the way the integration between Antrea and VMware NSX was designed, it's possible for "stale" Antrea-enabled clusters (clusters that once existed but are no longer present/valid) to show up in the NSX UI. Bassem Rezkalla shows [how to remove these stale clusters][link-28].

## Operating Systems/Applications

* Curious about what a JWT is? [This article][link-4] from Teleport may be helpful.
* Jeff Johnson [points out][link-18] an obvious but I suspect often-overlooked aspect of macOS' Full Disk Access.
* Even if you use an online service such as GitHub, GitLab, or Codeberg, you still need to ensure you have backups of your repositories. [This article][link-24] provides one potential solution.
* GitOps is all the rage these days (and there are valid reasons why), but I liked this article by Jim Sheldon because it discusses something more mundane yet critically important: how to [structure the code in your Git repositories for GitOps][link-25]. Sans the short Harness commercial at the end, I found this article to be useful.

## Storage

* It would seem that macOS 13 "Ventura" has introduced some problems with USB storage; these problems crop up [when working with Raspberry Pi systems][link-5].

## Virtualization

* But is it _really_ a [virtual machine][link-32]?

## Programming

* Engin Diri has two relatively recent posts on Rust, which he's been spending some time learning. The first is [how to async/await in Rust][link-30] (tackling the issue of asynchronous programming); the second is [creating a gRPC-based microservice in Rust][link-31]. If you're learning Rust (or interested in learning Rust), I think these articles will be helpful to you.

## Career/Soft Skills

* I really enjoyed this post on [learning from the past but not living there][link-2]. I think of this from a career perspective: we need to learn from our past (mistakes, jobs, opportunities, technologies), but our industry is one of change---we can't _stay_ in the past because we'll be left behind.
* Matt Stratton's presentation on [the journey from DevOps to cloud engineering][link-3] was one I really enjoyed (remotely/virtually, since it was presented at an event in London).
* I agree with Marc---[write more][link-22].

This will likely be the very last Technology Short Take of 2022, but I'll be back in 2023 with more Technology Short Takes, so make sure you stay tuned! In the meantime, feel free to connect with me [on Twitter][link-99] or [on Mastodon][link-99b], or connect with me in any one of the various Slack communities where I'm active (the Kubernetes and Pulumi Slack communities are a pretty sure bet). Thanks for reading!

[link-1]: https://www.karnwong.me/posts/2022/11/deploy-more-efficiently-with-templating/
[link-2]: https://dallincrump.com/learn-from-the-past-but-dont-live-there
[link-3]: https://speaking.mattstratton.com/DeAdOp
[link-4]: https://goteleport.com/blog/what-are-jwts/
[link-5]: https://www.raspberrypi.com/news/the-ventura-problem/
[link-6]: https://www.davehall.com.au/blog/2022/10/19/tracking-infrastructure-with-ssm-and-terraform/
[link-7]: https://build5nines.com/beginners-guide-to-pulumi-ci-cd-pipelines/
[link-8]: https://raesene.github.io/blog/2022/09/10/PCI-Guidance-for-containers-and-container-orchestration-tools/
[link-9]: https://raesene.github.io/blog/2022/09/20/Assessing-Kubernetes-Clusters-for-PCI-Compliance/
[link-10]: https://raesene.github.io/blog/2022/10/01/PCI-Kubernetes-Section1-Authentication/
[link-11]: https://raesene.github.io/blog/2022/10/08/PCI-Kubernetes-Section2-Authorization/
[link-12]: https://raesene.github.io/blog/2022/10/15/PCI-Kubernetes-Section3-workload-security/
[link-13]: https://raesene.github.io/blog/2022/10/23/PCI-Kubernetes-Section4-network-security/
[link-14]: https://raesene.github.io/blog/2022/10/29/PCI-Kubernetes-Section5-PKI/
[link-15]: https://www.mandiant.com/resources/blog/esxi-hypervisors-malware-persistence
[link-16]: https://isovalent.com/blog/post/azure-cni-cilium/
[link-17]: https://security.googleblog.com/2022/10/announcing-guac-great-pairing-with-slsa.html
[link-18]: https://lapcatsoftware.com/articles/FullDiskAccess.html
[link-19]: https://blog.ediri.io/continuous-cluster-audit-scanning-with-the-trivy-operator-using-custom-policies
[link-20]: https://blog.beachgeek.co.uk/newsletter/aws-open-source-news-and-updates-133/
[link-21]: https://blog.ipspace.net/2022/10/ethernet-encapsulations.html
[link-22]: https://brooker.co.za/blog/2022/11/08/writing.html
[link-23]: https://www.vantage.sh/blog/nat-gateway-vpc-endpoint-savings
[link-24]: https://www.dantuck.com/article/git-backup/
[link-25]: https://harness.io/technical-blog/gitops-repo-structure
[link-26]: https://steampipe.io/blog/github-security-tips
[link-27]: https://justedagain.com/posts/2022/pulumi-on-aws-lambda/
[link-28]: https://nsxbaas.blog/2022/12/07/removing-stale-antrea-tanzu-kubernetes-clusters-from-nsx-ui/
[link-29]: https://www.greynoise.io/blog/new-sensor-benign-activity
[link-30]: https://blog.ediri.io/how-to-asyncawait-in-rust-an-introduction
[link-31]: https://blog.ediri.io/creating-a-microservice-in-rust-using-grpc
[link-32]: https://www.engraved.blog/building-a-virtual-machine-inside/
[link-99]: https://twitter.com/scott_lowe
[link-99b]: https://fosstodon.org/@scottslowe
