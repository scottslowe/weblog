---
author: slowe
categories: Information
comments: true
date: 2014-05-09T09:00:00Z
slug: technology-short-take-41
tags:
- Hardware
- HP
- Networking
- NSX
- OpenFlow
- OpenStack
- KVM
- OVS
- Puppet
- Security
- SSL
- Storage
- VMotion
- VMware
- vSphere
- Virtualization
- HyperV
title: 'Technology Short Take #41'
url: /2014/05/09/technology-short-take-41/
wordpress_id: 3452
---

Welcome to Technology Short Take #41, the latest in my series of random thoughts, articles, and links from around the Internet. Here's hoping you find something useful!

## Networking

* Network Functions Virtualization (NFV) is a networking topic that is starting to get more and more attention (some may equate "attention" with "hype"; I'll allow you to draw your own conclusion there). In any case, I liked how [this article](http://getcloudify.org/2014/04/09/network_automation_NFV_cloud_orchestration.html) really hit upon what I personally feel is something many people are overlooking in NFV. Many vendors are simply rushing to provide virtualized versions of their solution without addressing the orchestration and automation side of the house. I'm looking forward to part 2 on this topic, in which the author plans to share more technical details.

* Rob Sherwood, CTO of Big Switch, recently published a reasonably in-depth [look at "modern OpenFlow" implementations and how they can leverage multiple tables](http://bigswitch.com/blog/2014/05/02/modern-openflow-and-sdn) in hardware. Some good information in here, especially on OpenFlow basics (good for those of you who aren't familiar with OpenFlow).

* Connecting Docker containers to Open vSwitch is one thing, but what about using Docker containers to run Open vSwitch in userspace? Read [this](https://github.com/dave-tucker/docker-ovs).

* Ivan knocks centralized SDN control planes in [this post](http://blog.ipspace.net/2014/05/does-centralized-control-plane-make.html). It sounds like Ivan favors scale-out architectures, not scale-up architectures (which are typically what is seen in centralized control plane deployments).

* Looking for more VMware NSX content? Anthony Burke has started a new series focusing on VMware NSX in pure vSphere environments. As far as I can tell, Anthony is up to 4 posts in the series so far. Check them out here: [part 1](http://networkinferno.net/installing-vmware-nsx-part-1), [part 2](http://networkinferno.net/installing-vmware-nsx-part-2), [part 3](http://networkinferno.net/installing-vmware-nsx-part-3), and [part 4](http://networkinferno.net/installing-vmware-nsx-part-4). Enjoy!

## Servers/Hardware

* Good friend Simon Seagrave is back to the online world again with this heads-up on [a potential NIC issue with an HP Proliant firmware update](http://techhead.co/hp-proliant-firmware-update-potential-nic-issue/). The post also contains a link to a fix for the issue. Glad to see you back again, Simon!

* Tom Howarth asks, ["Is the x86 blade server dead?"](http://www.virtualizationpractice.com/next-x86-hardware-industry-death-blade-format-25417/) (OK, so he didn't use _those_ words specifically. I'm paraphrasing for dramatic effect.) The basic premise of Tom's position is that new technologies like server-side caching and VSAN/Ceph/Sanbolic (turning direct-attached storage into shared storage) will dramatically change the landscape of the data center. I would generally agree, although I'm not sure that I agree with Tom's statement that "complexity is reduced" with these technologies. I think we're just shifting the complexity to a different place, although it's a place where I think we can better manage the complexity (and perhaps mask it). What do you think?

## Security

* Perhaps in light of the recent Heartbleed issue, this "how to" on [implementing Perfect Forward Secrecy (PFS) with Nginx on Ubuntu/Debian](http://www.howtoforge.com/ssl-perfect-forward-secrecy-in-nginx-webserver) has popped up. (Need more details on PFS? [This article](https://community.qualys.com/blogs/securitylabs/2013/06/25/ssl-labs-deploying-forward-secrecy) from June 2013 is pretty helpful.)

## Cloud Computing/Cloud Management

* Juan Manuel Rey has launched a series of blog posts on deploying OpenStack with KVM and VMware NSX. He has three parts published so far; all good stuff. See [part 1](http://jreypo.wordpress.com/2014/04/29/deploying-openstack-with-kvm-and-vmware-nsx-part-1-nsx-overview-and-initial-setup/), [part 2](http://jreypo.wordpress.com/2014/05/06/deploying-openstack-with-kvm-and-vmware-nsx-part-2-configure-nsx-transport-and-logical-network-views/), and [part 3](http://jreypo.wordpress.com/2014/05/07/deploying-openstack-with-kvm-and-vmware-nsx-part-3-kvm-hypervisor-and-gluster-storage-setup/).

* Kyle Mestery brought to my attention (via Twitter) this [list of the "best newly-available OpenStack guides and how-to's"](http://opensource.com/business/14/5/new-openstack-tutorials). It was good to see a couple of Cody Bunch's articles on the list; Cody's been producing some really useful OpenStack content recently.

* I haven't had the opportunity to use [SaltStack](http://docs.saltstack.com/en/latest/) yet, but I'm hearing good things about it. It's always helpful (to me, at least) to be able to look at products in the context of solving a real-world problem, which is why seeing this post with details on [using SaltStack to automate OpenStack deployment](http://csscorp.github.io/openstack-automation/) was helpful.

* Here's a heads-up on [a potential issue with the vCAC 6.0.1.1 upgrade](http://blog.alanrocks.com/?p=282)--the upgrade apparently changes some configuration files. The linked blog post provides more details on which files get changed. If you're looking at doing this upgrade, read this to make sure you aren't adversely affected.

* Here's a post with some additional [information on OpenStack live migration](http://kimizhang.wordpress.com/2013/08/26/openstack-vm-live-migration/) that you might find useful.

## Operating Systems/Applications

* RHEL7, Docker, and Puppet together? [Here's a post](http://shadow-soft.com/rhel7-docker-openstack-puppet-oh/) on just such a use case (oh, I forgot to mention OpenStack's involved, too).

* Have you ever walked through a spider web because you didn't see it ahead of time? (Not very fun.) Sometimes I feel that way with certain technologies or projects---like there are connections there with other technologies, projects, trends, etc., that aren't quite "visible" just yet. That's where I am right now with the recent hype around containers and how they are going to replace VMs. I'm not so sure I agree with that just yetbut I have more noodling to do on the topic.

## Storage

* "Server SAN" seems to be the name that is emerging to describe various technologies and architectures that create pools of storage from direct-attached storage (DAS). This would include products like VMware VSAN as well as projects like Ceph and others. Stu Miniman has a nice write-up [on Server SAN](http://wikibon.org/wiki/v/Server_SAN_Market_Definition) over at Wikibon; if you're not familiar with some of the architectures involved, that might be a good place to start. Also at Wikibon, David Floyer has a write-up on [the rise of Server SAN](http://wikibon.org/wiki/v/The_Rise_of_Server_SAN) that goes into a bit more detail on business and technology drivers, friction to adoption, and some recommendations.

* Red Hat recently [announced](http://www.redhat.com/about/news/press-archive/2014/4/red-hat-to-acquire-inktank-provider-of-ceph) they were acquiring Inktank, the company behind the open source scale-out Ceph project. Jon Benedict, aka "Captain KVM," weighs in with [his thoughts](http://captainkvm.com/2014/05/thoughts-on-red-hats-purchase-of-inktank/) on the matter. Of course, there's no shortage of thoughts on the acquisition---a quick web search will prove that---but I find it interesting that none of the "big names" in storage social media had anything to say (not that I could find, anyway). Howard? Stephen? Chris? Martin? Bueller?

## Virtualization

* Doug Youd pulled together a nice summary of some of [the issues and facts around routed vMotion](http://blog.cnidus.net/2014/05/09/why-routed-vmotion/) (vMotion across layer 3 boundaries, such as across a Clos fabric/leaf-spine topology). It's definitely worth a read (and not just because I get mentioned in the article, either---although that doesn't hurt).

* I've talked before---although it's been a while---about Hyper-V's choice to rely on host-level NIC teaming in order to provide network link redundancy to virtual machines. Ben Armstrong talks about another option, guest-level NIC teaming, in [this post](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/04/16/hyper-v-networking-nic-teaming.aspx). I'm not so sure that using guest-level teaming is any better than relying on host-level NIC teaming; what's really needed is a more full-featured virtual networking layer.

* Want to run nested ESXi on vCHS? Well, it's not supportedbut William Lam [shows you how](http://www.virtuallyghetto.com/2014/05/how-to-run-nested-esxi-on-the-vcloud-hybrid-service.html) anyway. Gotta love it!

* Brian Graf shows you [how to remove IP pools using PowerCLI](http://blogs.vmware.com/PowerCLI/2014/05/remove-ip-pools-using-powercli.html).

Well, that's it for this time around. As always, I welcome all courteous comments, so feel free to share your thoughts, ideas, rants, links, or feedback in the comments below.
