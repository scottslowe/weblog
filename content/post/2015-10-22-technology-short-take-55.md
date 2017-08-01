---
author: slowe
categories: Information
comments: true
date: 2015-10-22T09:30:00Z
tags:
- Networking
- Hardware
- Security
- Storage
- Virtualization
- Ansible
- OpenStack
- Docker
- EMC
- VMware
- HyperV
- Productivity
title: 'Technology Short Take #55'
url: /2015/10/22/technology-short-take-55/
---

Welcome to Technology Short Take #55! Here's hoping I've managed to find something of value and interest to you in this latest collection of links and articles from around the web on networking, storage, virtualization, security, and other data center-related technologies. Enjoy!

## Networking

* I recently came across Kuryr, an OpenStack project aimed at connecting Docker's libnetwork efforts to OpenStack Neutron. The end result, as I understand it, would be to allow _any_ Neutron plugin to be able to provide container networking functionality to Docker via libnetwork. This makes sense to me, although I think that network virtualization products are still going to need to integrate directly with libnetwork so that they can be used in environments outside of OpenStack. If you're interested in getting more information on Kuryr, check out Gal Sagie's post [here][link-1] or read this follow-up post on [using Kuryr and OVN][link-27] (Open Virtual Network) together.
* Drew Conry-Murray has a post up on the Packet Pushers blog talking about [the benefits and challenges of a single OS][link-2]; specifically, the benefits and challenges pertaining to Arista and EOS. Lots of companies like to tout the "single OS" banner, but there can be value in having specialized OSes custom-built for specific purposes.
* Here's an article that combines two of my favorite companies: Cumulus Networks and Ravello Systems. How so? The article shows you [how to build a Cumulus VX lab on Ravello Systems][link-5]. Very nifty stuff.
*  CloudBase Solutions recently [announced the beta availability of Open vSwitch (OVS) 2.4 on Hyper-V][link-7]---bringing VXLAN and STT support to Hyper-V and enabling interoperable tunneling between Hyper-V hosts and KVM hosts running OVS. A [follow-up blog post][link-20] talks about how to use OVS on Hyper-V outside of an OpenStack context.
* Lim Wei Chiang has a nice write-up on [using ERSPAN with the vSphere Distributed Switch to do packet analysis][link-15].
* Anthony Burke has an article on [a method for performing ingress optimization with NSX for vSphere][link-19] that leverages information from the hypervisor to help NSX make smarter routing decisions/updates. This is pretty cool and underscores the power of pooling data from compute, storage, and networking in a software-defined data center environment.
* Matt Oswalt is one of a number of forward-thinking networking pros who's helping to lead the charge in transforming what it means to be a "networking professional." In [this post on network automation][link-23], he encourages folks to "be bold" and really pursue network automation instead of "settling" for templating configurations.
* If you're running some Juniper equipment and are interested in getting started with network automation on that equipment, this article by Jason Edelman on [Juniper vSRX automation with Ansible][link-24] is a good resource.
 
## Servers/Hardware

* Dell is buying EMC. What---you hadn't heard? There are _tons_ of articles discussing the acquisition; check out this pair of articles from Chris Evans ([part 1][link-37], [part 2][link-38]).

## Security

* I'm no cryptographer, but I found [this article][link-30] on the NSA's move away from Suite B an interesting read. The author, Matthew Green, also has an [in-depth critique of PGP][link-31] that took up a fair amount of my time (be sure to read the comments).

## Cloud Computing/Cloud Management

* VMware recently announced some integrations between Ansible and vCloud Air that will allow customers to use Ansible playbooks to create resources in vCloud Air. See [this blog post][link-3] for more details and some example Ansible code.
* Mark Voelker has two posts (so far) on OpenStack DefCore misconceptions that I've personally found quite helpful. The [first post][link-8] explains the 12 criteria that are evaluated to determine whether a capability should be required in its guidelines. As Mark explains, these criteria are mostly trailing indicators, meaning they mostly reflect what is _already_ happening with the adoption of OpenStack features/capabilities. The [second post][link-9] explains the "advisory" status of a capability in a guideline, and how this status is intended to help trigger discussion and feedback around whether that capability should move to required, or dropped from the guideline entirely.
* This post on [hacking Neutron to add more IP addresses to a subnet][link-12] is useful for helping to understand some of the Neutron internals.
* Rancher---the company behind RancherOS (Linux distro that uses Docker for system services as well as user applications) and Rancher (a management platform for containerized infrastructure)---recently [announced a container metadata service][link-25] they're saying is analogous to Amazon's Instance Metadata service. I like the idea, but I'd like it even more if Rancher (the company) pushed this beyond only Rancher's products.
* Speaking of metadata services, have a look at [this comparison of metadata services][link-33] (which compares DigitalOcean, Amazon, and Google, but _not_ OpenStack). (Warning: the author's site seemed a bit flakey the last time I visited it to verify the link, so be warned.)
* Juan Manuel Rey has a write-up on [how easy it is to upgrade VIO][link-34] (VMware Integrated OpenStack).

## Operating Systems/Applications

* Recently I saw mention of [ScyllaDB][link-10], a drop-in replacement for Cassandra that claims to be 10x faster. It might be worth investigating if your organization uses Cassandra and is having a hard time with performance and scaling.
* Juan Manuel Rey has a write-up on [taking your first steps with Pivotal's Lattice][link-21]. I believe that platforms like [Lattice][link-22] are a rich opportunity for infrastructure folks who are seeking to "move up the stack" a bit, and so I'm glad to see resources such as this article emerging.
* I'm assuming everyone saw the news that [Red Hat is acquiring Ansible][link-26].

## Storage

* I recently posted [an article][xref-1] about LVM (it was more for me than anything else; I so very rarely run those commands that it's hard to remember them), but for those who want to automate everything, here's an article that describes [how to work with LVM using Ansible][link-4].
* Want to play with VSAN, but don't have the requisite equipment? No worries, [Ravello Systems has you covered][link-16]. This isn't going to cut it for a production environment, but as the title says it's handy for proof-of-concept, testing, and self-development.
* Peter Keilty (a former vSpecialist teammate now at VMware) has [a write-up on options for using data-at-rest encryption with VMware VSAN][link-32]. If data-at-rest encryption is something you're exploring, this post might be worth a few minutes of your time.

## Virtualization

* William Lam points out a new feature in the vSphere Web Client as of vSphere 6.0 Update 1: the ability to erase existing disk partitions. Read [his post][link-6] for all the gory details.
* [This][link-11] is an interesting experiment, although I confess that I don't see the value in this process. Feel free to drop me an e-mail or contact me via Twitter if you can enlighten me.
* If you weren't at VMworld 2015 and/or haven't had the chance to catch up on vSphere Integrated Containers, formerly "Project Bonneville," see [this VMware blog post for more information][link-13].
* From the Department of No Surprises comes [this study][link-14] that shows virtualization offers dramatic cost savings and helps reduce energy consumption. (I applaud Experts Exchange for help to spread the word about the benefits of virtualization, even if they are a few years behind the times.)
* VMware recently released a "standalone" version of VMware Tools (version 10.0, see [here][link-17] for the announcement), but as [Andreas Peetz points out][link-18] there are still some issues that VMware needs to fix around VMware Tools.
* Looks like [Microsoft is jumping in the nested virtualization ring][link-35]. Microsoft is also catching up on the ability to create [VMs with different versions][link-36] (think Virtual HW versions from the vSphere world, if that's your background).

## Career/Soft Skills/Productivity

* Chris Wahl posted [a brief discussion][link-28] on his use of [the Pomodoro technique][link-29] as a way of enhancing his learning efforts.

OK, I'd better stop before this gets any longer! Whew...so much good stuff out there, it's really hard to choose what's included and what's omitted. In any case, I hope something here was helpful to you. Thanks for reading!



[link-1]: https://galsagie.github.io/sdn/openstack/docker/kuryr/neutron/2015/08/24/kuryr-part1/
[link-2]: http://packetpushers.net/arista-eos-benefits-challenges/
[link-3]: http://blogs.vmware.com/vcloud/2015/09/ansible-and-vcloud-air-better-together.html
[link-4]: http://everythingshouldbevirtual.com/ansible-playbook-lvm
[link-5]: https://www.edge-cloud.net/2015/08/building-a-cumulus-networks-vx-cloud-lab-with-ravello-systems/
[link-6]: http://www.virtuallyghetto.com/2015/09/erasing-existing-disk-partitions-now-available-in-the-vsphere-web-client-vsphere-6-0-update-1.html
[link-7]: http://www.cloudbase.it/open-vswitch-24-on-hyperv-part-1/
[link-8]: http://markvoelker.github.io/blog/defcore-misconceptions-1/
[link-9]: http://markvoelker.github.io/blog/defcore-misconceptions-2/
[link-10]: http://www.scylladb.com
[link-11]: http://mr.gy/blog/build-vm-image-with-docker.html
[link-12]: http://cloudblog.switch.ch/2015/09/22/hack-neutron-to-add-more-ip-addresses-to-an-existing-subnet/
[link-13]: https://blogs.vmware.com/vsphere/2015/10/vsphere-integrated-containers-technology-walkthrough.html
[link-14]: http://pages.experts-exchange.com/Impact-of-Virtualization.html
[link-15]: http://kacangisnuts.com/2013/12/vsphere-distributed-virtual-switch-packet-analysis-using-erspan/
[link-16]: https://www.ravellosystems.com/blog/vsan-installation-on-aws-google-cloud/
[link-17]: https://blogs.vmware.com/vsphere/2015/09/vmware-tools-10-0-0-released.html
[link-18]: http://www.v-front.de/2015/10/the-great-vmware-tools-dilemma.html
[link-19]: http://networkinferno.net/ingress-optimisation-with-nsx-for-vsphere
[link-20]: http://www.cloudbase.it/open-vswitch-24-on-hyperv-part-2/
[link-21]: https://jreypo.wordpress.com/2015/10/02/first-steps-with-pivotal-lattice/
[link-22]: http://lattice.cf/
[link-23]: http://keepingitclassless.net/2015/09/network-automation-be-bold/
[link-24]: http://jedelman.com/home/juniper-vsrx-automation-with-ansible/
[link-25]: http://rancher.com/introducing-rancher-metadata-service-for-docker/
[link-26]: http://www.redhat.com/en/about/press-releases/red-hat-acquire-it-automation-and-devops-leader-ansible
[link-27]: http://galsagie.github.io/sdn/openstack/docker/kuryr/neutron/2015/10/10/kuryr-ovn/
[link-28]: http://wahlnetwork.com/2015/10/22/picking-up-new-skills-tips-and-tricks-to-build-your-technical-tool-chest/
[link-29]: https://en.wikipedia.org/wiki/Pomodoro_Technique
[link-30]: http://blog.cryptographyengineering.com/2015/10/a-riddle-wrapped-in-curve.html
[link-31]: http://blog.cryptographyengineering.com/2014/08/whats-matter-with-pgp.html
[link-32]: http://livevirtually.net/2015/10/21/virtual-san-and-data-at-rest-encryption/
[link-33]: https://ahmetalpbalkan.com/blog/comparison-of-instance-metadata-services/
[link-34]: https://jreypo.wordpress.com/2015/10/21/upgrading-vmware-integrated-openstack/
[link-35]: http://blogs.technet.com/b/virtualization/archive/2015/10/13/windows-insider-preview-nested-virtualization.aspx
[link-36]: http://blogs.msdn.com/b/virtual_pc_guy/archive/2015/10/20/windows-10-build-10565-creating-vms-with-different-versions.aspx
[link-37]: http://blog.architecting.it/2015/10/16/dell-acquiring-emc-the-facts/
[link-38]: http://blog.architecting.it/2015/10/22/dell-acquiring-emc-the-federation/
[xref-1]: {{< relref "2015-09-24-quick-reference-new-storage-lvm.md" >}}
