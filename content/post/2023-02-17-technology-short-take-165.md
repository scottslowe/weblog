---
author: slowe
categories: Information
comments: true
date: 2023-02-17T11:20:00-05:00
tags:
- Automation
- AWS
- Certification
- Dagger
- Docker
- GitHub
- Hardware
- IaC
- Kubernetes
- Networking
- OSS
- Productivity
- Pulumi
- Security
- Storage
- Terraform
- Virtualization
title: 'Technology Short Take 165'
url: /2023/02/17/technology-short-take-165/
---

Welcome to Technology Short Take #165! Over the last few weeks, I've been collecting articles I wanted to share with readers on major areas in technology: networking, security, storage, virtualization, cloud computing, and OSes/applications. This particular Technology Short Take is a tad heavy on cloud computing, but there's a decent mix of other articles as well. Enjoy!<!--more-->

## Networking

* For a deeper understanding of Kubernetes networking, and in particular the role played by `kube-proxy`, I highly recommend [this post by Arthur Chiao][link-13]. There is a ton of information here!
* Denis Mulyalin shows how to use Nornir, Salt, and NetBox to [template your network tests][link-22]. Now if Denis' site just had a discoverable RSS feed...

## Security

* Aeva Black and Gil Yehuda tackle the [conundrum of open source security][link-2].
* If you haven't looked at Teri Radichel's [series of posts on automating cybersecurity metrics (ACM)][link-11], you should. There's quite a bit of good information there.
* [This post][link-23] on Cedar---a new policy language developed by AWS---is an interesting read. I'm curious as to the constraints that led AWS to develop a new policy language versus using something like Rego (part of Open Policy Agent); this isn't something the article touches upon.

## Cloud Computing/Cloud Management

* Dinesh Sharma has [an interesting post][link-1] showing to combine two IaC tools---CloudFormation StackSet and Pulumi---to help ensure tagging compliance.
* Jan Domanski has a four-part series on deploying a React app to AWS using Pulumi ([part 1][link-3], [part 2][link-4], [part 3][link-5], and [part 4][link-6]).
* Stephen O'Grady [asks an interesting question][link-8] about AWS: should they be building what customers want, or what developers want?
* This is an older post on [running Spot Instances as worker nodes in your Kubernetes cluster][link-9], but there's lots of good information here. Also see the link to the newer article the author wrote as a follow-up.
* The "Open Guide to Amazon Web Services" is probably a well-known resource, but in the event you haven't heard of it or seen it referenced, you can find it [here on GitHub][link-12].
* Lee Briggs covers [the plethora of ways to authenticate to AWS][link-14].
* I found these two posts on structuring your repositories for GitOps to be very helpful. The author, Kostis Kapelonis, first talks about [what not to do][link-15], then follows up with a post that has recommendations on [how to model multiple environments][link-16]. The posts are a bit older, but I think they are still quite relevant today for people and organizations using or adopting GitOps.
* Erik Näslund shares [his experience in migrating from Terraform to Pulumi][link-18].
* Dr. Soumaya Mauthoor shows [how to use Pulumi's configuration system to create feature flags][link-19] for Pulumi infrastructure-as-code programs.
* Here's [an "initial impressions"][link-21] post on TKG 2.1.
* Have some EC2 instances you inherited from a previous coworker? Here is [a guide to gaining acccess to inherited AWS EC2 instances][link-30].

## Operating Systems/Applications

* I don't do a whole lot in application development (yet), but the idea of [declarative database schemas][link-7] is a pretty cool idea. Too bad they implemented a proprietary DSL (domain-specific language) instead of allowing users to use a general-purpose programming language.
* It's a fairly straightforward process, but here are [some instructions for installing Docker and Docker Compose on Amazon Linux 2][link-17].
* I didn't really understand Dagger until I read [this][link-27]. I still don't necessarily fully understand Dagger, but at least I have some idea about how it can be used. (I'm sure it's no surprise that I am a fan of using full-featured programming languages to do things.)
* As fate (luck?) would have it, I found [this article][link-28] by Engin Diri on using `kube2pulumi` right around the same time I actually needed to use it to convert some Kubernetes YAML into a Pulumi TypeScript program.
* [This post][link-29] outlines some recent gVisor improvements in file system performance.

## Career/Soft Skills

* This is [a slightly older post about taking the CKA][link-10], but what I really liked was Brandon's use of the graphic at the top of the post that talks about failure being part of success. It's so true---we learn and grow through failure (or should, at least). As you go out and start tackling new things, don't let failure stop you; use the failure to prepare you for the next attempt.
* I _love_ this three-part (so far) series by Adrian Hornsby on skills that make a big difference ([part 1][link-24], [part 2][link-25], and [part 3][link-26]). Part 3 is a strong reminder of what I mentioned in the first bullet in this section: don't let failure stop you, use it to prepare for the next attempt. As Adrian quoted from Edison: "Every time I fail, I am getting closer to the truth."
* I didn't know [it was called this][link-20], but this technique is something I've used for a while now. Switching between active projects when your focus is flagging, especially when one of them is a learning project, can be useful, but like all things it needs to be managed carefully.

I have a ton more links I could share, but in the interest of keeping the length of this post manageable I'll wrap it up here. I hope that I managed to include something useful for you here; if not, please let me know! Are there technology areas you'd like to see more coverage of? Or are there areas you'd like to see less coverage? Those of you that have feedback on questions like this are invited to contact me! You can reach me [on Twitter][link-99], [on Mastodon][link-98], via e-mail (my address isn't terribly hard to find), or through Slack (I'm active in a number of different Slack communities). Thanks for reading!

[link-1]: https://www.linkedin.com/pulse/tagging-compliance-stack-using-pulumi-cloudformation-stackset-sharma/
[link-2]: https://aeva.online/blog/2023-oss-security-conundrum/
[link-3]: https://jandomanski.com/pulumi/aws/react/2020/07/29/deploy-create-react-app-aws-pulumi-post1.html
[link-4]: https://jandomanski.com/pulumi/aws/react/2020/08/29/deploy-create-react-app-aws-pulumi-post2.html
[link-5]: https://jandomanski.com/pulumi/aws/react/2020/10/29/deploy-create-react-app-aws-pulumi-post3.html
[link-6]: https://jandomanski.com/pulumi/aws/2020/11/07/deploy-create-react-app-aws-pulumi-post4.html
[link-7]: https://atlasgo.io/
[link-8]: https://redmonk.com/sogrady/2022/12/09/faster-horse/
[link-9]: https://itnext.io/the-definitive-guide-to-running-ec2-spot-instances-as-kubernetes-worker-nodes-68ef2095e767
[link-10]: https://brandonwillmott.com/2021/01/07/my-first-cka-exam-attempt/
[link-11]: https://medium.com/cloud-security/automating-cybersecurity-metrics-890dfabb6198
[link-12]: https://github.com/open-guides/og-aws
[link-13]: https://arthurchiao.art/blog/cracking-k8s-node-proxy/
[link-14]: https://leebriggs.co.uk/blog/2022/09/05/authenticating-to-aws-the-right-way
[link-15]: https://codefresh.io/blog/stop-using-branches-deploying-different-gitops-environments/
[link-16]: https://codefresh.io/blog/how-to-model-your-gitops-environments-and-promote-releases-between-them/
[link-17]: https://floatingcloud.io/how-to-install-docker-and-compose-on-amazon-linux-2/
[link-18]: https://blog.ekik.org/my-experience-migrating-my-infrastructure-from-terraform-to-pulumi
[link-19]: https://itnext.io/implementing-feature-flags-with-pulumi-df578fc9ea43
[link-20]: https://clivethompson.medium.com/how-to-practice-productive-procrastination-e2522247bd07
[link-21]: https://vrabbi.cloud/post/tkg-2-1-initial-impressions/
[link-22]: https://dmulyalin.github.io/2023-02-05-templating-network-tests/
[link-23]: https://onecloudplease.com/blog/cedar-a-new-policy-language
[link-24]: https://medium.com/the-cloud-architect/skills-that-make-a-big-difference-569bfb81c4bc
[link-25]: https://medium.com/the-cloud-architect/skills-that-make-a-big-difference-a68d316b5954
[link-26]: https://medium.com/the-cloud-architect/skills-that-make-a-big-difference-overcoming-the-fear-of-failure-96ec743171bb
[link-27]: https://docs.dagger.io/205271/replace-dockerfile/
[link-28]: https://blog.ediri.io/kubernetes-and-pulumi-converting-k8s-yaml-to-a-pulumi-supported-language
[link-29]: https://cloud.google.com/blog/products/containers-kubernetes/gvisor-file-system-improvements-for-gke-and-serverless
[link-30]: https://wiringbits.net/aws/2022/09/01/gaining-access-to-inherited-aws-ec2-instances.html
[link-98]: https://fosstodon.org/@scottslowe
[link-99]: https://twitter.com/scott_lowe
