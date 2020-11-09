---
author: slowe
categories: Information
comments: true
date: 2016-04-22T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- OpenStack
- NSX
- vSphere
- Kubernetes
- Photon
- Linux
- HyperV
- Docker
- Puppet
- Ubuntu
- Apple
- VMware
title: 'Technology Short Take #65'
url: /2016/04/22/technology-short-take-65/
---

Welcome to Technology Short Take #65! As usual, I gathered an odd collection of links and articles from around the web on key data center technologies and trends. I hope you find something useful!

## Networking

* Michael Ryom has [a nice (but short) article][link-1] on using Log Insight along with a NetFlow proxy to help provide more detailed visibility into traffic flows between VMs on NSX logical networks.
* Brent Salisbury has [an article on GoBGP][link-7], a Go-based BGP implementation. BGP seems to be emerging as an early front-runner for a standards-based control plane for software networking. Couple something like GoBGP with IPVLAN L3 (see [Brent's article][link-8]) and you've got a new model for your data center network.
* Andy Hill has an article on [doing rolling F5 upgrades using Ansible][link-12].
* Filip Verloy has an article that discusses [the integration between Nuage Networks and Fortinet][link-26].
* This should probably go in the "Cloud Computing/Cloud Management" section, but the boundaries between areas are getting more and more blurry every day. (Thankfully, due to LASIK my vision is sharper than ever.) In any case, here's a post by Marcos Hernandex on [the use of subnet pools in OpenStack][link-28]. Although Marcos' post discusses them in conjunction with NSX, they're really a Neutron feature and should work with just about any Neutron plugin. In any case, I can see some very useful cases for subnet pools, particularly in conjunction with tenant networks that use "routable" IPs and don't use source NAT on the logical router(s).

## Servers/Hardware

Nothing this time around. Maybe next time!

## Security

* Bruce Schneier [asks the question][link-13] that society has yet to answer (and may be afraid to answer): "...do we prioritize security over surveillance, or do we sacrifice security for surveillance?"
* Mike Foley recently published a two-part series on two factor authentication (2FA) for vSphere ([part 1 is here][link-18]; [part 2 is here][link-19]). Now you have no excuses for not using 2FA with your vSphere environment.

## Cloud Computing/Cloud Management

* VMware's Cloud-Native Apps group recently released v0.8 of Photon Controller, and along with that comes [a BOSH CPI for Photon Controller][link-10]. If you're interested in taking Photon Controller for a spin, William Lam is in the midst of a series on Photon Controller; go have a look at [part 1][link-16], [part 2][link-17], and [part 3][link-25] (that's all he's written so far; more are planned).
* And while we are still on the topic of Photon Controller...Kris Thieler has a nice article on [how to reduce the resource requirements][link-24] needed to run Kubernetes on Photon Platform.
* There's been a fair amount of noise recently about running OpenStack on Kubernetes ([here's one example][link-21]). If I'm understanding it correctly, it's just running the OpenStack management components (API servers, message queues, databases, etc.) on Kubernetes, which does make a fair amount of sense. If it's something more than that...well, I'd need to ponder that a bit to make sure I grok it.

## Operating Systems/Applications

* Although it's not feature-complete (by a long shot), VMware recently open-sourced version 0.1 of vSphere Integrated Containers (VIC). Björn Bundert has a brief article on [installing VIC 0.1 via VMware Photon OS TP2][link-2].
* [An Ubuntu userspace and Bash shell running on Windows][link-3], eh? My my, how the world has changed.
* And for further evidence that the Windows world is drastically changing, note [this article][link-4] by Taylor Brown announcing Hyper-V Containers on Windows 10 and PowerShell for Docker. This _almost_ entices me to try Windows again. Almost---but not quite.
* Oh, speaking of PowerShell (well, sort of), Luc Dekens has [an update on the `Get-EsxCLI` cmdlet][link-32]. If this is one you use in your scripts, you may want to read Luc's article.
* It looks like there may be some "gotchas" when it comes to using user namespaces with Docker and also needing access to the Docker socket. Phil Estes explores the problem in [this post][link-9].
* I haven't messed with Puppet for a while, but here's a good post by Gareth Rushgrove on [using Puppet with CoreOS][link-14]. It's worth a read if these two technologies fall into your wheelhouse.
* Want to run Docker Swarm with IPv6? Shannon McFarland has you covered in [this post][link-22].
* One of the potential drawbacks of the "single process per container" model that Docker advocates is that it can have some unintended side effects. [This Yelp Engineering blog post][link-23] talks about one of these unintended side effects (processes running as PID 1 are treated differently by the Linux kernel). The blog post also discusses a potential fix for the issue, in the form of the "dumb-init" process.
* Ubuntu 16.04 is out (get it [here][link-29]), marking the first LTS release from Canonical that leverages systemd as its init system. Interestingly, despite a convergence on systemd across the major Linux distributions (RHEL/CentOS and Debian were already on systemd), I'm finding some noticeable differences in the systemd implementations. The more things change, the more they stay the same.

## Storage

* Here's an article I found interesting. GitHub recently changed their storage architecture, using a Git-based solution called DGit. More details are available in [this GitHub Engineering blog post][link-11].
* A reader pointed me to this article he wrote on [NFS v4.1 multipathing with KVM][link-20]. It's still pretty raw (the technology, not the article)---you have to patch and compile the kernel yourself---but it's an interesting look into where things are headed. As I told the reader/author (Martin Houry), I'm looking forward to seeing this land in the mainstream kernel!

## Virtualization

* This one isn't quite virtualization, but isn't quite hardware either, so we'll throw it in here. A conversation with a very talented engineer at VMware drew me to [this post on EVO SDDC workload domains][link-27], published by Jason Lochhead.
* Frank Denneman has a nice post on [using an OS X system to manage your vSphere environment][link-30]. There are some good tips in here. Psst, hey Frank: "Mac" is the hardware from Apple, "MAC" is an acronym that stands for Media Access Control (in networking circles, as in "MAC address") or "Mandatory Access Control" (in security circles). Just sayin'.
* Jason Boche walks you through [the steps needed to recover your VCSA 6 appliance][link-31] in the event an `fsck` (file system check) fails.

## Career/Soft Skills

* Somewhat (sort of) related to [my recent rant on lock-in][xref-1], [here's Walt Mossberg weighing in][link-5] on the (to borrow [a phrase from Massimo][link-6]) the "incestuous" relationship(s) between open and closed software in today's technology world. TL;DR: It's not as simple as it may seem at times, and there are often more important things on which technologists should be spending their time.
* If you're considering the AWS Certified Solution Architect Associate certification (I'm exploring it, trying to decide), [here's a write-up by Alex Galbraith][link-15] on his recent exam prep and exam experience.

It's time to wrap up now, or I'll never get this thing published. Happy Friday everyone!



[link-1]: https://michaelryom.dk/log-insight-netflow-awesome/#.VvsWNV9OLCQ
[link-2]: http://blog.think-v.com/?p=3649
[link-3]: http://blog.dustinkirkland.com/2016/03/ubuntu-on-windows.html
[link-4]: https://blogs.technet.microsoft.com/virtualization/2016/04/01/build-2016-container-announcements-hyper-v-containers-and-windows-10-and-powershell-for-docker/
[link-5]: http://www.theverge.com/2016/3/16/11242266/walt-mossberg-open-vs-closed-software-apple-os-x-google-android
[link-6]: http://www.it20.info/2016/03/the-incestuous-relations-among-containers-orchestration-tools/
[link-7]: http://networkstatic.net/gobgp-control-plane-evolving-software-networking/
[link-8]: http://networkstatic.net/configuring-macvlan-ipvlan-linux-networking/
[link-9]: https://integratedcode.us/2016/04/08/user-namespaces-sharing-the-docker-unix-socket/
[link-10]: http://blogs.vmware.com/cloudnative/photon-platform-bosh-cpi/
[link-11]: http://githubengineering.com/introducing-dgit/
[link-12]: https://virtualandy.wordpress.com/2016/03/31/f5-rolling-deployments-with-ansible/
[link-13]: https://www.schneier.com/blog/archives/2016/03/lawful_hacking_.html
[link-14]: https://puppet.com/blog/using-puppet-coreos-rkt-flannel-and-etcd
[link-15]: http://tekhead.it/blog/2016/03/aws-certified-solution-architect-associate-exam-prep-experience/
[link-16]: http://www.virtuallyghetto.com/2016/04/test-driving-vmware-photon-controller-part-1-installation.html
[link-17]: http://www.virtuallyghetto.com/2016/04/test-driving-vmware-photon-controller-part-2-deploying-first-vm.html
[link-18]: http://www.yelof.com/2016/04/01/two-factor-authentication-for-vsphere-rsa-securid/
[link-19]: http://www.yelof.com/2016/04/01/two-factor-authentication-for-vsphere-rsa-securid-part-2/
[link-20]: http://packetpushers.net/multipathing-nfs4-1-kvm/
[link-21]: https://tectonic.com/blog/openstack-and-kubernetes-come-together.html
[link-22]: http://www.debug-all.com/?p=148
[link-23]: http://engineeringblog.yelp.com/2016/01/dumb-init-an-init-for-docker.html
[link-24]: https://blog.inkysea.com/2016/04/20/less-than-16gb-k8-photon-controller-no-problem/
[link-25]: http://www.virtuallyghetto.com/2016/04/test-driving-vmware-photon-controller-part-3a-deploying-kubernetes.html
[link-26]: https://filipv.net/2016/04/17/fortinet-integration-with-nuage-networks-sdn/
[link-27]: https://blogs.vmware.com/virtualblocks/2015/12/15/evo-sddc-workload-domains/
[link-28]: http://blogs.vmware.com/openstack/neutron-subnet-pools/
[link-29]: http://releases.ubuntu.com/xenial/
[link-30]: http://frankdenneman.nl/2016/04/21/managing-your-virtual-datacenter-and-home-lab-with-a-mac/
[link-31]: http://www.boche.net/blog/index.php/2016/04/04/vcenter-server-6-appliance-fsck-failed/
[link-32]: http://www.lucd.info/2016/04/22/closer-look-get-esxcli-v2/
[xref-1]: {{< relref "2016-03-17-on-topic-of-lock-in.md" >}}
