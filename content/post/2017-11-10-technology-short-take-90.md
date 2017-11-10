---
author: slowe
categories: Information
comments: true
date: 2017-11-10T12:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- vSphere
- Automation
- Ansible
- Cisco
- NSX
- AWS
- Kubernetes
- Azure
- Photon
- Linux
- Encryption
title: 'Technology Short Take 90'
url: /2017/11/10/technology-short-take-90/
---

Welcome to Technology Short Take 90! This post is a bit shorter than most, as I've been on the road quite a bit recently. Nevertheless, there's hopefully something here you'll find useful.<!--more-->

## Networking

* VMware recently released an updated to PowerCLI (version 6.5.3), and with it comes a set of cmdlets that provide low-level access to the NSX-T REST API. Romain Decker [provides a walkthrough][link-3]. Have fun automating!
* Looking for some "MPLS 101"-type material? Jon Langemak [has you covered][link-6].
* I really enjoyed David Gee's series on the network automation engineer persona ([part 1][link-8], [part 2][link-9], [part 3][link-10], and [part 4][link-11]). David really unpacks the changes that are happening in the networking space and describes the key skills the "next-generation network engineer" will have. Good stuff!
* This post by Patrick Ogenstad on [Ansible action plugins for Cisco IOS][link-13] is just outside my comfort zone with regards to Python knowledge and skill, but it's only by pushing the boundaries that we learn, right?
* Romain Decker discusses [the ten things you should know before starting with NSX-T][link-20].
* Mark Betz digs into [some Kubernetes networking concepts][link-21] in detail.

## Security

* Tony Reeves shares a procedure for [setting up encryption in vSAN 6.6][link-4].
* Major Hayden [talks about ansible-hardening][link-17], a new role that grew out of an OpenStack-related security role. However, as Major points out in his article, the role works anywhere, OpenStack cloud or not.
* Here's a quick note [about AWS security groups and traffic flows][link-22]. Definitely worth reading.

## Cloud Computing/Cloud Management

* Google is rolling out Kubernetes 1.8 to Google Container Engine; [this blog post][link-12] talks about some of the new features and functionality.
* Lior Kamrat has a multi-part series on an architecture that blends Mesosphere DC/OS, Azure, Docker, and VMware vSphere. Check out the beginning of the series [here][link-23].

## Operating Systems/Applications

* Kevin Lynch of the Squarespace Engineering team has [a great write-up on Linux container scheduling][link-5]; this post provides some in-depth information on the various components and considerations involved.
* After a recent complaint by yours truly on Twitter regarding notifications on Windows 10, I was directed to [this article][link-7]. I haven't tested it yet, but it does look promising.
* VMware [recently released version 2.0 of Photon OS][link-14]. Unfortunately, Photon OS 2.0 has some issues running on VMware platforms, so visit William Lam's site for [the workarounds][link-15].
* This article by Christian Posta [on when not to do microservices][link-16] is, in my opinion, a well-balanced view on when microservices offer versus when they don't.
* Larry Smith Jr. shows [how to take data in CSV and convert it to YAML][link-18] for consumption by Ansible.

## Storage

* I'm clearly behind the times in some of my reading, as [this great article][link-1] by J Metz was just brought to my attention recently. J does a good job of laying out the various competing forces that drive product/technology evolution and selection in the storage space, though one might argue these same forces are at work in other areas besides just storage.

## Virtualization

* Frank Denneman's enormously useful and very successful _vSphere 6.5 Host Resources Deep Dive_ book is now available as a free e-book, courtesy of Rubrik. [Check out Frank's blog for more details.][link-19]

## Career/Soft Skills

* You've heard me talk about Spousetivities many times; here are [some thoughts from a Spousetivities participant][link-2].

Thanks for reading! Feel free to [hit me up on Twitter][link-24] if you have feedback or would like to share a link I should include in a future Technology Short Take.



[link-1]: https://jmetz.com/2016/07/storage-forces/
[link-2]: http://nigelhickey.com/vmworld-2017-spouses-perspective/
[link-3]: http://cloudmaniac.net/powercli-nsx-t-module/
[link-4]: http://www.digitalvspace.com/2017/10/setting-up-vsan-encryption-in-66.html
[link-5]: https://engineering.squarespace.com/blog/2017/understanding-linux-container-scheduling
[link-6]: http://www.dasblinkenlichten.com/mpls-101-the-basics/
[link-7]: https://blogs.technet.microsoft.com/platforms_lync_cloud/2017/05/05/disabling-windows-10-action-center-notifications/
[link-8]: http://ipengineer.net/2017/10/network-automation-engineer-persona-part-one/
[link-9]: http://ipengineer.net/2017/10/network-automation-engineer-persona-part-two/
[link-10]: http://ipengineer.net/2017/10/network-automation-engineer-persona-part-three/
[link-11]: http://ipengineer.net/2017/10/network-automation-engineer-persona-part-four/
[link-12]: https://cloudplatform.googleblog.com/2017/09/google-container-engine-kubernetes-18.html
[link-13]: https://networklore.com/extending-ansible-action-plugins/
[link-14]: https://blogs.vmware.com/cloudnative/2017/11/01/version-2-0-project-photon-os/
[link-15]: https://www.virtuallyghetto.com/2017/11/workarounds-for-deploying-photonos-2-0-on-vsphere-fusion-workstation.html
[link-16]: http://blog.christianposta.com/microservices/when-not-to-do-microservices/
[link-17]: https://major.io/2017/06/27/old-role-new-name-ansible-hardening/
[link-18]: https://everythingshouldbevirtual.com/automation/ansible-parsing-csv-list-hosts-ip-hostnames-mac/
[link-19]: http://frankdenneman.nl/2017/11/07/free-vsphere-6-5-host-resources-deep-dive-ebook/
[link-20]: http://cloudmaniac.net/nsx-t-things-to-know/
[link-21]: https://medium.com/google-cloud/understanding-kubernetes-networking-pods-7117dd28727
[link-22]: http://www.bardev.com/2017/10/22/aws-security-group-stop-traffic-flow/
[link-23]: https://azure.microsoft.com/en-us/blog/mesosphere-dcos-azure-docker-vmware-and-everything-between-architecture-and-ci-cd-flow/
[link-24]: https://twitter.com/scott_lowe
