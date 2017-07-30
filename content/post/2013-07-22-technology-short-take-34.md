---
author: slowe
categories: Information
comments: true
date: 2013-07-22T09:00:00Z
slug: technology-short-take-34
tags:
- Automation
- HyperV
- Linux
- Networking
- OpenFlow
- OpenStack
- Puppet
- Storage
- vCloud
- Virtualization
- VMFS
- VMware
- vSphere
- Xen
title: 'Technology Short Take #34'
url: /2013/07/22/technology-short-take-34/
wordpress_id: 3227
---

Welcome to Technology Short Take #34, my latest collection of links, articles, thoughts, and ideas from around the web, centered on key data center technologies. Enjoy!

## Networking

* Henry Louwers has [a nice write-up](http://hlouwers.wordpress.com/2013/06/17/choose-your-netscaler-wisely/) on some of the design considerations that go into selecting a Citrix NetScaler solution.

* Scott Hogg [explores jumbo frames and their benefits/drawbacks](http://www.networkworld.com/community/blog/jumbo-frames) in a clear and concise manner. It's worth reading if you aren't familiar with jumbo frames and some of the considerations around their use.

* The networking "old guard" likes to talk about how x86 servers and virtualization create network bottlenecks due to performance concerns, but as Ivan points out in [this post](http://blog.ioshints.info/2013/04/virtual-appliance-performance-is.html), it's rapidly becoming---or has already become---a non-issue. (By the way, if you're not already reading all of Ivan's content, you need to be. Seriously.)

* Greg Ferro, aka EtherealMind, has a great series of articles on overlay networking (a component technology used in a number of network virtualization solutions). Greg [starts out](http://etherealmind.com/overlay-networking-is-more-and-better-while-ditching-the-toxic-sludge/) with a quick look at the value prop for overlay networking. In addition to highlighting one key value of overlay networking---that decoupling the logical network from the physical network enables more rapid change and innovation---Greg also establishes that overlay networking is _not_ new. Greg continues with a more detailed look at [how overlay networking works](http://etherealmind.com/introduction-to-how-overlay-networking-and-tunnel-fabrics-work/). Finally, Greg takes a look at [whether overlay networking and the physical network should be integrated](http://etherealmind.com/integrating-overlay-networking-and-the-physical-network/); he arrives at the conclusion that integrating the two is likely to be unsuccessful given the history of such attempts in the past.

* Terry Slattery [ruminates](http://www.netcraftsmen.net/blogs/softwaredefinednetwork/entry/the-virtual-network-instance.html) on the power of creating (and using) the right abstraction in networking. The value of the "right abstraction" has come up a number of times; it was a featured discussion point of Martin Casado's talk at the OpenStack Summit in Portland in April, and takes center stage [in a recent post over at Network Heresy](http://networkheresy.com/2013/07/15/visibility-debugging-and-network-virtualization-part-1/).

* Here's a decent two-part series about running Vyatta on VMware Workstation ([part 1](http://wojcieh.net/vyatta-router-running-on-vmware-workstation-part-1/) and [part 2](http://wojcieh.net/vyatta-router-running-on-vmware-workstation-part-2-dns-firewall-and-nat/)).

* Could we use OpenFlow to build better internet exchanges? Here's [one idea](http://pieknywidok.blogspot.co.nz/2013/07/building-better-internet-exchange-with.html).

## Servers/Hardware

* The Dell VRTX made its debut recently. Kevin Houston has three blog posts that provide some additional information on VRTX: [a quick video introduction](http://bladesmadesimple.com/2013/06/an-introduction-to-dell-poweredge-vrtx/), a [more detailed look](http://bladesmadesimple.com/2013/06/a-detailed-look-at-dell-poweredge-vrtx/), and [a demonstration of how quiet the VRTX is](http://bladesmadesimple.com/2013/06/demonstrating-the-quietness-of-dell-poweredge-vrtx/).

* In this post, William Lam [explores the potential benefits](http://www.virtuallyghetto.com/2013/06/will-intels-vmcs-shadowing-feature.html) of Intel's VMCS Shadowing functionality on nested virtualization.

## Security

I have nothing to share this time around, but I'll keep watch for content to include in future Technology Short Takes.

## Cloud Computing/Cloud Management

* Tom Fojta takes a look at integrating vCloud Automation Center (vCAC) with vCloud Director in [this post](http://fojta.wordpress.com/2013/06/03/integration-of-vcloud-automation-center-with-vcloud-director/). (By the way, congrats to Tom on becoming the _first_ VCDX-Cloud!)

* In case you missed it, here's the recording for [the #vBrownBag session with Jon Harris on vCAC](http://professionalvmware.com/2013/06/vbrownbag-follow-up-vmware-vcloud-automation-center-with-jon-harris-jon-harrisnm/). (I had the opportunity to hear Jon speak about his employer's vCAC deployment and some of the lessons learned at a recent New Mexico VMUG meeting.)

## Operating Systems/Applications

* Need to do some port forwarding on a Linux box and want to automate the configuration with Puppet? Here's [a configuration example](http://chriscowan.us/posts/firewall-port-forwarding-with-puppet).

* Nick Buraglio provides some quick-and-dirty instructions for [building FlowVisor on CentOS 6](http://www.forwardingplane.net/2013/07/building-flowvisor-on-centos-6-quick-and-dirty/).

## Storage

* Rawlinson Rivera starts to address a lack of available information about Virsto in the first of a series of posts on VMware Virsto. This initial post provides [an introduction to Virsto](http://blogs.vmware.com/vsphere/2013/06/introduction-to-vmware-virsto.html); future posts will provide more in-depth technical details (which is what I'm really looking forward to getting).

* Nigel Poulton [talks a bit about target driven zoning](http://blog.nigelpoulton.com/anyone-for-target-driven-zoning/), something I've mentioned before on this site. For more information on target driven zoning (also referred to as peer zoning), also be sure to check out [Erik Smith's blog](http://brasstacksblog.typepad.com/).

* Now that he's had some time to come up to speed in his new role, Frank Denneman has started a great series on the basic elements of PernixData's Flash Virtualization Platform (FVP). You can read part 1 [here](http://frankdenneman.nl/2013/06/18/basic-elements-of-the-flash-virtualization-platform-part-1/) and part 2 [here](http://frankdenneman.nl/2013/07/02/basic-elements-of-fvp-part-2-using-own-platform-versus-in-place-file-system/). I'm looking forward to future parts in this series.

* I'd often wondered this myself, and now Cormac Hogan has the answer: [why is uploading files to VMFS so slow?](http://cormachogan.com/2013/07/18/why-is-uploading-files-to-vmfs-so-slow/) Good information.

## Virtualization

* Looks like Hyper-V supports an [out-of-band initial replication method for Hyper-V Replica](http://blogs.technet.com/b/virtualization/archive/2013/06/28/save-network-bandwidth-by-using-out-of-band-initial-replication-method-in-hyper-v-replica.aspx). I can see where this would be quite useful in some situations.

* Gabrie van Zanten shows [how to add the Fusion IO driver to VMware AutoDeploy](http://www.gabesvirtualworld.com/add-fusion-io-driver-to-vmware-auto-deploy/) in this write-up. Thanks Gabe!

* Interested in running RHEV as a nested VM on ESXi? William Lam [shows you how](http://www.virtuallyghetto.com/2013/07/how-to-run-nested-rhev-hypervisor-on.html). William has been posting some great stuff recently; in addition to the nested RHEV article I just referenced, you might also be interested in his [quick-start guide for OpenStack on vSphere](http://www.virtuallyghetto.com/2013/07/how-to-quickly-get-started-with-vmware.html), or his 2 part series on forwarding VM logs to syslog ([part 1](http://www.virtuallyghetto.com/2013/07/a-hidden-vsphere-51-gem-forwarding.html) and [part 2](http://www.virtuallyghetto.com/2013/07/a-hidden-vsphere-51-gem-forwarding_10.html)).

* Itzik Reich provides some [tips for tuning I/O](http://itzikr.wordpress.com/2013/07/17/under-the-dome0-tweaking-xenserver-for-emc-xtremio/) on XenServer when working with EMC XtremIO arrays.

* Some good information here from Jason Boche on [vCloud Director, RHEL 6.3, and Windows Server 2012 NFS](http://www.boche.net/blog/index.php/2013/07/16/vcloud-director-rhel-6-3-and-windows-server-2012-nfs/).

* Here's a quick note from Eric Gray on [automating the VMware Tools install on Ubuntu 12.04](http://www.vcritical.com/2013/07/automating-vmware-tools-install-on-ubuntu-12-04/).

* Josh Townsend has [an updated version of his HAProxy virtual appliance](http://vmtoday.com/2013/07/updated-load-balancer-virtual-appliance/) available.

* PowerCLI guru Luc Dekens has a pair of PowerCLI scripts to help with common homelab tasks: a script to [clone a VM without vCenter](http://www.lucd.info/2013/06/30/hl-tools-part-1-clone-a-vm-without-vcenter/), and a script to [create a nested ESXi hypervisor](http://www.lucd.info/2013/07/12/hl-tools-part-2-create-a-nested-hypervisor/).

* Setting a Hyper-V VM to use a wide-screen resolution is actually pretty easy, based on Ben Armstrong's instructions [here](http://blogs.msdn.com/b/virtual_pc_guy/archive/2013/07/09/configuring-wide-screen-resolutions-in-a-hyper-v-virtual-machine.aspx).

* For beginners, here's a write-up on [using Fusion to build a vSphere lab](http://www.webestigate.com/2011/03/11/how-to-create-a-vsphere-lab-on-a-mac/).

* Another good post by Cormac Hogan [answering some questions about SIOC](http://blogs.vmware.com/vsphere/2013/07/sioc-which-ios-are-charged-to-the-vm.html).

* New network port diagram for vSphere 5.x? Get it [here](http://blogs.vmware.com/kb/2013/07/new-network-port-diagram-for-vsphere-5-x.html).

It's time to wrap up now, or this Technology Short Take is going to turn into a Technology Long Take. Anyway, I hope you found something useful in this little collection. If you have any feedback or suggestions for improvement for future posts, feel free to speak up in the comments below.
