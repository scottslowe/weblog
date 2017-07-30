---
author: slowe
categories: Information
comments: true
date: 2017-03-03T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Terraform
- PowerCLI
- NSX
- VMware
- Linux
- AWS
- Kubernetes
- CLI
- OpenStack
- Docker
- VirtualBox
- Microsoft
- Windows
- HyperV
- vSphere
- Career
title: 'Technology Short Take #79'
url: /2017/03/03/technology-short-take-79/
---

Welcome to Technology Short Take #79! There's lots of interesting links for you this time around.

## Networking

* I was _sure_ I had mentioned Skydive before, but apparently not (a `grep` of all my blog posts found nothing), so let me rectify that first. Skydive is (in the project's own words) an "open source real-time network topology and protocols analyzer." The project's GitHub repository is [here][link-5], and documentation for Skydive is [here][link-6].
* OK, now that I've mentioned Skydive, I can talk about this article that provides an example of [functional SDN testing with Terraform and Skydive][link-7]. Terraform is used to turn up OpenStack infrastructure, and Skydive (via connections into Neutron and OpenContrail, in this example) is used to validate SDN functionality.
* Tony Sangha took PowerNSX (a set of PowerShell cmdlets for interacting with NSX) and created [a tool to help document the NSX Distributed Firewall configuration][link-13]. This tool exports the DFW configuration and then converts it into Excel format, and is [available on GitHub][link-14]. (What's that? You haven't heard of PowerNSX before? See [here][link-15].)

## Servers/Hardware

Nothing this time around. Should I keep this section, or ditch it? Feel free to give me your feedback [on Twitter][link-31].

## Security

* I found this article on [SELinux concepts for humans][link-3] to be quite helpful. SELinux has been something that I've avoided---yes, avoided---learning because it just seemed too complex. Instead, I should have followed my own advice and started with the vocabulary (which is what this article helps provide).
* The first practical way of generating SHA1 collisions has been discovered and revealed; more details are available [here][link-9]. Lots of things are potentially impacted.
* Here's [a great article][link-20] highlighting some of the risks involved in relying only on HTTPS certificates to denote the "security" or "validity" of a site. My takeaway: double-check the address bar and look at the certificate!

## Cloud Computing/Cloud Management

* Massimo Re Ferre pointed me to an article by "Cloud Opinion" that discusses [the upcoming SaaSocalypse][link-1] that will be created by the next generation of SaaS vendors leveraging FaaS (Function-as-a-Service, as typified by AWS Lambda) and other "serverless" features. The efficiency described by Cloud Opinion is similar to what James Watters discussed in [this post from 2013][link-2] (also thanks to Massimo), which extols the benefits of using Cloud Foundry on top of "plain" IaaS offerings in order to increase utilization/efficiency.
* Check out [this article][link-4] that discusses using AWS Spot Instances and spotinst to run low-cost Kubernetes clusters.
* Brendan Burns, co-founder of Kubernetes and now an architect at Microsoft Azure, has [a blog post][link-8] about how Kubernetes enables containers-as-a-service (CaaS), which in turn is the foundation for the next generation of Platform-as-a-Service (PaaS).
* You know an effort/project is starting to be "real" when [a logo gets designed][link-12]. Such is the case with CRI-O (the Kubernetes Container Runtime Interface for OCI).
* I recently started experimenting with `awless`, a CLI tool for working with Amazon Web Services (AWS). So far, I really like it. Not sure if it will replace my use of the AWS CLI, but it may become a useful tool in my toolbelt. Check it out [on GitHub][link-25].
* Sebastian Goasguen has a good overview of [federating Kubernetes clusters][link-27].
* I just noticed this article about [using Nova flavor extra-specs to pass QoS data down to the virtualization layer][link-28]. That's handy.
* This is a good article on [refactoring Terraform against existing infrastructure][link-29] using `terraform state` commands.

## Operating Systems/Applications

* Gojko Adzic has [a great article][link-10] describing his organization's transition to serverless and lessons learned as a result of that transition. For me, the key takeaway was that you need to carefully examine the architecture of your application(s) in order to understand where serverless (function-as-a-service) is the best fit.
* [Here's a walkthrough][link-11] to install Arch Linux on VirtualBox.
* I like the idea of using Docker labels to include a reference to a commit in a version control system as suggested [in this article][link-16], but as I was thinking about it this morning I had a question. If you are storing your Dockerfile in version control (which is a good idea, I would think), then changing the Dockerfile to add the commit reference is itself a change that needs to be committed, so any commit reference you add to the Dockerfile will be, at best, 1 commit behind (becuase you'll have to make a commit to record the reference in the Dockerfile). I suppose it doesn't really matter, it's just something about which my weird brain was thinking.
* I came across this article on [using SystemTap to help with containerization][link-17] by identifying the capabilities that a particular executable (or container) needs.
* Here's a list of [some open source Docker-related tools that developers may find useful][link-22]. There were a couple of projects there I hadn't seen before, and a number of them that I fully expected to see listed.
* "The HFT Guy" has returned with [an update on Docker in production][link-23].
* Distributed key-value stores---such as Zookeeper, Consul, and etcd---are an increasingly important part of application architectures (as well as an important part of the orchestration frameworks that manage applications). For that reason, [this article][link-24] comparing the performance of the three key-value stores I named earlier may be worth reading. Keep in mind that this article was written by the etcd team, so the fact that etcd performs favorably in most results must be weighed accordingly.
* Tim Fairweather has a really useful article on [using variables in loops in Jinja2 templates][link-26].

## Storage

* Alan Renouf shows you [how to retrieve NVMe storage device details][link-32] using PowerCLI.

## Virtualization

* Thinking of virtualizing Linux on Hyper-V? Then you may find this list of [tips for Linux performance on Hyper-V][link-19] to be useful.
* Frank Denneman and his partner in crime Niels Hagoort have launched a new book effort aimed at providing a deep-dive into host resources in vSphere 6.5 environments. Check out the details [here][link-30].
* Microsoft recently added an overlay network driver with support for Docker in Windows 10; check out [this Microsoft blog post][link-33] for more information.

## Career/Soft Skills

* I really enjoyed this article by Evgeny Zislis on [DevOps transformation using Theory of Constraints][link-18]. I'm including it here in the "Career/Soft Skills" section because I think the four questions listed in this article apply to _all_ IT professionals, not just those in "DevOps" roles. (Of course, the idea of a "DevOps" role is a topic unto itself, but I'll leave that for some future article.)
* I also enjoyed this article by Tim Bray on [geek career paths][link-21]. This is something I've been contemplating for a few years now, wondering if it was time for me to make the move to a less technical and more managerial/leadership role (yes, I know those two are different). Tim's article provided me with some useful food for thought.

That's all for now---after all, I have to save some stuff for the next one (which you can expect in about 2 weeks). I hope you found something useful, and have a great weekend!



[link-1]: https://medium.com/@cloud_opinion/upcoming-saasocalypse-34e7e2ecf3ca#.irsb76h8d
[link-2]: https://wattersjames.com/2013/05/09/economics-of-application-virtulization-on-aws/
[link-3]: https://learntemail.sam.today/blog/selinux-concepts-but-for-humans/
[link-4]: http://blog.spotinst.com/2016/08/04/elastigroup-kubernetes-minions-steroids/
[link-5]: https://github.com/skydive-project/skydive
[link-6]: https://skydive-project.github.io/skydive/
[link-7]: https://dev.cloudwatt.com/en/blog/functionnal-sdn-testing.html
[link-8]: http://blog.kubernetes.io/2017/02/caas-the-foundation-for-next-gen-paas.html
[link-9]: https://security.googleblog.com/2017/02/announcing-first-sha1-collision.html
[link-10]: https://gojko.net/2017/02/23/serverless-migration-lesson.html
[link-11]: https://www.howtoforge.com/tutorial/install-arch-linux-on-virtualbox/
[link-12]: http://blog.linuxgrrl.com/2017/02/22/a-logo-for-cri-o/
[link-13]: https://tonysangha.com/2016/10/20/documenting-the-nsx-v-dfw-with-powernsx/
[link-14]: https://github.com/tonysangha/PowerNSX-DFW2Excel
[link-15]: https://github.com/vmware/powernsx
[link-16]: http://blog.microscaling.com/2016/06/the-joy-of-organising-container-image_6.html
[link-17]: https://dzone.com/articles/a-script-to-help-with-containerization
[link-18]: https://blog.devopspro.co.uk/devops-theory-of-constraints-cf1477f9bd1a#.owy576pzl
[link-19]: http://www.blueshiftblog.com/?p=4010
[link-20]: https://textslashplain.com/2017/01/16/certified-malice/
[link-21]: https://www.tbray.org/ongoing/When/201x/2017/02/18/Geek-Career-Paths
[link-22]: http://opensourceforu.com/2017/02/docker-tools-developers/
[link-23]: https://thehftguy.com/2017/02/23/docker-in-production-an-update/
[link-24]: https://coreos.com/blog/performance-of-etcd.html
[link-25]: https://github.com/wallix/awless
[link-26]: http://www.arctiq.ca/our-blog/2017/2/16/ansible-jinja-warrior-loop-variable-scope
[link-27]: https://www.linux.com/learn/federating-your-kubernetes-clusters-new-road-hybrid-clouds
[link-28]: https://blogs.vmware.com/openstack/resources-guarantee-openstack-and-vmware-integrated-openstack/
[link-29]: https://ryaneschinger.com/blog/terraform-state-move/
[link-30]: http://frankdenneman.nl/2017/02/24/vsphere-6-5-host-deep-dive-update/
[link-31]: https://twitter.com/scott_lowe
[link-32]: http://www.virtu-al.net/2017/01/20/retrieving-nvme-details-powercli/
[link-33]: https://blogs.technet.microsoft.com/virtualization/2017/02/09/overlay-network-driver-with-support-for-docker-swarm-mode-now-available-to-windows-insiders-on-windows-10/
