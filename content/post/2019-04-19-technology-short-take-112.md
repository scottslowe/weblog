---
author: slowe
categories: Information
comments: true
date: 2019-04-19T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- KVM
- Linux
- Kubernetes
- SSH
- AWS
- Docker
- Windows
- vSphere
title: 'Technology Short Take 112'
url: /2019/04/19/technology-short-take-112/
---

Welcome to Technology Short Take #112! It's been quite a while since the last one, as life and work have been keeping me busy. I have, however, finally managed to pull together this list of links and articles from around the Internet, and I hope that something I've included here proves useful to readers.<!--more-->

## Networking

* [Part 2][link-13] of Stark and Wayne's container-to-container networking for Cloud Foundry and Kubernetes digs deep into CNI workings.
* Via Ivan Pepelnjak's site, Albert Siersema shares some information on using Ansible to [automate 802.1x configurations][link-24].
* Milind Gunjan shares some [tips for troubleshooting Linux bridged networking on a KVM host][link-28].

## Servers/Hardware

Nothing this time around! I'll stay alert for content I can include next time.

## Security

* Tim Hinrichs discusses [securing the Kubernetes API with Open Policy Agent][link-4].
* Pod Security Policies (PSPs) are an important security feature in Kubernetes. Sysdig [explains][link-2] PSPs, and talks about `kube-psp-advisor`, a tool to help simplify deploying PSPs.
* [ClusterScope][link-3] is a handy tool for finding outdated images in your Kubernetes cluster.
* [This article][link-12] discusses four open source secrets management tools.
* Many organizations prefer to use two-factor authentication (2FA) to help protect their systems. While this article on [how to configure 2FA for SSH on Fedora][link-11] probably won't work in many corporate environments (few use Fedora), it may provide enough information to figure out what it would look like in your environment.

## Cloud Computing/Cloud Management

* Bahubali (Bill) Shetti walks through [analyzing the cost of a self-managed Kubernetes cluster on AWS using VMware CloudHealth][link-1].
* Ahmet Alp Balkan does [a deep dive on the KUBECONFIG file][link-17].
* Lee Briggs writes about [his experience with Fargate][link-9]. I think the key takeaway here is that prior experience _always_ affects our perceptions and how we go about learning new technologies/acquiring new skills. My prior experience with hypervisors (vSphere, then KVM) affected how I learned Docker and containers. Lee's prior experience with Kubernetes affected how he learned Fargate. Someone who'd worked quite a bit with Fargate would probably have a hard time switching to Kubernetes. An individual's learning curve is strongly dictated by previous experience and knowledge.
* Ernese Norelus has [an introductory piece][link-26] on using Terraform and Ansible to enable repeatable infrastructure builds on AWS.
* Fernand Galiana [introduces Popeye][link-27], a tool for finding and identifying misconfigurations in your Kubernetes cluster. I haven't had the chance to give it a try yet, but it looks pretty interesting.
* Aeva talks a bit about [what happened to OpenStack][link-30]. Key excerpt (for me) from this article was this statement: "...creating a viable, open source, hyperscale cloud software solution was against the best interest of the companies most heavily investing in OpenStack’s development."

## Operating Systems/Applications

* Brian Christner talks about [becoming a Docker "power user" with VS Code][link-19].
* John Harris explains [how to use dynamic configuration discovery in Grafana][link-18].
* Matthias Eisner has written an article on [custom XML objects in vRealize Orchestrator][link-22].
* Tonis Tiigi has a write-up on [experimenting with rootless Docker][link-10] over on the Docker Engineering blog. Pay close attention to the current caveats---some of them are significant (there's a reason this is still experimental).
* I loved [this "quick tip"][link-8] on setting the default format for commands like `docker ps`. Very handy!
* There are some useful Bash tips [here][link-25].
* Mark Church from Docker provides [an update on Windows containers with Docker and Kubernetes][link-29], specifically calling out the support for Windows node as of the Kubernetes 1.14 release. Here's [Microsoft's side][link-31] of the same announcement.

## Storage

Nothing this time. Have something you think I should share here? Let [me know on Twitter][link-99].

## Virtualization

* Larry Smith Jr. [shares some information][link-21] learned in building nested ESXi templates.

## Career/Soft Skills

* [This blog post][link-20] from XMind has some nice tips on staying focused in the workspace.
* I really enjoyed [this discussion][link-23] on deep work and real-time collaboration. Cal's book is in the "To Read" pile on my desk; guess I need to hurry up and get to it!

That's all for now---stay tuned for future Tech Short Takes, as I'm striving to be more regular with publishing them. In the meantime, feel free to [contact me on Twitter][link-99] with any comments, suggestions, corrections, or other feedback.

[link-1]: https://medium.com/@bahubalishetti/analyzing-self-managed-kubernetes-cluster-cost-on-aws-via-cloudhealth-8a1c0b30156b
[link-2]: https://sysdig.com/blog/enable-kubernetes-pod-security-policy/
[link-3]: https://www.replicated.com/clusterscope/
[link-4]: https://blog.openpolicyagent.org/securing-the-kubernetes-api-with-open-policy-agent-ce93af0552c3
[link-5]: https://sookocheff.com/post/messaging/evolving-messaging-for-microservices/
[link-6]: https://www.wolfe.id.au/2017/11/05/aws-user-federation-with-keycloak/
[link-7]: https://blog.alexellis.io/get-started-with-openfaas-and-kind/
[link-8]: https://container42.com/2016/03/27/docker-quicktip-7-psformat/
[link-9]: http://leebriggs.co.uk/blog/2019/04/13/the-fargate-illusion.html
[link-10]: https://engineering.docker.com/2019/02/experimenting-with-rootless-docker/
[link-11]: https://fedoramagazine.org/two-factor-authentication-ssh-fedora/
[link-12]: https://opensource.com/article/19/2/secrets-management-tools-git
[link-13]: https://starkandwayne.com/blog/container-to-container-networking-for-cloud-foundry-and-kubernetes/
[link-14]: https://winterwindsoftware.com/serverless-migration-journal/
[link-15]: https://www.thectoadvisor.com/blog/2019/3/7/hybrid-isnt-a-place-its-an-operating-model
[link-16]: https://medium.com/@PaulDJohnston/we-need-to-stop-calling-them-sprints-couch-to-5k-is-the-right-analogy-22eb41729acf
[link-17]: https://ahmet.im/blog/mastering-kubeconfig/
[link-18]: https://johnharris.io/2019/03/dynamic-configuration-discovery-in-grafana/
[link-19]: https://www.brianchristner.io/docker-and-microsoft-vs-code/
[link-20]: https://www.xmind.net/blog/en/2019/01/4-tips-1-feature-staying-focused/
[link-21]: https://everythingshouldbevirtual.com/virtualization/Nested-ESXi-Templates/
[link-22]: https://blog.comdivision.com/blog/2019/03/custom-xml-objects-in-vrealize-orchestrator
[link-23]: https://blog.nuclino.com/slack-is-not-where-deep-work-happens
[link-24]: https://blog.ipspace.net/2019/04/automating-8021x-part-one.html
[link-25]: https://elder.dev/posts/safer-bash/
[link-26]: https://medium.com/devopslinks/building-repeatable-infrastructure-with-terraform-and-ansible-on-aws-3f082cd398ad
[link-27]: https://itnext.io/k8s-clusters-oh-biff-em-popeye-637e9312963
[link-28]: https://medium.com/@milind.gunjan/troubleshooting-linux-bridge-networking-issue-on-kvm-host-81193ced71de
[link-29]: https://blog.docker.com/2019/03/advancing-windows-containers-with-docker-and-kubernetes/
[link-30]: https://aeva.online/2019/03/what-happened-to-openstack/
[link-31]: https://cloudblogs.microsoft.com/opensource/2019/03/25/windows-server-containers-now-supported-kubernetes/
[link-99]: https://twitter.com/scott_lowe
