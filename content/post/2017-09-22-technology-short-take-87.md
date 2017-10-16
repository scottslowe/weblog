---
author: slowe
categories: Information
comments: true
date: 2017-09-22T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- Ansible
- OpenStack
- Docker
- AWS
- Kubernetes
- VMware
- Terraform
- VPN
- Ubuntu
- Encryption
title: "Technology Short Take 87"
url: /2017/09/22/technology-short-take-87/
---

Welcome to Technology Short Take #87! I have a mix of newer and older items for you this time around. While I'm a bit short on links in some areas, hopefully this is outweighed by some good content in other areas. Here's hoping you find something useful!<!--more-->

## Networking

* Vincent Bernat has a really in-depth article on [IPv4 route lookup on Linux][link-12] (and [one on IPv6 route lookup][link-13] as well).
* Ivan Pepelnjak has [a great article][link-17] that tries to get to the kernel of truth in the middle of the intent-based networking hype.
* Jason Edelman of Network2Code also has a post on [intent-based network automation with Ansible][link-22], in which he breaks down the idea of intent-based networking (IBN) and how tools such as Ansible or NAPALM can make it possible.
* From the Department of "Sitting in my Inbox for Way Too Long", I wanted to point out a company that I ran into back in May of this year at the OpenStack Summit in Boston. The company is [VirTool Networks][link-18] (catchy, eh?), and their product (VirTool Network Analyzer) is aimed at providing some operational visibility into OpenStack virtual networks. I saw a demo of the product---it looks quite handy, and provides a pretty fair amount of information to help operators figure out what's happening "under the covers." If your shop is into OpenStack, you might want to give this tool a look.
* Ajay Chenampara (also with Network2Code) has a good post on [lessons learned from interacting with NETCONF and YANG][link-32] (key lesson: there's two ways to use NETCONF, and one of them doesn't involve YANG).

## Servers/Hardware

Nothing this time around, but stay tuned for something next time.

## Security

* Chris Binnie walks you through the process of [using user namespaces to help secure your Docker hosts and containers][link-2].
* Joep Piscaer discusses his use of ProtonVPN as [an always-on VPN for his home network][link-5].
* Sadequl Hussain has [an introduction to basic concepts in SELinux][link-14] that may be worth reading if you're unfamiliar with SELinux (as it seems many people are).
* Virtue Security has some articles on AWS penetration testing: [one on S3 buckets][link-26] and [one that also covers IAM and EC2][link-27].

## Cloud Computing/Cloud Management

* Alexander Holbreich has a write-up on [how to install Kubernetes on Ubuntu][link-1].
* Corey Quinn dives into some of [the "secret sauce" behind Amazon Route 53][link-15]; specifically, health checks and traffic policies.
* Ádám Sándor has launched a blog series ([chapter 1 is available][link-19] now) that mirrors Kelsey Hightower's [Kubernetes the Hard Way tutorial on GitHub][link-20]. I'm looking forward to future chapters in this series.
* The folks over at Cloudify recently did a survey on the state of "enterprise multi-cloud"; the report is available [here][link-21] (doesn't appear to be behind a paywall/regwall). A few interesting things jumped out at me from the report; one of them was regarding an increase in "siloism" as organizations embraced multiple clouds. Anyway, you may find the report interesting.
* AWS goes to [per-second billing][link-23] for EC2 instances and EBS volumes.
* Massimo Re Ferre has a great article discussing [VMware Cloud on AWS versus Azure Stack][link-24] and breaking down the differences between the two approaches.

## Operating Systems/Applications

* [RIP Solaris][link-3].
* Chris Collins [takes a deeper look at Buildah][link-4], a tool for building container images (including Docker-compatible container images).
* Dusty Mabe has a multi-part blog series on Atomic Host. The series starts with [part 0][link-6] (preparation), and continues with [part 1][link-7] (mostly about `rpm-ostree`), [part 2][link-8] (container storage), [part 3][link-9] (rebase, upgrade, and rollback), [part 4][link-10] (package layering and experimental features), and [part 5][link-11] (containerized and non-containerized applications). There's a ton of information in this series!
* Ryan Lane provides a fairly in-depth overview of [why Lyft uses SaltStack for AWS orchestration instead of Terraform][link-16]. Lane makes some great points, but I think it's also really important to highlight that this decision also comes with the burden of maintaining custom Python state modules and other code. This isn't a bad thing; it's just something to consider if you or your organization is considering a similar choice.
* Andrew Montalenti [discusses the state of Linux on the desktop][link-28] by examining his own journey with various Lenovo-branded laptops.
* I recently used a variation of the information in [this article][link-31] to keep OTR private keys in sync among a group of OS X-based systems.

## Storage

* I wouldn't normally include this sort of link, but I know that I personally have benefited from J Metz' storage knowledge so this seems like a reasonable way of returning the favor (he was a huge help me to "back in the day" when I was writing [FCoE posts][xref-1]). J has launched [a Patreon page][link-33] to help drive funding to enable him to create new storage-related content. If storage is your thing, have a look and see if this is something that makes sense for you.

## Virtualization

* John Kozej walks through [how to configure vCenter HA using the NSX load balancer][link-25].
* Ivan Pepelnjak [speaks frankly about VMware Cloud on AWS][link-30]. (And don't you just love the new design on his site?)

## Career/Soft Skills

* Julia Evans has a good post on [how to answer questions well][link-29].

Alright, it's time to wrap up now. I hope you've found something useful, and look for the next Technology Short Take in about 2 weeks.



[link-1]: http://alexander.holbreich.org/kubernetes-on-ubuntu/
[link-2]: http://www.devsecops.cc/devsecops/hardening_docker_hosts.html
[link-3]: http://dtrace.org/blogs/bmc/2017/09/04/the-sudden-death-and-eternal-life-of-solaris/
[link-4]: http://chris.collins.is/2017/08/17/buildah-a-new-way-to-build-container-images/
[link-5]: https://www.virtuallifestyle.nl/2017/09/selectively-route-traffic-protonvpn/
[link-6]: https://dustymabe.com/2017/08/29/atomic-host-101-lab-part-0-preparation/
[link-7]: https://dustymabe.com/2017/08/30/atomic-host-101-lab-part-1-getting-familiar/
[link-8]: https://dustymabe.com/2017/08/31/atomic-host-101-lab-part-2-container-storage/
[link-9]: https://dustymabe.com/2017/09/01/atomic-host-101-lab-part-3-rebase-upgrade-rollback/
[link-10]: https://dustymabe.com/2017/09/02/atomic-host-101-lab-part-4-package-layering-experimental-features/
[link-11]: https://dustymabe.com/2017/09/03/atomic-host-101-lab-part-5-containerized-and-non-containerized-applications/
[link-12]: https://vincent.bernat.im/en/blog/2017-ipv4-route-lookup-linux
[link-13]: https://vincent.bernat.im/en/blog/2017-ipv6-route-lookup-linux
[link-14]: https://www.digitalocean.com/community/tutorials/an-introduction-to-selinux-on-centos-7-part-1-basic-concepts
[link-15]: https://read.acloud.guru/the-secret-sauce-behind-amazon-route53-dae2573293c6
[link-16]: https://eng.lyft.com/saltstack-as-an-alternative-to-terraform-for-aws-orchestration-cd2ceb06bf8c
[link-17]: http://blog.ipspace.net/2017/09/intent-based-hype.html
[link-18]: http://virtoolnetworks.com/
[link-19]: http://container-solutions.com/kubernetes-hard-way-explained-chapter-1/
[link-20]: https://github.com/kelseyhightower/kubernetes-the-hard-way
[link-21]: http://cloudify.co/2017-state-of-enterprise-multi-cloud-report/
[link-22]: http://jedelman.com/home/intent-based-network-automation-with-ansible/
[link-23]: https://aws.amazon.com/blogs/aws/new-per-second-billing-for-ec2-instances-and-ebs-volumes/
[link-24]: http://www.it20.info/2017/09/vmware-cloud-on-aws-vs-azure-stack/
[link-25]: https://thewificable.com/2017/04/07/configure-vcenter-ha-using-the-nsx-load-balancer/
[link-26]: https://www.virtuesecurity.com/blog/aws-penetration-testing-s3-buckets/
[link-27]: https://www.virtuesecurity.com/blog/aws-penetration-testing-part-2-s3-iam-ec2/
[link-28]: http://www.pixelmonkey.org/2017/09/01/lenovo-linux
[link-29]: https://jvns.ca/blog/answer-questions-well/
[link-30]: http://blog.ipspace.net/2017/09/comments-vmware-cloud-on-aws.html
[link-31]: https://parkerhiggins.net/2012/01/howto-transfer-otr-private-keys-between-adium-and-pidgin/
[link-32]: https://termlen0.github.io/2017/07/15/observations/
[link-33]: https://www.patreon.com/drjmetz
[xref-1]: /tags/fcoe
