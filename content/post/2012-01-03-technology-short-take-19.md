---
author: slowe
categories: Information
comments: true
date: 2012-01-03T15:20:11Z
slug: technology-short-take-19
tags:
- Cisco
- EMC
- FibreChannel
- macOS
- Microsoft
- Networking
- Storage
- Vblock
- vCloud
- Virtualization
- VMware
- VXLAN
- UCS
- OpenFlow
- EMC
- vSphere
title: 'Technology Short Take #19'
url: /2012/01/03/technology-short-take-19/
wordpress_id: 2512
---

Welcome to Technology Short Take #19, the first Technology Short Take for 2012. Here's this year's first collection of links, articles, and thoughts regarding virtualization, storage, networking, and other data center technology-related topics. I hope you find something useful!

## Networking

* While configuration limits aren't the most exciting reading, they are important from time to time. Here's some [configuration limits for the UCS 6100 and 6200 series](http://www.cisco.com/en/US/docs/unified_computing/ucs/sw/configuration_limits/2.0/b_UCS_Configuration_Limits_2_0.html).

* Understanding the differences---both positive and negative---between the various approaches to solving a particular challenge is a key skill. That's why I like this article on [HP Flex-10 versus NIOC for VDI](http://bladesmadesimple.com/2011/12/hp-flex-10-vs-vmware-vsphere-network-io-control-for-vdi-2/). The author (Dwayne) weighs the pros and cons of both approaches in helping to shape network traffic for VDI deployments using 10Gb Ethernet.

* It would appear that my recent VXLAN and OTV connectivity posts (incorrect VXLAN post [here][1], corrected VXLAN post [here][2], and OTV/VXLAN post [here][3]) sparked a discussion about whether we really need to concern ourselves with traffic trombones. On one side we have Brad Hedlund [speculating](http://bradhedlund.com/2011/12/22/on-optimizing-traffic-for-network-virtualization/) that the network should be treated like a large virtual I/O fabric; on the other side we have Greg Ferro [countering](http://etherealmind.com/responding-on-optimizing-traffic-for-network-virtualization/) that we _do_ need to be concerned about the topology of the network. I can see both sides of the argument, but at this stage of the game, I'm inclined to agree more with Greg. In the future (it's unclear how far in the future) I think that Brad's points will be more valid, but not right now.

* This post by Ivan Pepelnjak on [VXLAN, IP multicast, OpenFlow, and control planes](http://blog.ioshints.info/2011/12/vxlan-ip-multicast-openflow-and-control.html) highlights some of the current limitations with VXLAN and thus reinforces why I think that Brad's arguments are a bit ahead of their time.

* A few folks had some write-ups on Embrane Heleos: [Greg Ferro](http://etherealmind.com/scaling-virtual-appliances-embrane/), [Jason Edelman](http://www.jedelman.com/1/post/2011/12/embrane-heleos-great-pricing-model.html), [Brad Hedlund](http://bradhedlund.com/2011/12/12/first-take-on-embrane-heleos/), [Brad Casemore](http://nerdtwilight.wordpress.com/2011/12/12/embrane-emerges-from-stealth-brings-heleos-to-light/), and [Ivan Pepelnjak](http://www.ipspace.net/Embrane_heleos:_scale-out_distributed_virtual_appliance). My question (and this is spurred in part by some comments by Brad Casemore): is this another Cisco spin-in move?

## Servers/Operating Systems/Applications

* Want to play around with Hadoop/MapReduce? [This quick tutorial](http://www.commoncrawl.org/mapreduce-for-the-masses/) might be useful in getting you started.

* This two-part series on Hadoop ([part 1](http://blogs.cisco.com/datacenter/why-hadoop-part-1/) and [part 2](http://blogs.cisco.com/datacenter/why-hadoop-part-2/)) might also help you understand the value of Hadoop and where it fits into your overall technology profile.

* Continuing with Hadoop, this post by Brad Hedlund on [understanding Hadoop clusters and the network](http://bradhedlund.com/2011/09/10/understanding-hadoop-clusters-and-the-network/) gives a great overview of Hadoop and highlights some of the networking concerns/considerations.

* I mentioned that one of [my 2012 projects][4] is learning Perl, and here are three "beginner's introduction" articles that I've found useful so far ([Part 1](http://www.perl.com/pub/2008/04/23/a-beginners-introduction-to-perl-510.html), [Part 2](http://www.perl.com/pub/2008/05/07/beginners-introduction-to-perl-510-part-2.html), and [Part 3](http://news.oreilly.com/2008/06/a-beginners-introduction-to-pe.html)).

* Fellow vSpecialist Clint Kitson recently started blogging, and here's [a recent post on a function he wrote for PowerShell](http://velemental.com/2012/01/03/rubbing-a-bit-of-linux-init-into-powershell/) that brings some color-coding feedback into the environment.

* Running Mac OS X 10.7 Lion on your Mac? Then this article on [setting up dual Time Machine backup destinations](http://geekyschmidt.com/2011/12/29/dual-time-machine-wielding-backups) might be useful to you. (As an aside, I'll add an extra plug for [ControlPlane](http://controlplane.dustinrue.com/)--it's a very handy little tool that I also use).

## Storage

* Are you a networking geek, but haven't had the chance to do much with Fibre Channel? This [guide to Fibre Channel switching basics for network engineers](http://routerjockey.com/2011/12/23/mds-fiber-channel-switching-basics-for-network-engineers/) might be handy to get you started.

* Check out this post for a heads-up regarding [soft media errors with SSDs and FAST Cache on EMC arrays](http://emcsan.wordpress.com/2011/12/15/problem-with-soft-media-errors-on-ssd-drives-and-fastcache/) (thanks to Chad Sakac for the link).

## Virtualization

* I'm not sure how often this actually comes up, but Ben Armstrong shows here how to [use "built in" applications in Windows XP mode](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/12/22/using-built-in-applications-with-windows-xp-mode.aspx) under Windows 7. Ben also has a write-up on [how to convert to a fixed virtual hard disk](http://blogs.msdn.com/b/virtual_pc_guy/archive/2011/12/29/converting-to-a-fixed-virtual-hard-disk-the-hard-way.aspx) "the hard way".

* Duncan revisits [vSphere HA isolation response with IP storage](http://www.yellow-bricks.com/2011/12/15/vsphere-ha-isolation-response-when-using-ip-storage/) in this recent post, pointing out a potential issue that can occur when both the IP-based storage link and the management link are lost. From what I gather from both the article and the comments, it sounds like this isn't limited to only when the isolation response is set to "Leave Powered On."

* If you, like me, will be expanding beyond VMware in 2012, then this [open source cloud and virtualization terminology post](http://www.siliconloons.com/?p=71) will be useful to you. As I've mentioned before, one of the most important steps in learning new technologies is learning the vocabulary. (Oh, and it might be worth it to just subscribe to this site. It looks to have an interesting mix of content coming.)

* Seen the [VMware Technical Publications YouTube channel](http://www.youtube.com/user/VMwareTechPubs)?

* Here's a good post on [using vSphere Auto Deploy with Cisco UCS](http://infrastructureadventures.com/2011/12/11/scaling-vmware-deployments-with-cisco-ucs-and-vmware-auto-deploy/).

* Alan Renouf has posted a great PowerCLI script that helps [automate the start-up of your virtual environment](http://www.virtu-al.net/2011/12/14/vm-start-up-script/). This is a handy tool that should be in the toolbox of any administrator with a virtual vCenter instance in their environment.

* Fellow vSpecialist and VCDX Matt Cowger has a good write-up on [installing the vCenter Operations Symmetrix Collector plugin](http://blog.cowger.us/2011/12/22/installing-the-vcops-symmetrix-collector-plugin/).

* Kendrick Coleman has a good discussion of [vCloud Director provider vDC strategies](http://www.kendrickcoleman.com/index.php?/Tech-Blog/rethinking-your-vcloud-director-provider-vdc-strategy-with-vblock.html). His discussion is partially specific to Vblock, but some of the information would apply to any environment.

* Frank Denneman has a good post on [the recommendation to use similar types of disks in a datastore cluster](http://frankdenneman.nl/2012/01/impact-of-load-balancing-on-datastore-cluster-configuration/).

* Does Cisco regret not buying VMware? Brad Casemore [seems to think so](http://nerdtwilight.wordpress.com/2011/12/13/reflecting-on-the-big-acquisition-cisco-didnt-make/).

* I think I might have referenced these before, but these Linked Clone posts on the VMware vSphere blog are a good read ([part 1](http://blogs.vmware.com/vsphere/2011/11/linked-clones-part-1-fast-provisioning-in-vcloud-director-15.html) and [part 2](http://blogs.vmware.com/vsphere/2011/11/linked-clones-part-2-desktop-provisioning-in-vmware-view-50.html)).

* Tony Bourke is depressed at the [current state of statelessness](http://datacenteroverlords.com/2012/01/03/depressingstate-of-stateleness/). I can't say that I disagree.

* Massimo has a good write-up on [vCloud Director, custom portals, and backend integrations in service provider environments](http://blogs.vmware.com/vcloud/2011/11/vcd-custom-portals-and-backend-integrations-in-a-service-provider-environment.html).

And that it's for this time around; as always, I hope you've found something useful here. Courteous comments are always welcome; feel free to speak up below.

[1]: {{< relref "2011-11-30-vxlan-and-layer-3-connectivity.md" >}}
[2]: {{< relref "2011-12-07-revisiting-vxlan-and-layer-3-connectivity.md" >}}
[3]: {{< relref "2011-12-22-otv-and-vxlan-layer-3-connectivity-compared.md" >}}
[4]: {{< relref "2012-01-02-some-projects-for-2012.md" >}}
