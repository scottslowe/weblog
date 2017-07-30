---
author: slowe
categories: Information
comments: true
date: 2017-02-17T00:00:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Linux
- VMware
- NSX
- Automation
- Encryption
- Messaging
- Docker
- vRA
- Puppet
- Photon
- CentOS
- KVM
- HyperV
title: 'Technology Short Take #78'
url: /2017/02/17/technology-short-take-78/
---

Welcome to Technology Short Take #78! Here's another collection of links and articles from around the Internet discussing various data center-focused technologies.

## Networking

* The rise of the disaggregated network operating system (NOS) marches on: this time, it's Big Switch Networks [announcing expanded hardware support in Open Network Linux (ONL)][link-9], upon which its own NOS is based.
* The Pivotal Engineering blog has [an article][link-20] that shows how to use BOSH with the vSphere CPI to automate adding servers to an NSX load balancing pool.
* Mircea Ulinic has a nice article describing [the combination of NAPALM and Salt for network automation][link-21].
* Here's a post by Ron Fuller on [changing the IP address of NSX Manager][link-32].

## Servers/Hardware

Nothing this time around, sorry!

## Security

* As part of some research around my Linux migration, I came across this write-up on [how to do encrypted instant messaging on OS X with Adium and Off the Record (OTR)][link-5]. I've been using OTR with Adium for a while so this wasn't new to me, but I wanted to share it here for others who may not be familiar with the solution. (I use OTR with Adium on OS X, and OTR with [Pidgin][link-6] on my Fedora Linux laptop.)
* Along the same lines as the previous bullet, here's a write-up on [how to encrypt your e-mails with PGP keys][link-7]. Now, PGP keys have been around forever, but the cool part (IMO) about this write-up is integrating [Keybase][link-8] (and PGP keys generated in Keybase) into the process.
* Docker Inc. (the company) has recently announced they've [added secrets management into Docker Datacenter][link-13]. The New Stack has [a decent analysis][link-14] of this latest move and the potential impact it will have on the container ecosystem.
* Humair Ahmed takes a closer look at [some Cross-VC NSX security enhancements][link-19] that landed in the recent NSX 6.3 release.
* This is an older article, but I don't think I've mentioned it before now. [This article][link-24] discusses the virtual machine introspection (VMI) functionality introduced in XenServer 7 while illustrating the value of hypervisor-based security controls.

## Cloud Computing/Cloud Management

* Ryan Kelly has a whole collection of posts about the Puppet plugin for vRealize Automation (vRealize Orchestrator, actually). First, he shares [how to install and configure the Puppet plugin for vRealize Automation][link-10]. This enables you to automatically install the Puppet agent and make it active when you deploy a VM via vRealize Automation. (More information on the Puppet plugin is available [here][link-11].) Next, Ryan shares [how to integrate the Puppet plugin into vRA blueprints][link-16]. Finally, he has a couple of troubleshooting articles, one on [a potential gotcha when upgrading the plugin][link-17] and another [when getting an error on workflows][link-18]. There's lots of good information here.
* Adrian Roberts shares a walkthrough of [running Photon OS and VMware Admiral in a vCloud Air Network environment][link-12].
* How about a new [Clarity][link-27]-based interface for vRA 7.2? [Check this out.][link-28] (Note: This is _totally_ unsupported. You've been warned.)

## Operating Systems/Applications

* Daniel Allen Deutsch extolls the benefits of [learning to use tmux properly][link-1].
* Here's a piece by Carla Schroder on [third-party repositories for CentOS][link-15].
* I mentioned Flatpak (in the last Technology Short Take, I believe) recently. Here's a note mentioning that [new Firefox builds][link-25] and [the Telegram client][link-26] are available via Flatpak.
* Kudos to GitLab for [their detailed and transparent post-mortem][link-30] of the recent database outage. There are lots of lessons that can be learned from this information.

## Storage

* Anthony Spiteri [discusses an issue][link-4] he saw with his new SuperMicro-based home lab; apparently there's a problem with the native VMware driver that causes slow performance.
* Cormac Hogan has a brief update on [storage options for containers on VMware][link-29].
* Check out [Aaron Patten's article][link-31] on vRA + SPBM (storage policy-based management) with VVols.

## Virtualization

* Via one of the OpenStack mailing lists, I saw a reference this article article by CERN regarding their use of [NUMA and CPU pinning for high-throughput workloads][link-2]. While discussed in an OpenStack context, the focus of the article is more on KVM's functionality.
* Here's another virtualization-focused article written in the context of an OpenStack environment, this time focusing on [performance comparisons between Hyper-V and KVM][link-3].
* William Lam has an update on [the 7th-generation Intel NUC and ESXi 6.x][link-22]. Check it out if you're considering the latest Intel NUC for your home lab.

## Career/Soft Skills

* Communicating effectively is an important skill, and communicating effectively via e-mail is an especially important skill. Check out this HBR article on [making sure your e-mails give the right impression][link-23]. (Thanks to Jonathan Gershater for the link.)

That's all for this time, folks. Hopefully something I included here was useful to you, or made you think. Have a great weekend!



[link-1]: http://danielallendeutsch.com/blog/16-using-tmux-properly.html
[link-2]: http://openstack-in-production.blogspot.ch/2015/08/numa-and-cpu-pinning-in-high-throughput.html
[link-3]: https://blogs.msdn.microsoft.com/virtual_pc_guy/2017/02/03/hyper-v-vs-kvm-for-openstack-performance/
[link-4]: http://anthonyspiteri.net/homelab-supermicro-5020d-tnt4-storage-driver-performance-issues-and-fix/
[link-5]: https://www.calyxinstitute.org/education/howto-encrypted-instant-messaging-with-osx-adium-and-otr
[link-6]: http://pidgin.im/
[link-7]: http://journocode.com/2016/11/23/encrypt-emails-pgp-keys/
[link-8]: https://keybase.io/
[link-9]: http://www.bigswitch.com/blog/2016/11/21/open-network-linux-expansion
[link-10]: http://www.vmtocloud.com/how-to-install-and-configure-puppet-plugin-2-0-for-vrealize-automation/
[link-11]: https://docs.puppet.com/pe/latest/vro_intro.html
[link-12]: https://blogs.vmware.com/vcat/2017/01/hybrid-container-management-vcloud-director-photon-os-admiral.html
[link-13]: https://blog.docker.com/2017/02/docker-datacenter-1-13/
[link-14]: http://thenewstack.io/docker-grafts-secrets-management-swarm-says-strengthens-datacenter-platform/
[link-15]: https://www.linux.com/learn/intro-to-linux/2017/2/best-third-party-repositories-centos
[link-16]: http://www.vmtocloud.com/how-to-integrate-vrealize-automation-with-the-puppet-plugin-2-0/
[link-17]: http://www.vmtocloud.com/puppet-vro-plugin-1-0-to-2-0-upgrade-gotcha/
[link-18]: http://www.vmtocloud.com/puppet-plugin-2-0-for-vro-agent-install-workflow-gotcha/
[link-19]: https://blogs.vmware.com/networkvirtualization/2017/02/nsx-6-3-cross-vc-nsx-security-enhancements.html#.WKJA_lfiv6V
[link-20]: http://engineering.pivotal.io/post/nsx_with_bosh/
[link-21]: https://mirceaulinic.net/2016-11-17-network-orchestration-with-salt-and-napalm/
[link-22]: http://www.virtuallyghetto.com/2017/02/update-on-intel-nuc-7th-gen-kaby-lake-esxi-6-x.html
[link-23]: https://hbr.org/2017/02/how-to-make-sure-your-emails-give-the-right-impression
[link-24]: https://www.linux.com/news/virtual-machine-introspection-security-innovation-new-commercial-applications
[link-25]: https://eischmann.wordpress.com/2017/02/15/nightly-and-wayland-builds-of-firefox-for-flatpak/
[link-26]: http://www.jgrulich.cz/2016/07/08/telegram-desktop-client-for-flatpak/
[link-27]: https://vmware.github.io/clarity/
[link-28]: https://vxsan.com/vrealize-automation-7-2-clarity-interface/
[link-29]: http://cormachogan.com/2017/02/15/storage-for-containers-with-vmware-you-got-it/
[link-30]: https://about.gitlab.com/2017/02/10/postmortem-of-database-outage-of-january-31/
[link-31]: http://www.jedimt.com/2017/02/vra-spbm-vvols-awesome/
[link-32]: https://ccie5851.blogspot.com/2016/07/changing-ip-address-of-nsx-manager.html
