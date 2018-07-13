---
author: slowe
categories: Information
comments: true
date: 2018-07-13T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Automation
- Linux
- Docker
- OSS
- Career
- Ansible
- Kubernetes
- vSphere
- AWS
- CLI
title: 'Technology Short Take 102'
url: /2018/07/13/technology-short-take-102/
---

Welcome to Technology Short Take 102! I normally try to get these things published biweekly (every other Friday), but this one has taken quite a bit longer to get published. It's no one's fault but my own! In any event, I hope that you're able to find something useful among the links below.<!--more-->

## Networking

* Matt Oswalt, now at Juniper, has been recently trumpeting the idea of reliability as a core reason for automation. Here's [his latest piece][link-3]. (For the record, I don't disagree with much of what Matt has to say on this topic.)
* Ajay Chenampara has a post on [using the Ansible `network-engine` command parser to parse the output of commands on network devices][link-11]. It looks like there will be a follow-up to this article as well, so you may want to check back on Ajay's site.
* Bernd Malmqvist [talks][link-17] about Avi Networks' software-defined load balancing solution, including providing an overview of how to use Vagrant to test it yourself.
* Julia Evans provides [a quick overview of Wireshark][link-19].

## Security

* The sort of things that are possible with Falco---some of which are outlined in [this admittedly very vendor-specific article][link-2]---makes it an intriguing solution (to me, anyway).
* Chris Hein shows [how to use the Heptio Authenticator][link-14] with `kops` to link Kubernetes cluster authentication to AWS IAM.

## Cloud Computing/Cloud Management

* Joe Duffy [takes the wraps off Pulumi][link-7], which looks _very_ interesting. This is an area where I'd love to spend some time really digging in.
* Interesting article [here][link-10] on Chick-Fil-A's use of Kubernetes in their restaurants, and the tools/process they follow for establishing those clusters.
* Jeff Geerling---well-known in the Ansible and Drupal communities---discusses [the (perceived) complexity of Kubernetes][link-12]. He spent some time _actually_ working with Kubernetes to solve a problem, and he came away from that time recognizing (in his words) "most of the complexity is necessary," and seeing value in using Kubernetes for appropriate use cases. This article is not (in my opinion) a knee-jerk reaction to some of the debate around Kubernetes' complexity, but rather a measured and honest evaluation of a tool to solve problems.
* Maish Saidel-Keesing has a two-part (so far?) series on comparing CloudFormation, Terraform, and Ansible ([part 1][link-24], [part 2][link-25]). This is worth a read if you're trying to determine which of these tools may be right for your use case.
* Here's a three-part series on Helm ([part 1][link-26], [part 2][link-27], and [part 3][link-28]).

## Operating Systems/Applications

* This is probably well-known, but I found [this little tidbit][link-1] about read-only containers handy.
* This article on [another reason your Docker containers may be slow][link-4] was an excellent reminder that containerization is not equal to virtualization (that doesn't make it better or worse, just different), and therefore can't be treated the same. Different design and architecture considerations apply in each instance.
* Lightroom is one of only a few applications that I keep around for macOS; [this article][link-8] gives me some alternatives.
* Here's an article on [getting started with Buildah][link-16], part of a suite of tools being built as alternatives to Docker.
* Tom Sweeney also [talks][link-15] about Buildah and how it can be used to create small container images. My takeaway from this article is that building really small container images requires either a) arcane knowledge of Docker layers and the `Dockerfile`, or b) arcane knowledge of little-used `yum` parameters.
* Shahidh Muhammad has [a review/comparison of various tools][link-18] aimed at helping developers build and deploy their apps on Kubernetes.
* William Lam shares [his list][link-23] of Visual Studio Code extensions.
* Finally, a reasonably usable [CLI client for Slack][link-29]---written in Bash, no less!

## Storage

* Cormac Hogan walks through [setting up the Minio object store on top of VMware vSAN][link-13].

## Virtualization

* Mike Foley walks you through [configuring TPM 2.0 on a vSphere 6.7 host][link-8]. (This must be the "future TPM content" that Mike mentioned.)
* Ed Haletky [documents][link-21] the approach he uses to produce segregated virtual-in-virtual test environments that live within his production environment.
* It seems as if there's quite a bit of complexity in [this article][link-22] on using KubeVirt witih GlusterFS, but I can't tell if that is a byproduct of KubeVirt, GlusterFS, or the combination of the two.

## Career/Soft Skills

A couple of useful career-related articles popped up on my radar over the last couple of weeks:

* First, there's [this article][link-5] by Eric Lee on IT burnout. If you find yourself experiencing some of the same issues that Eric describes, I'd encourage you to take a deeper look and see if changes are necessary.
* Next, Cody De Arkland [tackles the topic of imposter syndrome][link-6]. This is an area where I personally have struggled in the past, and I had to learn to humbly accept praise and stop negative self-talk. Talk to someone if you're wrestling with this.

That's all for now. Look for the next Technology Short Take in 2-3 weeks, and feel free [to contact me via Twitter][link-20] if you have any links or articles you think I should share. Thanks!

[link-1]: https://nickjanetakis.com/blog/docker-tip-55-creating-read-only-containers
[link-2]: https://sysdig.com/blog/docker-runtime-security/
[link-3]: https://forums.juniper.net/t5/Enterprise-Cloud-and/The-Network-Reliability-Engineer-s-Manifesto/ba-p/329020
[link-4]: https://hackernoon.com/another-reason-why-your-docker-containers-may-be-slow-d37207dec27f
[link-5]: https://veric.me/2018/06/06/my-trudge-through-it-burnout-and-the-fight-to-keep-it-at-bay/
[link-6]: https://www.thehumblelab.com/lets-talk-about-imposter-syndrome/
[link-7]: http://joeduffyblog.com/2018/06/18/hello-pulumi/
[link-8]: https://opensource.com/alternatives/adobe-lightroom
[link-9]: https://blogs.vmware.com/vsphere/2018/06/configuring-tpm-2-0-on-a-6-7-esxi-host.html
[link-10]: https://medium.com/@cfatechblog/bare-metal-k8s-clustering-at-chick-fil-a-scale-7b0607bd3541
[link-11]: https://termlen0.github.io/2018/06/26/observations/
[link-12]: https://www.jeffgeerling.com/blog/2018/kubernetes-complexity
[link-13]: https://cormachogan.com/2018/06/22/minio-s3-object-store-deployed-as-a-set-of-vms-on-vsan/
[link-14]: https://aws.amazon.com/blogs/opensource/deploying-heptio-authenticator-kops/
[link-15]: https://opensource.com/article/18/5/containers-buildah
[link-16]: https://opensource.com/article/18/6/getting-started-buildah
[link-17]: https://techbloc.net/archives/3066
[link-18]: https://blog.hasura.io/draft-vs-gitkube-vs-helm-vs-ksonnet-vs-metaparticle-vs-skaffold-f5aa9561f948
[link-19]: https://jvns.ca/blog/2018/06/19/what-i-use-wireshark-for/
[link-20]: https://twitter.com/scott_lowe
[link-21]: https://www.astroarch.com/2018/07/vsphere-upgrade-saga-building-virtual-in-virtual-labs/
[link-22]: http://kubevirt.io/2018/Deploying-VMs-on-Kubernetes-GlusterFS-KubeVirt.html
[link-23]: https://www.virtuallyghetto.com/2018/07/my-list-of-microsoft-visual-code-studio-extensions.html
[link-24]: https://technodrone.blogspot.com/2018/06/comparing-cloudformation-terraform-and.html
[link-25]: https://technodrone.blogspot.com/2018/07/comparing-cloudformation-terraform-and.html
[link-26]: https://developer.epages.com/blog/tech-stories/kubernetes-deployments-with-helm/
[link-27]: https://developer.epages.com/blog/tech-stories/kubernetes-deployments-with-helm-templates/
[link-28]: https://developer.epages.com/blog/tech-stories/kubernetes-deployments-with-helm-secrets/
[link-29]: https://github.com/rockymadden/slack-cli
