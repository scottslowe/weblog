---
author: slowe
categories: Information
comments: true
date: 2016-06-03T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- VMware
- NSX
- OpenStack
- Windows
- Docker
- vSphere
- macOS
- Linux
- VSAN
- HyperV
- Cisco
title: 'Technology Short Take #67'
url: /2016/06/03/technology-short-take-67/
---

Welcome to Technology Short Take #67. Here's hoping something I've collected for you here proves useful!

## Networking

* Anthony Burke has written a script that uses VMware NSX to protect VMware Log Insight instances. More information on the script is in [his blog post][link-1].
* Russ White [tackles][link-2] the issue of networking engineers needing to learn to code. Is it necessary? Russ thinks so---but probably not for the reasons you might think. I tend to agree with Russ' line of thinking.
* [This article][link-8] from Marcos Hernandez shows one way to do dynamic routing in OpenStack. It's a bit of a hack, to be honest, but it gets the job done until dynamic routing makes its way into OpenStack Neutron (which looks like it may have landed in the Mitaka release---can anyone confirm?).
* Jason Messer has an article describing [how networking works with Windows containers][link-14].
* Tom Hollingsworth [discusses][link-24] how the rise of overlay networks killed large layer 2 networks and tools for building large layer 2 networks, like TRILL.
* Dmitri Kalintsev examines some options for [addressing storage-related connectivity in NSX environments][link-25].

## Servers/Hardware

* Drew Conry-Murray shines a light on [Intel's network ambitions][link-22]. This is something I've been watching for a couple of years, since attending my first Intel Developer Forum (IDF). Intel clearly has its sights set on expanding beyond just "servers" into many more platforms, including network hardware platforms.

## Security

* AlgoSec has a couple of articles published last year before VMworld 2015, but I just now came across them. The first article provides [a blueprint for migrating applications to VMware NSX][link-17]; the second article supplies [some tips for creating filtering policies in VMware NSX][link-18].

## Cloud Computing/Cloud Management

* Ajeet Singh Raina has a write-up on [running Docker Datacenter on VMware vSphere][link-11].
* Following the recent OpenStack Summit in Austin, Stephen O'Grady posted this article on [OpenStack and the fragmenting infrastructure market][link-13]. It's worth a read, especially if you're trying to get a bit of a grasp on how things are evolving with regard to containers and OpenStack.
* I recently came across these articles on [running AWS CLI in a Docker container][link-19] and [running S3cmd in a Docker container][link-20]. Personally---and this is just my view---running stuff in Docker containers makes sense in many cases. I'm not yet convinced that running something like the AWS CLI in a Docker container is worth the extra effort (a Python virtual environment may be more appropriate here).
* Matt Dorn has a good article on [configuring cloud-init to use an admin password][link-21] in an OpenStack environment.
* William Lam explains [how to override the default CPU/RAM settings for the Photon Controller management VM][link-23].
* Midokura recently issued [a press release][link-30] touting their place as the "#1 third-party Neutron plugin for OpenStack clouds with 1000+ cores". This statement is taken from [the most recent OpenStack user survey][link-31], which---if you read carefully---will show that the sample size for that particular statistic was 44 responses, and that Midokura (5%) was only _barely_ ahead of Cisco UCS/Nexus (3%), Juniper (4%), and NSX (4%).

## Operating Systems/Applications

* Someone recently pointed [this out][link-5]. It looks like container-optimized OSes designed to run in VMs on a hypervisor are becoming all the rage these days. (Maybe VMware's Photon OS wasn't such a crazy idea after all?)
* OS X users might find [this Apple support article][link-7] on OS X wireless roaming to be helpful.
* You may have heard about Microsoft's announcement regarding running Bash on Windows; here's [an article][link-9] written by Deepu Thomas with more details on the Windows Subsystem for Linux (WSL), which makes running native Linux ELF64 binaries on Windows possible. (By the way, I alluded to this sort of architecture in December 2006 while discussing the future of the operating system; see [this blog post][xref-1].)
* I mentioned this on Twitter, but wanted to include it here as well. Read [this article][link-12] by Mary Rose Cook for an excellent review of how Git works.
* I believe I've mentioned the change in Docker 1.11, where Docker Engine is now built on runC (the container runtime) and containerd (a daemon controlling runC). Tiffany Jernigan has [a good post outlining][link-16] the changes and how it all ties together.

## Storage

* John Nicholson talks about [the Virtual SAN (VSAN) performance service][link-10], available in VSAN 6.2.

## Virtualization

* Usually, articles talking about nested virtualization use ESXi as the "base hypervisor" and are running KVM, Xen, or Hyper-V as the nested hypervisor. What about flipping that on its head, and running ESXi as a nested hypervisor inside Hyper-V? Daniel Scott Raynsford has [an article][link-3] showing exactly that: ESXi running nested on Hyper-V.
* And while I am on the topic of nested virtualization: here's an article from William Lam on [running OS X as a VM on a nested ESXi instance running under VMware Fusion][link-4].
* This nixCraft article shows [how to assign static IP addresses to KVM-based VMs][link-6] using `dnsmasq` managed by Libvirt.
* XenServer 7, aka "Dundee," was released recently. The features and capabilities of this new release are described in [this blog post][link-26].
* Seems like everybody and their brother is launching "container as VM" projects; the latest that I've seen is [HyperContainer][link-28] (which, AFAICT, is _not_ the same as [Hyper][link-27]). Information on how HyperContainer integrates with Kubernetes to create---you guessed it---"Hypernetes" is found [here][link-29]. I guess the hypervisor makes a really good isolation tool after all.

## Career/Soft Skills

* I recently had the pleasure of participating in a panel discussion on the future of the data center while at Interop 2016 in Las Vegas. One of my fellow panelists, Sonia Cuff, summarized some of the thoughts and statements from the panel discussion in [this blog post][link-15].

That's all, folks...until next time, at least!

[link-1]: http://networkinferno.net/powernsx-log-insight-segmenter
[link-2]: http://ntwrk.guru/need-learn-code-no-not-think/
[link-3]: https://dscottraynsford.wordpress.com/2016/04/22/install-a-vmware-esxi-6-0-hypervisor-in-a-hyper-v-vm/
[link-4]: http://www.virtuallyghetto.com/2014/08/how-to-run-nested-mac-os-x-guest-on-nested-esxi-on-top-vmware-fusion.html
[link-5]: https://cloud.google.com/compute/docs/containers/vm-image/
[link-6]: http://www.cyberciti.biz/faq/linux-kvm-libvirt-dnsmasq-dhcp-static-ip-address-configuration-for-guest-os/
[link-7]: https://support.apple.com/en-us/HT206207
[link-8]: http://blogs.vmware.com/openstack/dynamic-routing-openstack/
[link-9]: https://blogs.msdn.microsoft.com/wsl/2016/04/22/windows-subsystem-for-linux-overview/
[link-10]: http://thenicholson.com/virtual-san-performance-service/
[link-11]: http://collabnix.com/archives/1149
[link-12]: https://codewords.recurse.com/issues/two/git-from-the-inside-out
[link-13]: http://redmonk.com/sogrady/2016/04/29/openstack-fragmentation/
[link-14]: https://blogs.technet.microsoft.com/virtualization/2016/05/05/windows-container-networking/
[link-15]: http://24x7itconnection.com/2016/05/18/debating-future-data-center/
[link-16]: https://medium.com/@tiffanyfayj/docker-1-11-et-plus-engine-is-now-built-on-runc-and-containerd-a6d06d7e80ef#.19apxdd42
[link-17]: http://blog.algosec.com/2015/08/a-blueprint-for-migrating-applications-to-vmware-nsx.html
[link-18]: http://blog.algosec.com/2015/08/tips-on-how-to-create-filtering-policies-for-vmware-nsx.html
[link-19]: https://blog.flowlog-stats.com/2016/05/03/aws-cli-in-a-docker-container/
[link-20]: https://blog.flowlog-stats.com/2016/05/03/s3cmd-in-a-docker-container/
[link-21]: http://www.madorn.com/cloud-init-admin-pass.html
[link-22]: http://packetpushers.net/intels-network-ambitions/
[link-23]: http://www.virtuallyghetto.com/2016/04/how-to-override-the-default-cpumemory-when-deploying-photon-controller-management-vm.html
[link-24]: https://networkingnerd.net/2016/05/11/the-death-of-trill/
[link-25]: https://telecomoccasionally.wordpress.com/2016/05/04/serving-bandwidth-hungry-vms-with-dc-fabrics-and-nsx-for-vsphere/
[link-26]: http://xenserver.org/blog.html?view=entry&id=118
[link-27]: https://www.hyper.sh/
[link-28]: http://hypercontainer.io/
[link-29]: http://blog.kubernetes.io/2016/05/hypernetes-security-and-multi-tenancy-in-kubernetes.html
[link-30]: http://www.midokura.com/press-releases/midokura-third-party-network-driver-large-scale-openstack-clouds-production/
[link-31]: http://www.openstack.org/assets/survey/April-2016-User-Survey-Report.pdf
[xref-1]: {{< relref "2006-12-05-the-future-of-the-os.md" >}}
