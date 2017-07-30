---
author: slowe
categories: Information
comments: true
date: 2012-12-06T18:20:26Z
slug: technology-short-take-27
tags:
- Automation
- CentOS
- Debian
- KVM
- Linux
- Networking
- OpenStack
- Puppet
- RedHat
- Security
- Storage
- Virtualization
- VMware
- VXLAN
- Xen
title: 'Technology Short Take #27'
url: /2012/12/06/technology-short-take-27/
wordpress_id: 2998
---

Welcome to Technology Short Take #27! This is my usual collection of links, thoughts, rants, and ideas about data center-related technologies. Here's hoping you find something useful!

## Networking

* If you're interested in learning more about OpenFlow and software-defined networking but need to do this on a shoestring budget in your home lab, a number of guides have been written to help out. I haven't personally used any of these guides yet, but I'm working my way in that direction. (I needed to fill in some other knowledge gaps first.) First up is Brent Salisbury's [how to build an SDN lab without needing OpenFlow hardware](http://networkstatic.net/2012/07/how-to-build-an-sdn-lab-without-needing-openflow-hardware/). Brent is creating some _fantastic_ content that I've found extremely useful. His earlier post on [getting started with OpenFlow and Open vSwitch tutorial lab](http://networkstatic.net/2012/06/openflow-openvswitch-lab/) is also quite good. Another good resource is Dan Hersey's guide to [building an SDN-based private cloud in an hour](http://virtualnow.net/2012/09/17/building-a-sdn-based-private-cloud-in-an-hour/). I encourage you to have a look at these posts if you're at all interested in any of these technologies.

* Bruce Davie and Martin Casado (with Nicira, now part of VMware) have written a post [comparing the VXLAN and STT tunneling protocols](http://networkheresy.com/2012/10/15/tunneling-for-network-virtualization/). Not unsurprisingly, one of the key advantages of STT that's highlighted is its improved performance due to TSO support in NIC hardware. VXLAN, on the other hand, is seeing broader adoption across multiple vendors. There's no mention of NVGRE (or just plain GRE).

* Related to the bare metal provisioning work (see below under "Servers/Hardware"), Mirantis also [detailed some bare-metal networking stuff](http://www.mirantis.com/blog/configuring-baremetal-openstack-cloud/) they've done for OpenStack in relation to the use of bare metal nodes.

## Servers/Hardware

* Mirantis published an article discussing a framework they built for [bare-metal provisioning with OpenStack](http://www.mirantis.com/blog/bare-metal-provisioning-with-openstack-cloud/) that allows OpenStack to place workloads onto bare-metal nodes instead of onto a hypervisor. It's interesting work, but unfortunately it looks like this work won't be returned to the community (it was developed for one or more of their clients). There are also a few follow-up posts, such as this one on [placement control and multi-tenancy isolation](http://www.mirantis.com/blog/baremetal-provisioning-multi-tenancy-placement-control-isolation/) and this one on [preparing images for bare metal nodes](http://www.mirantis.com/blog/baremetal-provisioning-part3-images-preparation/). Also see the "Networking" section above for a related post on the networking aspects involved.

## Security

I don't have anything for this area this time around, but I'll stay alert for articles to add next time. Feel free to share something in the comments!

## Cloud Computing/Cloud Management

* I might have mentioned this before, but [Ken Pepple's OpenStack Folsom architecture post](http://ken.pepple.info/openstack/2012/09/25/openstack-folsom-architecture/) is just awesome. It's well worth reading and reviewing in depth.

* This [OpenStack-on-Debian HOWTO](http://wiki.debian.org/OpenStackHowto) is a bit older (and probably out of date), but it does give a decent overview of the components that are involved and---via the configuration---how they relate to each other. While the details for installing a current version of OpenStack are likely to be different now, you might still find this conceptually helpful.

* These articles are a bit long in the tooth, but CSS Corp has a useful series of articles on bundling various Linux distributions for use with OpenStack: [bundling CentOS](http://cssoss.wordpress.com/2011/11/28/bundling-centos-image-for-openstack/), [bundling CentOS with VNC](http://cssoss.wordpress.com/2012/01/10/bundling-centos-image-with-vnc-access-for-openstack/), [bundling Debian](http://cssoss.wordpress.com/2011/11/28/bundling-debian-image-for-openstack/), and [bundling OpenSUSE](http://cssoss.wordpress.com/2011/11/28/bundling-opensuse-image-for-openstack/). It would be interesting to me to see how much of this, if any, could be automated with something like Puppet. If any enterprise Puppet experts want to give it a go, I'd be happy to publish a guest blog post for you with full details on how it's done.

* Much like there are some great "how to's" on how to run an SDN lab (see the Networking section earlier), there are also some great write-ups on doing the same for OpenStack. For example, Cody Bunch published [this article on running OpenStack Private Cloud on ESXi](http://openstack.prov12n.com/running-openstack-private-cloud-on-esxi/), and Brent Salisbury (there he is again!) posted an older [guide to OpenStack Essex on Ubuntu on VirtualBox](http://networkstatic.net/2012/04/23/openstack-essex-scripted-installation-on-ubuntu-12-04-part-1/) as well as a newer [guide to OpenStack DevStack on Fusion](http://networkstatic.net/openstack-quantum-devstack-on-a-laptop-with-vmware-fusion/).

## Operating Systems/Applications

* I found this article on [imperative vs. declarative system configuration](http://spin.atomicobject.com/2012/09/13/from-imperative-to-declarative-system-configuration-with-puppet/) is quite helpful in understanding Puppet's declarative model. If you're trying to become more proficient with Puppet, as I am, then this might be worth reviewing.

* This three-part series on Puppet concepts for beginners ([part 1](http://justfewtuts.blogspot.com/2012/05/puppet-beginners-concept-guide-part-1.html), [part 2](http://justfewtuts.blogspot.com/2012/07/puppet-beginners-concept-guide-part-2.html), and [part 3](http://justfewtuts.blogspot.com/2012/08/puppet-beginners-concept-guide-part-3.html)) is also quite helpful in solidifying concepts and terminology.

* [This VMware blog post](http://blogs.vmware.com/vfabric/2012/09/why-application-director-puppet-work-better-together.html) helps explain the link between Puppet and vFabric Application Director, and why organizations may want to use both.

* Here's [a quick guide to installing Puppet on CentOS 5](http://www.how2centos.com/centos-5-puppet-install/), as well as some sample code for a few simple tasks.

## Storage

* I don't fully understand all the details involved, but this post on [changes in block protocol scalability in Xen](http://blog.xen.org/index.php/2012/11/23/improving-block-protocol-scalability-with-persistent-grants/) outlines what sounds like good progress in improving efficiency.

* [This article](http://servicesangle.com/blog/2012/10/03/qlogics-frees-ssds-from-server-slavery-with-mt-rainier/) is a bit older, published at the start of October, but it talks about an interesting project (product?) by Qlogic called "Mt. Rainier." (Stu Miniman of Wikibon has [more information here](http://wikibon.org/wiki/v/QLogic_Unveils_a_Networking_Approach_to_Enhance_Flash_Storage) as well.) Apparently, "Mt. Rainier" will allow customers to combine PCIe-based SSD storage inside servers into a "virtual SAN" (now there's an original and not over-used term). The _really_ interesting aspect, in my opinion, is the use of "Mt. Rainier" to create shared caches across servers. Is this the beginning of [the data center fractal edge](http://blogs.cisco.com/datacenter/introducing-engineers-unplugged-video-podcast-for-technical-people-with-a-side-of-unicorns/)?

## Virtualization

* Big news in the QEMU world: In the QEMU 1.3 release, [the QEMU-KVM and QEMU projects have been merged](http://www.linux-kvm.com/content/qemu-13-released-qemu-kvm-merge-qemu-complete). Why is this important? It's first necessary to understand the relationship between QEMU and KVM. KVM is the set of kernel modules that leverage hardware virtualization functionality inside Intel and AMD CPUs, and it makes possible the virtualization of closed-source operating systems like Windows. QEMU, on the other hand, is needed to emulate everything else that a VM needs: networking, storage, USB, keyboard, mouse, etc. Both KVM and QEMU are needed for a full virtualization solution. Until the 1.3 release, QEMU (without hardware acceleration via KVM) was one branch, and QEMU-KVM (with KVM hardware acceleration) was a separate branch. The QEMU 1.3 release completes an effort to merge both efforts into a single development tree.

* The merge of QEMU and QEMU-KVM isn't the only cool thing happening with QEMU; also included in the 1.3 release is [GlusterFS integration](http://raobharata.wordpress.com/2012/10/29/qemu-glusterfs-native-integration/). This integration dramatically improves GlusterFS performance by allowing QEMU's block layer to communicate directly with the Gluster backend without going through the userspace FUSE components.

* Erik Scholten of VMGuru.nl has posted a good [hypervisor feature comparison](http://www.vmguru.nl/wordpress/2012/12/enterprise-hypervisor-feature-comparison/) document. It includes RHEV 3.1 in the comparison, even though RHEV 3.1 wasn't released (was still in beta) at the time the comparison was written.

* Speaking of RHEV: apparently RHEV 3.1 was released yesterday (Wednesday, December 4, 2012), although I haven't been able to find any sort of official press release or announcement.

* Debunking an argument I've heard quite a bit is this article by Frank Denneman on [using SIOC with multiple datastores backed by a single pool of disks](http://frankdenneman.nl/sioc/sioc-on-datastores-backed-by-a-single-datapool/).

* Need to compact a virtual hard disk in Windows 8/Windows Server 2012? Ben Armstrong shows how [here](http://blogs.msdn.com/b/virtual_pc_guy/archive/2012/10/10/compacting-virtual-hard-disks-in-windows-8-windows-server-2012.aspx).

* I enjoyed this article by Josh Townsend on [using SUSE Studio and HAProxy to create a (free) open source load balancing solution for VMware View](http://vmtoday.com/2012/09/free-vmware-view-load-balancer-using-suse-studio-and-haproxy/).

That's it for this time around; no need to overwhelm you with too much information! Besides, I have to keep a few items around for Technology Short Take #28

As always, comments, thoughts, rants, or corrections are welcome below.
