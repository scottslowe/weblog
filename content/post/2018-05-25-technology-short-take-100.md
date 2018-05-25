---
author: slowe
categories: Information
comments: true
date: 2018-05-25T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- AWS
- Ansible
- Terraform
- Linux
- Kubernetes
- Docker
- Azure
- vSphere
title: 'Technology Short Take 100'
url: /2018/05/25/technology-short-take-100/
---

Wow! This marks 100 posts in the Technology Short Take series! For almost eight years (Technology Short Take #1 [was published][xref-1] in August 2010), I've been collecting and sharing links and articles from around the web related to major data center technologies. Time really flies when you're having fun! Anyway, here is Technology Short Take 100...I hope you enjoy!<!--more-->

Also, a quick note that I removed the "Servers/Hardware" and "Storage" sections this time around, as I didn't have any useful content to share. I'll continue to evaluate whether I will/should include those sections moving forward (your feedback is welcome; [hit me up on Twitter][link-22]).

## Networking

* Will Robinson walks readers through [installing NTC Ansible][link-5].
* [Battleship over BGP?][link-10] Interesting (saw this via Rob Markovic on Twitter).
* Ben Piper shares an example of [automating Cumulus Linux with Ansible][link-19].

## Security

* Container wizard Jessie Frazelle shares a proposal for [hard multi-tenancy in Kubernetes][link-15]. Along the way, she also provides some additional (useful) information about existing isolation mechanisms.

## Cloud Computing/Cloud Management

* This article on [how proportional CPU allocation works with AWS Lambda][link-1] shows that there's more to architecting and supporting Lambda for maximum efficiency than a lot of folks realize. (I could be wrong, though. It could be that lots of people recognize that Lambda isn't as simple as it seems.)
* While we are on the topic of AWS Lambda, check out this post by Mayank Lahiri on [understanding and controlling AWS Lambda costs][link-2]. The fact that pricing is based on a composite unit combining both execution time and memory allocation means that billing isn't necessarily as straightforward as it may seem, and this article has some good details. (Note that it is a bit dated, having been written in September 2017.)
* Lest you think pricing for serverless architectures is as simple as "how many AWS Lambda invocations I had last month," check out Amiram Shachar's post on [the hidden costs of serverless][link-7].
* Yan Cui of A Cloud Guru discusses [how language, memory, and package size affect cold starts of AWS Lambda][link-6]. Key takeaways from Cui's post are that C# and Java have (and I quote) "~100 times the cold start time of Python and also suffer from much higher standard deviation". Perhaps you should re-think using Java as your language of choice for Lambda functions!
* Let's continue the AWS Lambda love-fest with a mention of this post by Roman Dodin on [building AWS Lambda with Python, S3, and serverless][link-3]. The nice thing about this post, in my opinion, is that it peels back to show some of what's happening under the hood.
* Further proof that using AWS Lambda involves more than "just functions" is found in this post by Payam Moghaddam on [using Terraform to deploy AWS Lambda][link-4]. What is Terraform deploying, you ask? I could tell you, but I'd rather you go read the article for yourself.
* Gal Bashan has a two-part series on AWS Lambda internals ([part 1][link-8] and [part 2][link-9]). Bashan gets a bit geeky, but it's still interesting (and potentially useful) information in my opinion.
* This post by Caleb Lloyd on [recovering a broken Kubernetes cluster][link-11] has some helpful information.

## Operating Systems/Applications

* Michael Gasch [goes poking around in the Docker "scratch" container][link-13].
* This is interesting---Microsoft has [added support for Terraform providers to Azure Resource Manager (ARM)][link-14]. This allows ARM to orchestrate the creation of resources _through_ Terraform. Only three Terraform providers are supported right now (Kubernetes, CloudFlare, and Datadog).
* Anyone played with (or is using) [Jenkins X][link-16] yet?
* Michael Hausenblas has [a nice `kubectl` in action post][link-18] that provides a handy reference.
* The Kubernetes-Containerd integration has been made GA, according to [this blog post][link-21]. This is on my list of things to try, when I get time.

## Virtualization

* William Lam explores the use of Instant Clone in vSphere 6.7 for [very fast nested ESXi provisioning][link-12].
* Somewhat tongue-in-cheek, Bob Plankers [addresses the complaints][link-20] of the folks using old lab hardware to run the latest generation of vSphere.

## Career/Soft Skills

* At first I thought [this article on SRE vs. DevOps][link-17] was link-bait (click-bait?), but upon reading the article I found it to be quite useful and informative. If you're unclear about the differences between SRE and DevOps, or are looking for more information on SRE, I'd recommend reading this post. (I haven't taken a look at the embedded videos yet, so I can't speak to their usefulness.)

That's all I have for you this time around (I know, it's a bit shorter than usual). I'll have the next Technology Short Take published in about two weeks, so you have until then to read all the articles I've included above. Good luck!

[link-1]: https://engineering.opsgenie.com/how-does-proportional-cpu-allocation-work-with-aws-lambda-41cd44da3cac
[link-2]: https://serverless.com/blog/understanding-and-controlling-aws-lambda-costs/
[link-3]: https://netdevops.me/2017/building-aws-lambda-with-python-s3-and-serverless/
[link-4]: https://medium.com/build-acl/aws-lambda-deployment-with-terraform-24d36cc86533
[link-5]: http://www.oznetnerd.com/installing-ntc-ansible/
[link-6]: https://read.acloud.guru/does-coding-language-memory-or-package-size-affect-cold-starts-of-aws-lambda-a15e26d12c76
[link-7]: https://medium.com/@amiram_26122/the-hidden-costs-of-serverless-6ced7844780b
[link-8]: https://medium.com/epsagon/lambda-internals-exploring-aws-lambda-462f05f74076
[link-9]: https://medium.com/epsagon/aws-lambda-internals-part-2-going-deeper-1e12b9d2515f
[link-10]: https://blog.benjojo.co.uk/post/bgp-battleships
[link-11]: https://codefresh.io/Kubernetes-Tutorial/recover-broken-kubernetes-cluster/
[link-12]: https://www.virtuallyghetto.com/2018/05/leveraging-instant-clone-in-vsphere-6-7-for-extremely-fast-nested-esxi-provisioning.html
[link-13]: https://embano1.github.io/post/scratch/
[link-14]: https://azure.microsoft.com/en-us/blog/introducing-the-azure-terraform-resource-provider/
[link-15]: https://blog.jessfraz.com/post/hard-multi-tenancy-in-kubernetes/
[link-16]: https://jenkins-x.io/
[link-17]: https://cloudplatform.googleblog.com/2018/05/SRE-vs-DevOps-competing-standards-or-close-friends.html
[link-18]: http://mhausenblas.info/kubectl-in-action/
[link-19]: https://cumulusnetworks.com/blog/automating-cumulus-linux-ansible/
[link-20]: https://lonesysadmin.net/2018/05/24/vsphere-67-lab-parable/
[link-21]: https://kubernetes.io/blog/2018/05/24/kubernetes-containerd-integration-goes-ga/
[link-22]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2010-08-18-technology-short-take-1.md" >}}
