---
author: slowe
categories: Information
comments: true
date: 2018-01-05T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- OpenStack
- Ansible
- Linux
- Python
- SSH
- Windows
- Microsoft
- VMware
- vSphere
- CLI
- Terraform
- Azure
- Kubernetes
- Intel
title: 'Technology Short Take 92'
url: /2018/01/05/technology-short-take-92/
---

Welcome to Technology Short Take 92, the first Technology Short Take of 2018. This one was _supposed_ to be the last Tech Short Take of 2017, but I didn't get it published in time (I decided to spend time with my family instead---some things are just more important). In any case, hopefully the delay of one additional week hasn't caused any undue stress---let's jump right in!<!--more-->

## Networking

* Lindsay Hill walks through [using Telegraf, InfluxDB, and Grafana to monitor network statistics][link-5].
* Via Ivan Pepelnjak, I found [this article][link-13] by Diane Patton at Cumulus Networks talking about container network designs. The article is a bit heavy on pushing the Host Pack (a Cumulus thing), but otherwise provides a good overview of several different possible container network designs, along with some of the criteria that might lead to each design.
* Erik Hinderer takes a stab (based on his field experience) at [estimating how long it takes to upgrade VMware NSX][link-16]. Erik's figures are just estimates, of course; actual values will be determined based on each customer's specific environment.
* [This post][link-25] is a bit older, but covers a challenge faced by cloud-native darling Netflix---how does one, exactly, identify which application used which IP address at a given point in time? When you're operating at the scale at which Netflix operates, this is no trivial feat.

## Servers/Hardware

* Christian Kellner talks about work done on [Thunderbolt 3 security levels for GNU/Linux][link-22]. There is more work ahead, but it's great to see progress being made.

## Security

* The CPU architecture flaw involving speculative execution has been garnering a great deal of attention (see [here][link-26], [here][link-31], [here][link-32], and [here][link-33]). Also, here's [Google Project Zero's write-up][link-34] (along with [a support FAQ][link-35] from Google on mitigation). There's lots more coverage, obviously, but this should be enough to get you started.

## Cloud Computing/Cloud Management

* Kevin Carter has [a detailed write-up][link-1] on efforts around leveraging `systemd-nspawn` for deploying OpenStack via OpenStack Ansible. `systemd-nspawn` is an interesting technology I've been watching since early this year, and it will be cool (in my opinion) to see a project using it in this fashion.
* The vSphere provider for Terraform (did you know there was one?) recently hit 1.0, and HashiCorp has [a blog post (re-)introducing the provider][link-7]. I thought I also saw a VMware blog post on the provider as well, but couldn't find any link (guess I was mistaken).
* Oh, and speaking of Terraform: check out [this post][link-11] on the release of Terraform 0.11.
* Tim Nolet reviews some [differences between Azure Container Instances and AWS Fargate][link-8] (recently announced at AWS re:Invent 2017). Tim's review of each of the offerings is pretty balanced (thanks for that), and I'd recommend reading this post to get a better idea of how each of them work.
* Jorge Salamero Sanz (on behalf of Sysdig) provides a similar comparison, this time [looking at ECS, Fargate, and EKS][link-9]. Jorge's explanation of Fargate as "managed ECS/EKS instances" is probably the most useful explanation of Fargate I've seen so far.
* Michael Gasch digs relatively deep to address the question of [how Kubernetes reconciles allocatable resources and requested resources][link-12] in order to satisfy QoS. Good information here, in my opinion. Thanks Michael!
* Running distributed systems such as etcd, Kubernetes, Linkerd, etc., to support applications means making a conscious decision to embrace a certain level of complexity in exchange for the benefits these systems offer. Read [this post-mortem on an outage][link-15] to gain a better idea of some of the challenges this additional complexity might present when it comes to troubleshooting.
* Tim Hinrichs [provides some details on Rego][link-20], the policy language behind the [Open Policy Agent project][link-21].
* Paul Czarkowski walks you through [creating your first Helm chart][link-29].

## Operating Systems/Applications

* I came across [this mention of Mitogen][link-2], a project whose goal---as described by the creator---is to "make it childsplay [_sic_] to run Python code on remote machines". 
* From the "interesting-but-not-practicallly-useful" department, Nick Janetakis shows [how to use Docker to run a PDP-11 simulator][link-3]. The magic here, in my opinion, is in the simulator (not in Docker), but it's still an interesting look at how one might use Docker.
* Also from Nick, [here's an attempt][link-18] to the answer the question, "Do I learn Docker Swarm or Kubernetes?"
* I debated on adding this link because I wasn't sure how useful it might be to readers, but decided to include it anyway. [Apache Guacamole][link-4] describes itself as "a clientless remote desktop gateway" supporting standard protocols like SSH, VNC, and RDP.
* Tamás Török has [a quite lengthy post on transforming your system into microservices][link-10]. It's nice to see some of the challenges---which aren't all technical---mentioned as well, as sometimes today's tech writers only seem to see microservices through rose-colored glasses.
* [This][link-14] is an awesome collection of patched fonts.
* [OpenSSH on Windows][link-19]---what a time to be alive! It almost makes me want to add a Windows 10 machine to my collection...
* I enjoyed this [developer-centric comparison of Kubernetes and Pivotal Cloud Foundry][link-27].

## Storage

* Tony Bourke has a two-part series on ZFS and Linux and encryption ([part 1][link-23], [part 2][link-24]).

## Virtualization

* Brendan Gregg has [a really good post][link-6] describing the AWS virtualization journey and Nitro, their new lightweight KVM-based hypervisor.
* Here's [a look at using `govc` to manage vSphere from the Linux command line][link-17]. I haven't messed much with `govc` (mainly because I don't have a home lab running vSphere), but it looks pretty cool.
* Luc Dekens shares a PowerShell script to [clean up vSphere permissions][link-28].

## Career/Soft Skills

* Although targeted at "creatives," I think there are some tips and ideas in [this post][link-30] that are equally applicable to IT professionals.

That's it for this time around. Look for the next Technology Short Take in a couple of weeks, where I'll have another curated collection of links and articles for you. Until then, enjoy!



[link-1]: https://cloudnull.io/2017/06/nspawning-openstack-ansible/
[link-2]: http://pythonsweetness.tumblr.com/post/165366346547/mitogen-an-infrastructure-code-baseline-that
[link-3]: https://nickjanetakis.com/blog/run-the-first-edition-of-unix-1972-with-docker
[link-4]: https://guacamole.apache.org
[link-5]: https://lkhill.com/telegraf-influx-grafana-network-stats/
[link-6]: http://www.brendangregg.com/blog/2017-11-29/aws-ec2-virtualization-2017.html
[link-7]: https://www.hashicorp.com/blog/a-re-introduction-to-the-terraform-vsphere-provider
[link-8]: https://hackernoon.com/azure-container-instances-vs-aws-fargate-3216607f63f4
[link-9]: https://sysdig.com/blog/ecs-fargate-eks-kubernetes-aws-compared/
[link-10]: http://codingsans.com/blog/microservice-architecture-best-practices
[link-11]: https://www.hashicorp.com/blog/hashicorp-terraform-0-11
[link-12]: https://embano1.github.io/post/sched-reconcile/
[link-13]: https://cumulusnetworks.com/blog/5-ways-design-container-network/
[link-14]: https://nerdfonts.com/
[link-15]: https://community.monzo.com/t/resolved-current-account-payments-may-fail-major-outage-27-10-2017/26296/95
[link-16]: http://virtuallyread.com/how-long-does-it-take-to-upgrade-vmware-nsx/
[link-17]: http://prefetch.net/blog/index.php/2017/12/19/managing-vsphere-from-the-linux-command-line/
[link-18]: https://nickjanetakis.com/blog/docker-swarm-vs-kubernetes-which-one-should-you-learn
[link-19]: https://blogs.msdn.microsoft.com/powershell/2017/12/15/using-the-openssh-beta-in-windows-10-fall-creators-update-and-windows-server-1709/
[link-20]: https://blog.openpolicyagent.org/opas-full-stack-policy-language-caeaadb1e077
[link-21]: http://www.openpolicyagent.org/
[link-22]: https://christian.kellner.me/2017/12/14/introducing-bolt-thunderbolt-3-security-levels-for-gnulinux/
[link-23]: https://datacenteroverlords.com/2017/12/17/zfs-and-linux-and-encryption-part-1/
[link-24]: https://datacenteroverlords.com/2017/12/17/zfs-on-linux-with-encryption-part-2/
[link-25]: https://blog.thousandeyes.com/how-netflix-tracks-ip-addresses-within-aws/
[link-26]: https://www.theregister.co.uk/2018/01/02/intel_cpu_design_flaw/
[link-27]: https://medium.com/@odedia/comparing-kubernetes-to-pivotal-cloud-foundry-a-developers-perspective-6d40a911f257
[link-28]: http://www.lucd.info/2017/12/08/vsphere-permission-cleanup/
[link-29]: http://tech.paulcz.net/blog/getting-started-with-helm/
[link-30]: http://99u.com/articles/56753/find-friction-forget-logos-fck-buzzwords-fight-assholes-and-42-other-ways-youll-do-the-best-creative-work-of-your-life-this-year
[link-31]: https://wccftech.com/intel-kernel-memory-leak-bug-speculative-execution-performance-hit/
[link-32]: https://www.engadget.com/2018/01/03/intel-kernel-memory-flaw/
[link-33]: https://www.engadget.com/2018/01/03/intel-says-security-issue-is-not-specific-to-its-cpus/
[link-34]: https://security.googleblog.com/2018/01/todays-cpu-vulnerability-what-you-need.html
[link-35]: https://support.google.com/faqs/answer/7622138
