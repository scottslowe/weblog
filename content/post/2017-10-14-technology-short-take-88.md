---
author: slowe
categories: Information
comments: true
date: 2017-10-14T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- AWS
- Terraform
- Cisco
- Azure
- Microsoft
- Encryption
- Puppet
- VMware
- Fedora
- Linux
- vSphere
- OpenStack
- Kubernetes
- Docker
title: 'Technology Short Take 88'
url: /2017/10/14/technology-short-take-88/
---

Welcome to Technology Short Take #88! Travel is keeping me pretty busy this fall (so much for things slowing down after VMworld EMEA), and this has made it a bit more difficult to stick to my self-imposed biweekly schedule for the Technology Short Takes (heck, I couldn't even get this one published on Friday!). Sorry about that! Hopefully the irregular schedule is outweighed by the value found in the content I've collected for you.<!--more-->

## Networking

* Christopher Davis has an article discussing [a recommended VPC subnet configuration][link-11]. This is, of course, just _one_ way of handling subnets within a VPC, but some of the principles outlined in Christopher's article are definitely sound.
* Speaking of VPCs and subnets, here's [an example of a Terraform module][link-12] for a VPC with public, private, and internal subnets (similar to the article in the previous bullet).
* Prefer SaltStack to Terraform? No worries, Calvin Hendryx-Parker has [an example of building AWS VPCs with SaltStack formulas][link-13].
* Phil Bedard (TME at Cisco) has a post discussing [telemetry in Cisco IOS-XR][link-14].
* Fidelis Ekezue has [a high-level "compare-and-constrast" between AWS VPCs and Azure VNETs][link-15]. This article is pretty high-level; I wish it had a bit more depth to it.
* Abdullah Abdullah shares some thoughts on [design decisions regarding NSX VXLAN control plane replication modes][link-17].
* Romain Decker has [an "under the hood" look][link-21] at the VMware NSX load balancer.

## Servers/Hardware

Nothing this time (sorry!). I'll keep an eye open for links to include next time around.

## Security

* Mike Foley [provides an overview][link-26] of key manager concepts and topology basics for VM and vSAN encryption.
* If you're studying for your CISSP, you might find [this article][link-34] helpful.

## Cloud Computing/Cloud Management

* Mark Brookfield [shares the fix][link-2] for missing Puppet attributes on vRealize Automation blueprints.
* Jason Brooks has a write-up discussing [how to run Kubernetes on Fedora Atomic Host][link-3]. I haven't had the opportunity to try this out yet, but hope to do so in the next few weeks.
* [Via Joe Beda on Twitter][link-9], I saw [this announcement][link-8] from CloudFlare about service workers. Joe's comment---"the merging of CDNs and cloud continues"---is pretty spot on, in my opinion.
* Colin Westwater discussing [using Terraform with vSphere][link-18] for "infrastructure as code".
* Here's a post on [upgrading to OpenStack Pike using Kolla and Kayobe][link-19].
* Doug Smith [outlines][link-20] a completely "Docker-free" Kubernetes setup (leveraging `kpod`, `buildah`, and CRI-O).
* [This graphical summary][link-24] of the AWS Application Load Balancer (ALB) is pretty handy.

## Operating Systems/Applications

* For those of you that might be a bit behind the times and still getting up to speed on containers, Ryan Kelly has a post describing [containers for the vSphere admin][link-4].
* The popular open source container registry project, [Harbor][link-7], has released a new version (version 1.2). This version introduces vulnerability scanning (leveraging [the Clair project][link-6]), and Henry Zhang has [a blog post with more details][link-5].
* Brendan Gregg has a pretty detailed write-up on [migrating from Solaris to Linux][link-10] (pertinent given the recent layoff of the majority of the Solaris developers).
* If you're not clear on the role/status of CRI-O, check out [this article][link-22] by Scott McCarty of Red Hat.
* Michael Friis [provides more details][link-23] on a recent big change to the Docker Official Images: they're now multi-platform.

## Storage

* Ben Evans has a four-part series (so far) on vSAN. The series provides [an introduction and overview of licensing][link-28] (part 1), [a review of architecture and hardware recommendations][link-29] (part 2), [an overview of data availability][link-30] (part 3), and [a discussion of fault domains][link-31] (part 4).

## Virtualization

* Vladan Seget shares [how to create a VMware ESXi ISO image with the latest patches][link-1].
* Richard Arnold shares [some thoughts on VMware Cloud on AWS][link-32].
* Wouter Kursten shows how to use PowerCLI [to add ESXi users when the host is in Lockdown Mode][link-33] (and add them to the exception list).

## Career/Soft Skills

* Joel Knight shares [how he's tried to blog more in 2017][link-16]. There may be some tips here to help others seeking to "bulk up" on their blogging.
* Frank Milisi shares some information around stagnation and [how to tell if you're suffering from stagnation][link-25].
* John Allspaw has [a great write-up on blameless post-mortems and a Just Culture][link-27]. This is, in my humble opinion, a great article that _all_ IT professionals need to read, digest, and apply to their own interactions with others.

That's all for this time around. Thanks for reading!



[link-1]: https://www.vladan.fr/how-to-create-vmware-esxi-iso-with-latest-patches/
[link-2]: https://virtualhobbit.com/2017/09/27/wednesday-tidbit-fix-missing-puppet-attributes-on-vrealize-automation-blueprints/
[link-3]: http://www.projectatomic.io/blog/2017/09/running-kubernetes-on-fedora-atomic-26/
[link-4]: http://www.vmtocloud.com/containers-for-the-vsphere-admin-after-school-special-update/
[link-5]: http://www.think-foundry.com/harbor-private-registry-image-vulnerability-scanning-demo/
[link-6]: https://github.com/coreos/clair
[link-7]: https://github.com/vmware/harbor
[link-8]: https://blog.cloudflare.com/introducing-cloudflare-workers/
[link-9]: https://twitter.com/jbeda/status/913772325153079296
[link-10]: http://www.brendangregg.com/blog/2017-09-05/solaris-to-linux-2017.html
[link-11]: https://chrisguitarguy.com/2017/09/30/aws-vpc-subnet-configuration/
[link-12]: https://github.com/agencypmg/terraform-vpc
[link-13]: http://www.sixfeetup.com/blog/build-aws-vpc-with-saltstack
[link-14]: https://xrdocs.github.io/design/blogs/2017-09-21-peering-telemetry/
[link-15]: https://blogs.msdn.microsoft.com/premier_developer/2017/09/17/differentiating-between-azure-virtual-network-vnet-and-aws-virtual-private-cloud-vpc/
[link-16]: https://www.packetmischief.ca/2017/08/29/how-ive-attempted-to-blog-more-in-2017/
[link-17]: http://notes.doodzzz.net/2017/10/03/nsx-vxlan-control-plane-replication-modes-design-decision/
[link-18]: http://www.vgemba.net/vmware/terraform/Terraform-Part-2/
[link-19]: https://www.stackhpc.com/kolla-kayobe-pike.html
[link-20]: http://dougbtv.com/nfvpe/2017/09/20/crio-workflow/
[link-21]: http://cloudmaniac.net/nsx-load-balancer-under-the-hood/
[link-22]: https://medium.com/cri-o/understanding-container-standards-1e1448cbb92c
[link-23]: https://blog.docker.com/2017/09/docker-official-images-now-multi-platform/
[link-24]: https://www.awsgeek.com/posts/aws-alb-summary/
[link-25]: http://ctrl-alt-insert.com/2017/10/10/knowing-stagnation-part-1/
[link-26]: https://www.yelof.com/2017/10/05/key-manager-concepts-and-toplogy-basics-for-vm-and-vsan-encryption/
[link-27]: https://codeascraft.com/2012/05/22/blameless-postmortems/
[link-28]: http://www.definetomorrow.co.uk/blog/2017/9/4/vmware-vsan-a-closer-look-part-1-introducing-and-licensing
[link-29]: http://www.definetomorrow.co.uk/blog/2017/9/18/vmware-vsan-a-closer-look-part-2-architecture-and-hardware
[link-30]: http://www.definetomorrow.co.uk/blog/2017/9/22/vmware-vsan-a-closer-look-data-availabilty
[link-31]: http://www.definetomorrow.co.uk/blog/2017/10/11/vmware-vsan-a-closer-look-part-4-fault-domains-and-stretched-clusters
[link-32]: https://d8tadude.com/2017/10/11/thoughts-on-vmware-cloud-on-aws/
[link-33]: http://www.retouw.nl/esxi/creating-local-esxi-user-in-a-locked-down-situation-and-add-it-to-exception-list/
[link-34]: https://www.studynotesandtheory.com/
