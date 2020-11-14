---
author: slowe
categories: Information
comments: true
date: 2013-06-04T11:53:10Z
slug: technology-short-take-33
tags:
- Automation
- Hardware
- HP
- HyperV
- Linux
- macOS
- Networking
- OpenStack
- Puppet
- SDN
- vCloud
- Virtualization
- vSphere
- VXLAN
- Writing
- Security
- Storage
title: 'Technology Short Take #33'
url: /2013/06/04/technology-short-take-33/
wordpress_id: 3204
---

Welcome to Technology Short Take #33, the latest in my irregularly-published series of articles discussing various data center technology-related links, articles, rants, thoughts, and questions. I hope that you find something useful here. Enjoy!

## Networking

* Tom Nolle [asks the question](http://blog.cimicorp.com/?p=1280), "Is virtualization reality even more elusive than virtual reality?" It's a good read; the key thing that I took away from it was that SDN, NFV, and related efforts are great, but what we really need is something that can pull all these together in a way that customers (and providers) reap the benefits.

* What happens when multiple VXLAN logical networks are mapped to the same multicast group? Venky explains it in [this post](http://blogs.vmware.com/vsphere/2013/05/vxlan-series-multiple-logical-networks-mapped-to-one-multicast-group-address-part-4.html). Venky also has a great write-up on how the VTEP (VXLAN Tunnel End Point) [learns and creates the forwarding table](http://blogs.vmware.com/vsphere/2013/05/vxlan-series-how-vtep-learns-and-creates-forwarding-table-part-5.html).

* This post by Ranga Maddipudi shows you [how to use App Firewall in conjunction with VXLAN logical networks](http://blogs.vmware.com/vsphere/2013/05/using-app-firewall-with-vxlan-networks.html).

* Jason Edelman is on a roll with a couple of great blog posts. First up, Jason goes off on [a rant about network virtualization](http://www.jedelman.com/1/post/2013/06/network-virtualization-general-rant.html), briefly hitting topics like the relationship between overlays and hardware, the role of hardware in network virtualization, the changing roles of data center professionals, and whether overlays are the next logical step in the evolution of the network. I particularly enjoyed the snippet from [the post by Bill Koss](http://siwdt.com/2013/05/07/sdn-its-free-just-like-a-puppy/). Next, Jason dives a bit deeper on [the relationship between network overlays and hardware](http://www.jedelman.com/1/post/2013/06/network-overlays-and-hardware-do-they-go-together.html), and shares his thoughts on where it does---and doesn't---make sense to have hardware terminating overlay tunnels.

* [Another post](http://blog.cimicorp.com/?p=1262) by Tom Nolle explores the relationship---complicated at times---between SDN, NFV, and the cloud. Given that we define the cloud (sorry to steal your phrase, Joe) as elastic, pooled resources with self-service functionality and ubiquitous access, I can see where Tom states that to discuss SDN or NFV without discussing cloud is silly. On the flip side, though, I have to believe that it's possible for organizations to make a gradual shift in their computing architectures and processes, so one almost _has_ to discuss these various components individually, because to tie them all together makes it almost impossible. Thoughts?

* If you haven't already introduced yourself to VXLAN (one of several draft protocols used as an overlay protocol), Cisco Inferno has [a reasonable write-up](http://blog.ciscoinferno.net/who-needs-more-than-4094-vlans).

* I know Steve Jin, and he's a really smart guy. I must disagree with some of his statements regarding [what software-defined networking is and is not and where it fits](http://www.doublecloud.org/2013/04/what-software-defined-networking-is-and-is-not-and-where-it-fits/), written back in April. I talked before about the difference between network virtualization and SDN, so no need to mention that again. Also, the two key flaws that Steve identifies---single point of failure and scalability---aren't flaws with SDN/network virtualization, but rather flaws in an _implementation_ of said technologies, IMHO.

## Servers/Hardware

* Correction from the [last Technology Short Take][1]---I incorrectly stated that the HP Moonshot offerings were ARM-based, and therefore wouldn't support vSphere. I was wrong. The servers (right now, at least) are running Intel Atom S1260 CPUs, which are x86-based and do offer features like Intel VT-x. Thanks to all who pointed this out, and my apologies for the error!

* I missed this on the #vBrownBag series: [designing HP Virtual Connect for vSphere 5.x](http://professionalvmware.com/2013/05/vbrownbag-follow-up-designing-virtual-connect-for-vsphere-with-joe-clark-elgwhoppo/).

## Security

* Via Forbes Guthrie on Twitter, I saw this post on [how to setup a CA on Linux and use it in a Windows environment](http://virtuallyhyper.com/2013/04/setup-your-own-certificate-authority-ca-on-linux-and-use-it-in-a-windows-environment/).

* Ranga also has a nice write-up (via [Trevor Gerdes](https://twitter.com/trevorgerdes)) on [using the SSL VPN functionality in vCNS Edge](http://blogs.vmware.com/vsphere/2013/04/vcloud-networking-and-security-5-1-edge-ssl-vpn-configuration.html).

* ["Best practices" for vCNS 5.1 App Firewall?](http://blogs.vmware.com/vsphere/2013/06/vcloud-networking-and-security-5-1-app-firewall-best-practices.html) For shamedidn't anyone tell them the correct term is recommended practices? (I'm kidding, of course---the content is well worth reading.)

## Cloud Computing/Cloud Management

* Hyper-V as hypervisor with OpenStack Compute? Sure, [see here](http://www.cloudbase.it/openstack/openstack-compute-installer/).

* Cody Bunch, who has been focusing quite a bit on OpenStack recently, has a nice write-up on using Razor and Chef to automate an OpenStack build. Part 1 is [here](http://openstack.prov12n.com/chef-razor-openstack-part-1/); part 2 is [here](http://openstack.prov12n.com/chef-razor-openstack-part-2/). Good stuff---keep it up, Cody!

* I've mentioned in some of my OpenStack presentations (see [SpeakerDeck](http://speakerdeck.com/slowe) or [Slideshare](http://slideshare.net/lowescott)) that a great place to start if you're just getting started is DevStack. Here, Brent Salisbury has a nice write-up on [using DevStack to install OpenStack Grizzly](http://networkstatic.net/installing-openstack-grizzly-with-devstack).

## Operating Systems/Applications

* [Boxen](http://boxen.github.com/), a tool created by GitHub to manage their OS X Mountain Lion laptops for developers, looks interesting. Might be a useful tool for other environments, too.

* If you use TextMate2 (I switched to BBEdit a little while ago after being a long-time TextMate user), you might enjoy this quick post by Colin McNamara on [Puppet syntax highlighting using TextMate2](http://www.colinmcnamara.com/puppet-syntax-highlighting-using-textmate2/).

## Storage

* Anyone have more information on Jeda Networks? They've been mentioned a couple of times on GigaOm ([here](http://gigaom.com/2013/02/19/jeda-networks-proposes-yet-another-software-defined-option-for-the-data-center/) and [here](http://gigaom.com/2013/04/24/jeda-networks-promises-software-defined-storage-controller-to-come-soon/)), but I haven't seen anything concrete yet. Hey, Stephen Foskett, if you're reading: get Jeda Networks to the next Tech Field Day.

* Tim Patterson shares some code from Luc Dekens that helps [check VMFS version and block sizes using PowerCLI](http://timsvirtualworld.com/2013/05/checking-vmfs-version-and-block-sizes-with-powercli/). This could come in quite handy in making sure you know how your datastores are configured, especially if you are in the midst of a migration or have inherited an environment from someone else.

## Virtualization

* Interested in using SAML and Horizon Workspace with vCloud Director? Tom Fojta [shows you how](http://fojta.wordpress.com/2013/04/07/vcloud-director-and-single-sign-on-saml/).

* If you aren't using vSphere Host Profiles, this [write-up on the VMware SMB blog](http://blogs.vmware.com/smb/2013/05/more-virtualization-benefits-taking-advantage-of-your-vsphere-host-profile-feature.html) might convince you why you should and show you how to get started.

* Michael Webster tackles the question: [is now the best time to upgrade to vSphere 5.1?](http://longwhiteclouds.com/2013/05/23/is-now-the-best-time-to-upgrade-to-vsphere-5-1/) Read the full post to see what Michael has to say about it.

* Duncan points out an easy error to make when working with vSphere HA heartbeat datastores in [this post](http://www.yellow-bricks.com/2013/05/23/number-of-vsphere-ha-heartbeat-datastores-less-than-2-error-while-having-more/). Key takeaway: sometimes the fix is a lot simpler than we might think at first. (I know I'm guilty of making things more complicated than they need to be at times. Aren't we all?)

* Jon Benedict (aka "Captain KVM") shares a script he wrote to [help provide high availability for RHEV-M](http://captainkvm.com/2013/05/providing-high-availability-for-rhev-m/).

* Chris Wahl has a nice write-up on [using log shipping to protect your vCenter database](http://wahlnetwork.com/2012/03/25/protecting-the-vcenter-database-with-sql-log-shipping/). It's a bit over a year old (surprised I missed it until now), and---as Chris points out---log shipping doesn't protect the database (primary and secondary copies) against corruption. However, it's better than nothing (which I suspect it what far too many people are using).

## Other

* If you aspire to be a writer---whether that be a blogger, author, journalist, or other---you might find this article on [using the DASH method for writing](http://www.amanet.org/training/articles/Writing-with-DASH.aspx) to be helpful. The six tips at the end of the article are especially helpful, I think.

Time to wrap this up for now; the rest will have to wait until the next Technology Short Take. Until then, feel free to share your thoughts, questions, or rants in the comments below. Courteous comments are always welcome!

[1]: {{< relref "2013-05-03-technology-short-take-32.md" >}}
