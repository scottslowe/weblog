---
author: slowe
categories: Information
comments: true
date: 2014-02-06T09:34:31Z
slug: technology-short-take-38
tags:
- Cisco
- CLI
- EMC
- HyperV
- Linux
- Networking
- NSX
- OpenStack
- OVS
- SDN
- Storage
- Virtualization
- VSAN
- Puppet
- iSCSI
- KVM
- VMware
- vSphere
title: 'Technology Short Take #38'
url: /2014/02/06/technology-short-take-38/
wordpress_id: 3400
---

Welcome to Technology Short Take #38, another installment in my irregularly-published series that collects links and thoughts on data center-related technologies from around the web. But enough with the introduction, let's get on to the content already!

## Networking

* Jason Edelman does some [experimenting with the Python APIs on a Cisco Nexus 3000](http://www.jedelman.com/1/post/2014/01/nexus-3000-python-linux-and-open-switch-platforms.html). In the process, he muses about the value of configuration management tool chains such as Chef and Puppet in a world of "open switch" platforms such as Cumulus Linux.

* Speaking of Cumulus Linuxdid you see [the announcement](http://cumulusnetworks.com/press_releases/detail/20140128-cumulus-networkstm-announces-partnership-and-distribution-agreement-with-dell/) that Dell has signed a reseller agreement with Cumulus Networks? I'm pretty excited about this announcement, and I hope that Cumulus sees great success as a result. There are a variety of write-ups about the announcement; so good, many not so good. The not-so-good variety typically refers to Cumulus' product as an SDN product when technically it isn't. [This article on Barron's by Tiernan Ray](http://blogs.barrons.com/techtraderdaily/2014/01/28/dell-to-resell-linux-networking-software-from-cumulus-a-warm-cuddly-blanket-to-displace-cisco/) is a pretty good summary of the announcement and some of its implications.

* Pete Welcher has launched a series of articles discussing "practical SDN," focusing on the key leaders in the market: NSX, DFA, and the yet-to-be-launched ACI. In the [initial installation of the series](http://www.netcraftsmen.net/blogs/entry/practical-sdn-nsx-dfa-and-aci-the-all-seeing-eye.html), he does a good job of providing some basics around each of the products, although (as would be expected of a product that hasn't launched yet) he has to do some guessing when it comes to ACI. The series continues with a discussion of [L2 forwarding](http://www.netcraftsmen.net/blogs/softwaredefinednetwork/entry/practical-sdn-l2-forwarding-in-nsx-dfa-and-aci.html) and [L3 forwarding](http://www.netcraftsmen.net/blogs/softwaredefinednetwork/entry/practical-sdn-l3-forwarding-in-nsx-dfa-and-aci.html) across the various products. Definitely worth reading, in my opinion.

* Nick Buraglio takes away all your reasons for not collecting flow-based data from your environment with his write-up on [installing nfsen and nfdump for NetFlow and/or sFlow collection](http://www.forwardingplane.net/2014/01/install-nfsen-and-nfdump-on-centos-6-5-for-netflow-and-or-sflow-collection/).

* Terry Slattery has a nice write-up on [new network designs that are ideally suited for SDN](http://www.netcraftsmen.net/blogs/softwaredefinednetwork/entry/network-designs-that-support-sdn.html). If you are looking for a primer on "next-generation" network designs, this is worth reviewing.

* Need some Debian packages for Open vSwitch 2.0? Here's another article from Nick Buraglio---he has [some information](http://www.forwardingplane.net/2013/11/openvswitch-2-0-debian-packages/) to help you out.

## Servers/Hardware

Nothing this time, but check back next time.

## Security

Nothing from my end. Maybe you have something you'd like to share in the comments?

## Cloud Computing/Cloud Management

* Christian Elsen (who works in Integration Engineering at VMware) has a nice series of articles going on using OpenStack with vSphere and NSX. The series starts [here](http://www.edge-cloud.net/2013/12/openstack-vsphere-nsx/), but follow the links at the bottom of that article for the rest of the posts. This is really good stuff---he includes the use of the NSX vSwitch with vSphere 5.5, and talks about vSphere OpenStack Virtual Appliance (VOVA) as well. All in all, well worth a read in my opinion.

* Maish Saidel-Keesing (one of my co-authors on the first edition of _VMware vSphere Design_ and also a super-sharp guy) recently wrote an article on [how adoption of OpenStack will slow the adoption of SDN](http://technodrone.blogspot.com/2014/01/sdn-adoption-is-not-as-easy-as-you-think.html). While I agree that widespread adoption of OpenStack could potentially retard the evolution of enterprise IT, I'm not necessarily convinced that it will slow the adoption of SDN and network virtualization solutions. Why? Because, in part, I believe that the full benefits of something like OpenStack _need_ a good network virtualization solution in order to be realized. Yes, some vendors are writing plugins for Neutron that manipulate physical switches. But for developers to get true isolation, application portability, the ability to re-create production environments in development---all that is going to require network virtualization.

* Here's a useful [OpenStack CLI cheat sheet](http://anystacker.com/2014/02/openstack-command-line-cheat-sheet/) for some commonly-used commands.

## Operating Systems/Applications

* If you're using [Ansible](http://www.ansibleworks.com/) (a product I haven't had a chance to use but I'm closely watching), but I came across this article on [an upcoming change to the SSH transport that Ansible uses](http://blog.ansibleworks.com/2014/01/15/ssh-connection-upgrades-coming-in-ansible-1-5/). This change, referred to as "ssh_alt," promises a significant performance increase for Ansible. Good stuff.

* I don't think I've mentioned this before, but Forbes Guthrie (my co-author on the _VMware vSphere Design_ books and an already great guy) has a series going on using Linux as a domain controller for a vSphere-based lab. The series is up to four parts now: [part 1](http://www.vreference.com/2014/01/20/a-linux-based-domain-controller-for-a-vsphere-lab-part-1/), [part 2](http://www.vreference.com/2014/01/21/a-linux-based-domain-controller-for-a-vsphere-lab-part-2/), [part 3](http://www.vreference.com/2014/01/23/a-linux-based-domain-controller-for-a-vsphere-lab-part-3/), and [part 4](http://www.vreference.com/2014/01/24/a-linux-based-domain-controller-for-a-vsphere-lab-part-4/).

* Need (or want) to increase the SCSI timeout for a KVM guest? See [these instructions](http://captainkvm.com/2014/01/extending-scsi-timeouts-in-kvm-guests/).

* I've been recommending that IT pros get more familiar with Linux, as I think its influence in the data center will continue to grow. However, the problem that I sometimes face is that experienced folks tend to share these "super commands" that ordinary folks have a hard time decomposing. However, [this site should make that easier](http://explainshell.com). I've tried it---it's actually pretty handy.

## Storage

* Jim Ruddy (an EMCer, former co-worker of mine, and an overall great guy) has a pretty cool series of articles discussing the use of EMC ViPR in conjunction with OpenStack. Want to use OpenStack Glance with EMC ViPR using ViPR's Swift API support? See [here](http://theruddyduck.typepad.com/theruddyduck/2013/12/configure-openstack-glance-to-use-swift-api-with-emc-vipr.html). Want a multi-node Cinder setup with ViPR? Read how [here](http://theruddyduck.typepad.com/theruddyduck/2014/01/multi-node-cinder-with-emc-vipr.html). Multi-node Glance with ViPR? He's [got it](http://theruddyduck.typepad.com/theruddyduck/2014/01/multi-node-glance-with-emc-vipr.html). If you're new to ViPR (who outside of EMC isn't?), you might also find his articles on [deploying EMC ViPR](http://theruddyduck.typepad.com/theruddyduck/2013/11/deploy-emc-vipr.html), [setting up back-end storage for ViPR](http://theruddyduck.typepad.com/theruddyduck/2013/11/emc-vipr-discover-vnx-and-isilon-arrrays-for-physical-assets.html), or [deploying object services with ViPR](http://theruddyduck.typepad.com/theruddyduck/2013/12/deploy-object-services-with-emc-vipr.html) to also be helpful.

* Speaking of ViPR, EMC has apparently decided to release it for free for non-commercial use. See [here](http://www.emc.com/getvipr).

* Looking for more information on VSAN? Look no further than Cormac Hogan's extensive VSAN series (up to Part 14 at last check!). The best way to find this stuff is to check [articles tagged VSAN on Cormac's site](http://cormachogan.com/category/vmware/vsan/). The official VMware vSphere blog also has a series of articles running; check out [part 1](http://blogs.vmware.com/vsphere/2013/12/virtual-san-hardware-guidance-part-1-solid-state-drives.html) and [part 2](http://blogs.vmware.com/vsphere/2014/01/virtual-san-hardware-guidance-part-ii-storage-controllers.html).

## Virtualization

* Did you happen to see [this news about Microsoft Hyper-V Recovery Manager (HRM)](http://blogs.technet.com/b/in_the_cloud/archive/2014/01/16/announcing-the-ga-of-windows-azure-hyper-v-recovery-manager.aspx)? This is an Azure-hosted service that can be roughly compared to VMware's Site Recovery Manager (SRM). However, unlike SRM (which is hosted on-premise), HRM is hosted by Microsoft Azure. As the article points out, it's important to understand that this doesn't mean your VMs are replicated to Azure---it's just the orchestration portion of HRM that is running in Azure.

* Oh, and speaking of Hyper-Vin early January Microsoft [released version 3.5 of their Linux Integration Services](http://blogs.technet.com/b/virtualization/archive/2014/01/02/linux-integration-services-3-5-announcement.aspx), which primarily appears to be focused on adding Linux distribution support (CentOS/RHEL 6.5 is now supported).

* Gregory Gee has a write-up on [installing the Cisco CSR 1000V in VirtualBox](http://gregorygee.wordpress.com/2014/01/09/installing-cisco-csr-1000v-in-virtualbox/). (I'm a recent VirtualBox convert myself; I find the `vboxmanage` command just so very handy.) Note that I haven't tried this myself, as I don't have a Cisco login to get the CSR 1000V code. If any readers have tried it, I'd love to hear your feedback. Gregory also has a few other interesting posts I'm planning to review in the next few weeks as well.

* Sunny Dua, who works with VMware PSO in India, has a series of blog posts on architecting vSphere environments. It's currently up to five parts; I don't know how many more (if any) are planned. Here are the links: [part 1 (clusters)](http://vxpresss.blogspot.in/2013/11/part-1-architecting-vsphere-clusters.html), [part 2 (vCenter SSO)](http://vxpresss.blogspot.in/2013/12/part-2-architecting-vcenter-single-sign.html), [part 3 (storage)](http://vxpresss.blogspot.in/2013/12/part-3-architecting-storage-for-vsphere.html), [part 4 (design process)](http://vxpresss.blogspot.in/2013/12/part-4-architecting-vsphere-remember.html), and [part 5 (networking)](http://vxpresss.blogspot.com.au/2014/01/part-5-architecting-vsphere-networks.html).

It's time to wrap up now before this gets any longer. If you have any thoughts or tidbits you'd like to share, I welcome any and all courteous comments. Join (or start) the conversation!
