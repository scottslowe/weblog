---
author: slowe
categories: Information
comments: true
date: 2015-12-11T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- vSphere
- VMware
- Microsoft
- Windows
- HyperV
- Docker
- Kubernetes
- Ansible
- Linux
- OpenStack
title: 'Technology Short Take #57'
url: /2015/12/11/technology-short-take-57/
---

Welcome to Technology Short Take #57. I hope you find something useful here!

## Networking

* Tor Anderson has [an article][link-5] on using IPv6 for network boot using UEFI and iPXE.
* Larry Smith Jr. has a great blog series going called "Hey, I can DevOps my Network too!" He started out with [an intro post][link-12] just to set the stage for the series, then had [a post on the prep work][link-13] required to get ready to proceed with the series. In part 2 Larry walks through [the node definitions in Vagrant][link-14], and in part 3 he reviews the `Vagrantfile` and [turns up the environment][link-15]. Parts 4 and 5 walk through [auto-configured OSPF][link-16] and [manually-configured OSPF][link-17], respectively. I'm looking forward to more posts in this series!
* Sometimes I see blog posts that tout themselves as a "deep dive" but aren't really _that_ deep or detailed. However, [this OVN L3 deep dive by Gal Sagie][link-27] really _is_ a deep dive---he goes into a pretty fair amount of detail on how OVN's L3 implementation works. Good stuff if you're interested in getting more details on how OVN is implementing new features like L3.
* Kirk Byers has [a helpful article][link-31] that provides some suggestions and guidelines for how to make your network automation/network scripts become more than just your own personal hobby at work.

## Servers/Hardware

* If you're looking for a cost-effective home lab setup, you might check out [this article][link-29] on a $500 vCloud Suite lab.

## Security

* We all know that security is more than just a host-based firewall, but a host-based firewall can be part of an overall security strategy. [This article][link-30] provides a good introductory overview of Linux `iptables` commands for configuring host-based firewall rules on your Linux systems.

## Cloud Computing/Cloud Management

* Eric Wright has a couple of posts I saw on the OpenStack Cookbook lab, which comes out of the most-excellent _OpenStack Cloud Computing Cookbook_ (now in its third edition, by Kevin Jackson, Cody Bunch, and Egle Sigler). First up is a post [on installing the OpenStack Cookbook lab on OS X][link-23], followed by a post on [suspending and restarting the OpenStack Cookbook lab][link-24]. If you're just getting started with OpenStack, one of the first books you buy should be _OpenStack Cloud Computing Cookbook_, and one of the first things you should do after you buy the book is set up the lab.
* The areas of orchestration systems and resource scheduling are getting a bit more connected due to the efforts of Kubernetes-Mesos, which recently [announced the release of version 0.7.0][link-32]. What is Kubernetes-Mesos, you ask? It's a way to run Kubernetes (the container orchestration system) on top of Mesos (the cluster resource scheduler) as a native Mesos framework.

## Operating Systems/Applications

* Benny Cornelissen has [a really good article][link-1] talking about the value of Consul, the distributed key/value store, in providing valuable infrastructure services. A lot of people think tools like Consul are only valuable in "next-generation" microservices-based architectures, but Benny's article shows how it can add a lot of value in "traditional" architectures. I _highly_ recommend this article.
* Nigel Poulton attended DockerCon EU and has an article sharing some early thoughts on [the state of Docker and Windows containers][link-2]. While it's still early yet (and Nigel fully acknowledges this in his article), I will say that I'm not terribly surprised by some of his observations. The Windows architecture is so dramatically different from UNIX/Linux that trying to adapt per-process containers is quite naturally going to be more than a little bit challenging.
* And speaking of containers on Windows: this past week, Microsoft released Windows Server 2016 Technical Preview 4, with the first public preview of Hyper-V Containers and---according to [this blog post][link-6]---"significant enhancements to both Windows Server Containers and the Docker engine for Windows."
* Microsoft's focus on Windows Server 2016 is increasing; I also saw [this post on moving to Nano Server][link-9], the new "subset" of Server Core, that will make its appearance in Windows Server 2016. It appears that applications that were written for Nano Server may require some porting/modification, which in turn may negatively impact the adoption of Nano Server among Microsoft's customers and ISVs.
* Need a "cheat sheet," so to speak, for VMware's cloud-native applications initiatives? [Look no further.][link-3]
* This post is slightly older (it's from June 2015), but still helpful, especially if you're trying to wrap your mind around the idea of a "pod" in Kubernetes. The article discusses [patterns for composite containers][link-18]---in other words, examples where a pod (a group of containers) make the ideal use case. As I said, it's definitely worth reading if you're (relatively) new to the Kubernetes scene and are trying to get some concrete examples of how the Kubernetes concepts play out.
* Just because I can: here's an article on [updating OS X using the command-line `softwareupdate` tool][link-20]. Have fun.
* Tyler Cross has an article on [using YAML as YAML with Ansible][link-28]. This may sound weird, but if you've worked with Ansible you know it supports both YAML and a "YAML-like" syntax. Tyler's article argues for using "pure" YAML, a suggestion that I second (I switched my playbooks to YAML some time ago and found them to be much more readable).

## Storage

Nothing this time around, but I'll keep searching for useful articles to include in the future.

## Virtualization

* William Lam shares (via guest author John Clendenen) some information on [running ESXi 6.0 on an Apple Xserve 3,1][link-4]. William also has a great post on migrating ESXi to a Distributed Virtual Switch with a single NIC running vCenter Server (how's that for a mouthful?). The trick, [as William explains][link-8], lies in disabling the network rollback functionality while also using ephemeral port binding.
* To those who say that "virtualization is dead, long live the container," I can only point to the numerous mashups between "traditional" virtualization and OS container concepts such as those espoused by Docker: vSphere Integrated Containers (VIC), Hyper-V Containers, Intel's Clear Containers, and now [RancherVM (KVM VMs inside Docker containers)][link-7]. (Here's [an article][link-19] providing a bit more details on RancherVM and using it with Kubernetes.) Yes, containerization is a key part of the future, but virtualization isn't going away anytime soon.
* If you're interested in a bit more detail on the components and architecture of VMware's newly-open source Photon Controller (here's [the GitHub repo][link-10]), check out [this blog post][link-11].
* Ben Armstrong explains the reasons behind [two Hyper-V PowerShell modules in Windows 10][link-21]. (Hint: it's about supporting multiple versions of Hyper-V.)
* Ed Haletky [shares][link-22] some pain he experienced recently with a KVM host running Open vSwitch (OVS). I don't normally see _any_ of the issues that Ed experienced with his KVM host, but this may be due to some Linux distribution differences (Ed's running CentOS, whereas I'm running Ubuntu). In any case, if you're running KVM with OVS, you might find some of the scripts that Ed presents useful.
* Paul Gifford has an install script to turn up a complete container lab using VMware AppCatalyst, Docker Machine, Vagrant, Ansible, and Packer. Check out [the blog post][link-26] for more details.

## Career/Soft Skills/Productivity

* JJ Asghar has a post on [some characteristics of a successful chatroom meeting][link-25]. Given the prevalent use of IRC and IRC meetings in open source projects (which is fine by me, I don't mind using IRC at all), this is a helpful post for both organizers and attendees. I particularly appreciated the suggestions around "time-boxed pauses" to encourage attendee feeback; I think this is something that too often gets overlooked.

That's it for now. There's so much content out there; I'd love to include more but these posts are already long enough as it is. I guess I need to try to increase the frequency at which I publish these things!



[link-1]: http://blog.bennycornelissen.nl/consul-the-end-of-the-cname/
[link-2]: http://blog.nigelpoulton.com/docker-on-windows-state-of-play/
[link-3]: http://blog.think-v.com/?page_id=3277
[link-4]: http://www.virtuallyghetto.com/2015/11/esxi-6-0-on-apple-xserve-31.html
[link-5]: http://blog.toreanderson.no/2015/11/16/ipv6-network-boot-with-uefi-and-ipxe.html
[link-6]: http://blogs.technet.com/b/server-cloud/archive/2015/11/19/announcing-the-release-of-hyper-v-containers-in-windows-server-2016-technical-preview-4.aspx
[link-7]: http://rancher.com/introducing-ranchervm-package-and-run-virtual-machines-as-docker-containers/
[link-8]: http://www.virtuallyghetto.com/2015/11/migrating-esxi-to-a-distributed-virtual-switch-with-a-single-nic-running-vcenter-server.html
[link-9]: http://blogs.technet.com/b/windowsserver/archive/2015/11/16/moving-to-nano-server-the-new-deployment-option-in-windows-server-2016.aspx
[link-10]: https://github.com/vmware/photon-controller
[link-11]: https://blogs.vmware.com/cloudnative/vmware-photon-controller-deep-dive/
[link-12]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-intro
[link-13]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-prep-work-part-1
[link-14]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-define-nodes-part-2
[link-15]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-vagrant-up-part-3
[link-16]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-auto-configured-ospf-part-4
[link-17]: http://everythingshouldbevirtual.com/hey-i-can-devops-my-network-too-manual-configured-ospf-part-5
[link-18]: http://blog.kubernetes.io/2015/06/the-distributed-system-toolkit-patterns.html
[link-19]: http://sebgoa.blogspot.com/2015/05/running-vms-in-docker-containers-via.html
[link-20]: http://www.cyberciti.biz/faq/apple-mac-os-x-update-softwareupdate-bash-shell-command/
[link-21]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/11/16/why-are-there-two-hyper-v-powershell-modules-in-windows-10.aspx
[link-22]: http://www.astroarch.com/2015/11/kvm-upgrade-saga-open-vswitch-woes/
[link-23]: http://discoposse.com/2015/11/21/installing-the-openstack-cookbook-on-osx/
[link-24]: http://discoposse.com/2015/11/21/suspending-and-restarting-the-openstack-cookbook-lab/
[link-25]: http://jjasghar.github.io/blog/2015/11/18/characteristics-of-a-successful-chatroom-meeting/
[link-26]: http://www.canuck.io/posts/appacatalyst-lab-installer
[link-27]: http://galsagie.github.io/sdn/nfv/openstack/ovs/2015/11/23/ovn-l3-deepdive/
[link-28]: http://blog.bandwidth.com/why-you-should-use-yaml-as-yaml-with-ansible/
[link-29]: http://virtualviking.net/2015/12/09/500-poor-mans-vcloud-suite-lab/
[link-30]: http://www.cyberciti.biz/tips/linux-iptables-examples.html
[link-31]: http://www.networkcomputing.com/networking/network-automation-programming-and-scale/a/d-id/1323477
[link-32]: https://mesosphere.com/blog/2015/12/04/kubernetes-mesos-0-7-0/