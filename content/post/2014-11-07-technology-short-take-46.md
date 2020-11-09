---
author: slowe
categories: Information
comments: true
date: 2014-11-07T14:00:00Z
slug: technology-short-take-46
tags:
- Docker
- Hardware
- Linux
- Networking
- NSX
- OpenStack
- Security
- Storage
- Virtualization
- VMware
- Fusion
title: 'Technology Short Take #46'
url: /2014/11/07/technology-short-take-46/
wordpress_id: 3560
---

Welcome to Technology Short Take #46. That's right, it's time for yet another collection of links and articles from around the Internet on various data center-related technologies, products, projects, and efforts. As always, there is no rhyme or reason to my collection; this is just a glimpse into what I've seen over the past few weeks. I hope you are able to glean something useful.

## Networking

* This post by Matt Oswalt---the first in a series, apparently---provides a great introduction to [5 development tools for network engineers](http://keepingitclassless.net/2014/10/five-dev-tools-network-engineers/). I've already increased my usage of Git in an effort to become more fluent with this very popular version control tool, and I was already planning on exploring Jinja2 as well (these are both mentioned in Matt's article). This is a really useful post and I'm looking forward to future posts in this series.
* Matt also recently posted part 4 (of 5) in his series on SDN protocols; this post covers [OpFlex and declarative networking](http://keepingitclassless.net/2014/09/sdn-protocols-4-opflex-declarative-networking/).
* It was good to read [this post on Cumulus Linux first impressions](http://packetlife.net/blog/2014/oct/1/cumulus-linux-first-impressions/) by Jeremy Stretch. I'm a fan of Cumulus, but I'm admittedly a Linux guy (see [here][1]) so you might say I'm a bit biased. Jeremy is a "hard-core" networking professional, and so hearing his feedback on Cumulus Linux was, in my opinion, useful. I like that Jeremy was completely honest: "I'm not going to lie: Cumulus Linux was not immediately appealing to me." I highly encourage reading this article.
* If you're interested in more details on how NSX handles ARP suppression, Dmitri Kalintsev has [a post just for you](http://telecomoccasionally.wordpress.com/2014/10/27/nsx-v-under-the-hood-vxlan-arp-suppression/). Dmitri has some [other great NSX-related content](http://telecomoccasionally.wordpress.com/category/nsx/) as well.

## Servers/Hardware

* While all the attention is "up the stack," there are still some occasions when you need to worry about the details in the hardware. Kevin Houston's recent article on [selecting the right memory for your blade server](http://bladesmadesimple.com/2014/10/choosing-the-right-memory-for-your-blade-server/) is one such example.
* ["Junk-box infrastructure,"](http://www.storagebod.com/wordpress/?p=1675) eh? Interesting thought. There's no doubt Greg Ferro had to be involved somehow in this discussion; this rings of the "post-scarcity" discussions he and I had at IDF 2014 in September.

## Security

Nothing this time around, but I'll stay tuned for material to include next time. You're also invited to share relevant links or articles in the comments!

## Cloud Computing/Cloud Management

* Jay Pipes has [an excellent and well-written post on the core of OpenStack](http://www.joinfu.com/2014/09/so-what-is-the-core-of-openstack/). I really appreciate Jay's focus on what's beneficial to the users of OpenStack: the cloud operators, the end users/consumers, and the developers building applications on top of OpenStack.
* Andy Bruce has an article on [adding external networks to Neutron with GRE](https://www.softwareab.net/wordpress/openstack-adding-external-networks-neutron-gre/).
* Previous Technology Short Takes have mentioned CoreOS and Kubernetes in the same sentence, but as far as I can tell I haven't pointed readers to this two part series on running Kubernetes directly on CoreOS. See [part 1](https://coreos.com/blog/running-kubernetes-example-on-CoreOS-part-1/) and [part 2](https://coreos.com/blog/running-kubernetes-example-on-CoreOS-part-2/) for the full details. (Nice use of VMware Fusion in part 2, by the way.)
* Here's an article on [using Terraform in conjunction with Docker and Digital Ocean](https://blog.starkandwayne.com/2014/10/16/terraforming-workloads-with-docker-and-digital-ocean/). Terraform looks interesting, but I wish it would add an OpenStack provider.
* Here's [a walkthrough to running containers on vCloud Air](http://blog.pacogomez.com/running-containers-on-vcloud-air/).
* VMware, Vagrant, and Docker together is the subject of [this blog post by Fabio Rapposelli](http://gosddc.com/articles/dock-your-container-on-vmware-with-vagrant/). This is useful information if you are looking to combine these technologies in a useful way.
* As can be expected given the very recent release of Juno, the fall 2014 release of OpenStack, a number of Juno-specific "how to install" pages are popping up. Here's [one such example](http://intocloud.org/?p=281). Much of the content is similar to previous "how to install" guides that I've seen, but it might be useful to a few folks out there.

## Operating Systems/Applications

* I'm seriously considering using the information in [this article on an IRC proxy](https://dague.net/2014/09/13/my-irc-proxy-setup/) for myself. I find it enormously helpful to stay connected to various open source-related IRC channels, but staying logged while on the move is, for all intents and purposes, impossible. Perhaps the use of an IRC proxy can help. Anyone else out there using a setup like this?
* Did you happen to notice that [CoreOS is now available on Microsoft Azure](https://coreos.com/blog/coreos-available-on-azure/)? It makes me wonder when VMware will announce support for CoreOS on vCloud Air.

## Storage

* I'm not sure if this falls into storage or virtualization, but we'll place it here in the Storage section. Eric Sloof (in conjunction with VMworld TV) has [a video introducing readers to CloudVolumes](http://www.ntpro.nl/blog/archives/2764-What-are-CloudVolumes.html), the relatively recent VMware acquisition that's being put to work in the end-user computing space.
* Greg Schulz has a decent two-part (well, three part actually, but it's only the second two that interest me) series on VVols and storage I/O fundamentals ([part 1](http://storageioblog.com/vmware-vvols-and-storage-io-fundementals/) and [part 2](http://storageioblog.com/vmware-vvols-and-storage-io-fundementals-part-2/)).

## Virtualization

* It will be nice when the virtualization industry converges on some common set of disk formats for virtual machines. OVF/OVA was an attempt, but there's still some work to be done on that front. Until then, VM converters like [version 3.0 of the Microsoft Virtual Machine Converter](http://www.microsoft.com/en-us/download/details.aspx?id=42497) will keep popping up.
* William Lam has a great guest post from Peter Bjork on [a Mac Mini setup running VSAN](http://www.virtuallyghetto.com/2014/10/a-killer-custom-apple-mac-mini-setup-running-vsan.html).
* A short while ago I gave you [a quick introduction to Vagrant][2]. One of the key components of Vagrant is the _box_, which is essentially a VM template. Cody Bunch recently published a post on [using Packer to make Vagrant boxes](http://blog.codybunch.com/posts/2014-10-28-Using-Packer-to-Make-Vagrant-Boxes/), which might come in handy if you want to create your own Vagrant boxes.
* Here's [a quick reminder](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/10/16/the-rpc-server-is-unavailable-with-microsoft-virtual-machine-converter.aspx) from Ben Armstrong that if you want to use the Microsoft Virtual Machine Converter 3.0, you'll need to be sure to unblock WMI in the Windows Firewall.
* Want to install ESXi 5.5 Patch03 on the new Mac Pro? William Lam [shows you how](http://www.virtuallyghetto.com/2014/10/how-to-install-esxi-5-5-patch03-on-the-new-mac-pro-61.html).
* Installing CoreOS on vSphere got a bit easier, thanks to the inclusion of Open VM Tools in CoreOS Alpha 490.0.0 and a new script by William Lam. [Here's the details.](http://www.virtuallyghetto.com/2014/11/how-to-quickly-deploy-new-coreos-image-wvmware-tools-on-esxi.html) (I guess William has been doing some super-useful stuff, since I keep referencing his links here. Keep up the great work, William!)
* Gabrie van Zanten brings up a flaw between VMware Auto Deploy (in vSphere 5.1) and Microsoft Cluster Server (MSCS). What's the flaw, you say? [The two won't work together.](http://www.gabesvirtualworld.com/vmware-auto-deploy-mscs-wont-work/)

I have more articles in my bucket labeled "Articles to blog about," but I'll save those for some other time in the interest of keeping this from getting overly long (which it probably already is). Until next time!

[1]: {{< relref "2014-04-16-crossing-the-threshold.md" >}}
[2]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
