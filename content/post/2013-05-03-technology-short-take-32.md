---
author: slowe
categories: Information
comments: true
date: 2013-05-03T09:20:38Z
slug: technology-short-take-32
tags:
- Hardware
- iSCSI
- KVM
- Networking
- OpenFlow
- OpenStack
- RedHat
- SDN
- Storage
- Virtualization
- VMware
- vSphere
- VXLAN
- Security
title: 'Technology Short Take #32'
url: /2013/05/03/technology-short-take-32/
wordpress_id: 3171
---

Welcome to Technology Short Take #32, the latest installment in my irregularly-published series of link collections, thoughts, rants, raves, and miscellaneous information. I try to keep the information linked to data center technologies like networking, storage, virtualization, and the like, but occasionally other items slip through. I hope you find something useful.

## Networking

* Ranga Maddipudi ([@vCloudNetSec](https://twitter.com/vCloudNetSec) on Twitter) has put together two blog posts on vCloud Networking and Security's App Firewall ([part 1](http://blogs.vmware.com/vsphere/2013/04/vcloud-networking-and-security-5-1-app-firewall-part-1.html) and [part 2](http://blogs.vmware.com/vsphere/2013/04/vcloud-networking-and-security-5-1-app-firewall-part-2.html)). These two posts are detailed, hands-on, step-by-step guides to using the vCNS App firewall---good stuff if you aren't familiar with the product or haven't had the opportunity to really use it.

* The sentiment behind this post isn't unique to networking (or networking engineers), but that was the original audience so I'm including it in this section. Nick Buraglio climbs on [his SDN soapbox](http://www.forwardingplane.net/2013/03/my-sdn-soapbox-now-with-ipv6/) to tell networking professionals that changes in the technology field are part of life---but then provides some specific examples of how this has happened in the past. I particularly appreciated the latter part, as it helps people relate to the fact that they _have_ undergone notable technology transitions in the past but probably just don't realize it. As I said, this doesn't just apply to networking folks, but to everyone in IT. Good post, Nick.

* Some good advice here on [scaling/sizing VXLAN](http://blog.ioshints.info/2013/04/vxlan-scalability-challenges.html) in VMware deployments (as well as some useful background information to help explain the advice).

* Jason Edelman goes on a thought journey [connecting some dots around network APIs, abstractions, and consumption models](http://www.jedelman.com/1/post/2013/03/connecting-the-dots-network-apis-abstractions-and-consumption-models.html). I'll let you read his post for all the details, but I do agree that it is important for the networking industry to converge on a consistent set of abstractions. Jason and I disagree that OpenStack Networking (formerly Quantum) should be the basis here; he says it shouldn't be (not well-known in the enterprise), I say it should be (already represents work created collaboratively by multiple vendors and allows for different back-end implementations).

* Need a reasonable introduction to OpenFlow? [This post](http://www.dasblinkenlichten.com/?p=2582) gives a good introduction to OpenFlow, and the author takes care to define OpenFlow as accurately and precisely as possible.

* SDN, NFV---what's the difference? [This post](http://cplane.net/blog/?p=269) does a reasonable job of explaining the differences (and the relationship) between SDN and NFV.

## Servers/Hardware

* Chris Wahl provides [a quick overview of the HP Moonshot servers](http://wahlnetwork.com/2013/04/09/a-technical-look-into-hp-moonshot/), HP's new ARM-based offerings. I think that Chris may have accidentally overlooked the fact that these servers are **not** x86-based; therefore, a hypervisor such as vSphere is not supported. Linux distributions that offer ARM support, though---like Ubuntu, RHEL, and SuSE---are supported, however. The target market for this is massively parallel workloads that will benefit from having many different cores available. It will be interesting to see how the support of a "Tier 1" hardware vendor like HP affects the adoption of ARM in the enterprise.

## Security

* Ivan Pepelnjak talks about a demonstration of [an attack based on VM BPDU spoofing](http://blog.ioshints.info/2013/04/vm-bpdu-spoofing-attack-works-quite.html). In vSphere 5.1, VMware addressed this potential issue with a feature called BPDU Filter. Check out how to configure BPDU Filter [here](https://blogs.vmware.com/vsphere/2012/11/vsphere-5-1-vds-new-features-bpdu-filter.html).

## Cloud Computing/Cloud Management

* Check out [this post](http://cloudassassin.wordpress.com/2013/04/09/24/) for some vCloud Director and RHEL 6.x interoperability issues.

* Nick Hardiman has a good write-up on [the anatomy of an AWS CloudFormation template](http://www.techrepublic.com/blog/datacenter/anatomy-of-an-aws-cloudformation-template/6117).

* If you missed the OpenStack Summit in Portland, Cody Bunch has a reasonable collection of Summit summary posts [here](http://openstack.prov12n.com/openstack-summit-summary-part-1/) (as well as materials for his hands-on workshops [here](http://openstack.prov12n.com/openstack-summit-summary-pt-2/)). I was also there, and I have some [session live blogs][1] available for your pleasure.

* We've probably all heard the "pets vs. cattle" argument applied to virtual machines in a cloud computing environment, but Josh McKenty of Piston Cloud Computing asks whether it is now time [to apply that thinking to the physical hosts as well](http://www.pistoncloud.com/2013/04/say-goodbye-to-your-operating-system/). Considering that the IT industry still seems to be struggling with applying this line of thinking to virtual systems, I suspect it might be a while before it applies to physical servers. However, Josh's arguments are valid, and definitely worth considering.

* I have to give Rob Hirschfeld some credit for---as a member of the OpenStack Board---acknowledging that, in his words, "we've created such a love fest for OpenStack that I fear we are drinking our own kool aide." Open, honest, transparent dealings and self-assessments are critically important for a project like OpenStack to succeed, so kudos to Rob for posting a [list of some of the challenges](http://robhirschfeld.com/2013/04/23/openstack-enters-rapids-with-grizzly-watch-for-strong-currents-hidden-rocks-eddies/) facing the project as adoption, visibility, and development accelerate.

## Operating Systems/Applications

Nothing this time around, but I'll stay alert for items to add next time.

## Storage

* Nigel Poulton [tackles](http://blog.nigelpoulton.com/3par-asictwo-edged-sword/) the question of whether ASIC (application-specific integrated circuit) use in storage arrays elongates the engineering cycles needed to add new features. This "double edged sword" argument is present in networking as well, but this is the first time I can recall seeing the question asked about modern storage arrays. While Nigel's article specifically refers to the 3PAR ASIC and its relationship to "flash as cache" functionality, the broader question still stands: at what point do the drawbacks of ASICs begin to outweight the benefits?

* Quite some time ago I pointed readers to a post about Target Driven Zoning from Erik Smith at EMC. Erik recently announced that [TDZ works](http://brasstacksblog.typepad.com/brass-tacks/2013/04/target-driven-zoning-works.html) after a successful test run in a lab. Awesome---here's hoping the vendors involved will push this into the market.

* Using iSER (iSCSI Extensions for RDMA) to accelerate iSCSI traffic seems to offer some pretty promising storage improvements (see [this article](http://www.mirantis.com/blog/offloading-cpu-data-traffic-in-openstack-cloud-iscsi-over-rdma/)), but I can't help but feel like this is a really complex solution that may not offer a great deal of value moving forward. Is it just me?

## Virtualization

* Kevin Barrass has [a blog post](http://communities.vmware.com/blogs/kevinbarrass/2012/11/21/simple-vxlan-lab-on-workstation-viewing-traffic-with-wireshark) on the VMware Community site that shows you how to create VXLAN segments and then use Wireshark to decode and view the VXLAN traffic, all using VMware Workstation.

* Andre Leibovici explains [how Horizon View Multi-VLAN works and how to configure it](http://myvirtualcloud.net/?p=4730).

* Looking for a good list of virtualization and cloud podcasts? [Look no further](http://techhead.co/virtualization-cloud-podcast-directory/).

* Need Visio stencils for VMware? [Look no further](http://technodrone.blogspot.com/2013/04/vmware-visio.html).

* It doesn't look like it has changed much from previous versions, but nevertheless some people might find it useful: a "how to" on [virtualization with KVM on CentOS 6.4](http://www.howtoforge.com/virtualization-with-kvm-on-a-centos-6.4-server).

* Captain KVM (cute name, a take-off of Captain Caveman for those who didn't catch it) has a couple of posts on maximizing 10Gb Ethernet on KVM and RHEV (the KVM post is [here](http://captainkvm.com/2013/04/maximizing-your-10gb-ethernet-in-kvm/), the RHEV post is [here](http://captainkvm.com/2013/04/maximizing-your-10gb-ethernet-in-rhev/)). I'm not sure that I agree with his description of LACP bonds ("2 10GbE links become a single 20GbE link"), since any given flow in a LACP configuration can still only use 1 link out of the bond. It's more accurate to say that aggregate bandwidth increases, but that's a relatively minor nit overall.

* Ben Armstrong has a write-up on [how to install Hyper-V's integration components when the VM is offline](http://blogs.technet.com/b/virtualization/archive/2013/04/19/how-to-install-integration-services-when-the-virtual-machine-is-not-running.aspx).

* What are the differences between QuickPrep and Sysprep? Jason Boche's [got you covered](http://www.boche.net/blog/index.php/2013/05/02/quickprep-and-sysprep/).

I suppose that's enough information for now. As always, courteous comments are welcome, so feel free to add your thoughts in the comments below. Thanks for reading!

[1]: {{< relref "2013-04-22-collection-of-openstack-summit-session-liveblogs.md" >}}
