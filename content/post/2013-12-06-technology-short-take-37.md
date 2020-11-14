---
author: slowe
categories: Information
comments: true
date: 2013-12-06T09:00:00Z
slug: technology-short-take-37
tags:
- macOS
- ESXi
- Linux
- Networking
- NSX
- OpenStack
- Storage
- Virtualization
- VMware
title: 'Technology Short Take #37'
url: /2013/12/06/technology-short-take-37/
wordpress_id: 3371
---

Welcome to Technology Short Take #37, the latest in my irregularly-published series in which I share interesting articles from around the Internet, miscellaneous thoughts, and whatever else I feel like throwing in. Here's hoping you find something useful!

## Networking

* Ivan does [a great job](http://blog.ipspace.net/2013/08/management-control-and-data-planes-in.html) of describing the difference between the management, control, and data planes, as well as providing examples. Of course, the distinction between control plane protocols and data plane protocols [isn't always perfectly clear](http://blog.ipspace.net/2013/10/what-exactly-is-control-plane.html).

* You've heard me talk about [snowflake servers](http://martinfowler.com/bliki/SnowflakeServer.html) before. In this post on [why networking needs a Chaos Monkey](http://www.plexxi.com/2013/10/designing-resilience-case-network-chaos-monkey/), Mike Bushong applies to the terms to networks---a _snowflake network_ is an intricately crafted network that is carefully tailored to utilize a custom subset of networking features unique to your environment. What is the fix---if one exists---to snowflake networks? Designing your network for resiliency and unleashing a Chaos Monkey on it is one way, as Mike points out. A fan of network virtualization might also say that decomposing today's complex physical networks into multiple simple logical networks on top of a simpler physical transport network---similar to Mike's suggestion of converging on a smaller set of reference architectures---might also help. (Of course, I am a fan of network virtualization, since I work with/on VMware NSX.)

* Martijn Smit has launched a series of articles on VMware NSX. Check out part 1 ([general introduction](http://lostdomain.org/2013/10/20/vmware-nsx-general/)) and part 2 ([distributed services](http://lostdomain.org/2013/11/03/vmware-nsx-distributed-services/)) for more information.

* The [elephants and mice](http://networkheresy.com/2013/11/01/of-mice-and-elephants/) post at Network Heresy has sparked some discussion across the "blogosphere" about how to address this issue. (Note that my name is on the byline for that Network Heresy post, but I didn't really contribute all that much.) Jason Edelman took up the idea of [using OpenFlow to provide a dedicated core/spine for elephant flows](http://www.jedelman.com/1/post/2013/11/a-dedicated-data-center-corespine-for-your-special-elephants.html), while Marten Terpstra at Plexxi talks about how Plexxi's [Affinities could be used](http://www.plexxi.com/2013/11/elephants-need-affinities/#sthash.35qrqBIn.dpbs) to help address the problem of elephant flows. Peter Phaal speaks up in the comments to Marten's article about [how sFlow can be used to rapidly detect elephant flows](http://blog.sflow.com/2013/01/rapidly-detecting-large-flows-sflow-vs.html), and points to a demo taking place during SC13 that shows [sFlow tracking elephant flows](http://blog.sflow.com/2013/11/sc13-large-flow-demo.html) on SCinet (the SC13 network).

* Want some additional information on layer 2 and layer 3 services in VMware NSX? Here's [a good source](http://blog.ipspace.net/2013/11/layer-2-and-layer-3-switching-in-vmware.html).

* [This](https://github.com/johann8384/puppet-routing/) looks interesting, but I'm not entirely sure how I might go about using it. Any thoughts?

## Servers/Hardware

Nothing this time around, but I'll keep my eyes peeled for something to include next time!

## Security

I don't have anything to share this time---feel free to suggest something to include next time.

## Cloud Computing/Cloud Management

* Red Hat has a great [breakdown of all the different components in a typical OpenStack environment using OVS and GRE tunnels](http://openstack.redhat.com/Networking_in_too_much_detail). If you're looking for more information on how all the various pieces fit together, I definitely recommend reading this---it's worth your time.

## Operating Systems/Applications

* I found this post on [getting the most out of HAProxy](https://www.twilio.com/engineering/2013/10/16/haproxy)--in which Twilio walks through some of the configuration options they're using and why---to be quite helpful. If you're relatively new to HAProxy, as I am, then I'd recommend giving this post a look.

* [This list](http://www.maclife.com/article/columns/terminal_101_5_timesaving_tips_and_tricks) is reasonably handy if you're not a Terminal guru. While written for OS X, most of these tips apply to Linux or other Unix-like operating systems as well. I particularly liked tip #3, as I didn't know about that particular shortcut.

* Mike Preston has a great series going on tuning Debian Linux running under vSphere. In [part 1](http://blog.mwpreston.net/2013/08/06/tuning-linux-debian-in-a-vsphere-vm-part-1-installation/), he covered installation, primarily centered around LVM and file system mount options. In [part 2](http://blog.mwpreston.net/2013/08/09/tuning-linux-debian-in-a-vsphere-vm-part-2-virtual-hardware/), Mike discusses things like using the appropriate virtual hardware, the right kernel modules for VMXNET3, getting rid of unnecessary hardware (like the virtual floppy), and similar tips. Finally, in [part 3](http://blog.mwpreston.net/2013/09/16/tuning-linux-debian-in-a-vsphere-vm-part-3-usrbinrandom/), he talks about a hodgepodge of tips---things like blacklisting other unnecessary kernel drivers, time synchronization, and modifying the Linux I/O scheduler. All good stuff, thanks Mike!

## Storage

* "Captain KVM," aka Jon Benedict, takes on [the discussion of enterprise storage vs. open source storage solutions](http://captainkvm.com/2013/10/openstack-netapp-not-such-an-odd-couple/) in OpenStack environments. One good point that Jon makes is that solutions need to be evaluated on a variety of criteria. In other words, it's not just about cost nor is it just about performance. You need to use the right solution for your particular needs. It's nice to see Jon say that if your needs are properly met by an open source solution, then "by all means stick with Ceph, Gluster, or any of the other cool software storage solutions out there." More vendors need to adopt this viewpoint, in my humble opinion. (By the way, if you're thinking of using NetApp storage in an OpenStack environment, here's [a "how to"](http://captainkvm.com/2013/10/using-netapp-cinder-drivers-with-openstack/) that Jon wrote.)

* Duncan Epping has a quick post about [a VMware KB article update regarding EMC VPLEX and Storage DRS/Storage IO Control](http://www.yellow-bricks.com/2013/11/01/emc-vplex-storage-drs-storage-io-control/). The update is actually applicable to all vMSC configurations, so have a look at Duncan's article if you're using or considering the use of vMSC in your environment.

* Vladan Seget has [a look at Microsoft ReFS](http://www.vladan.fr/microsoft-refs-first-look/).

## Virtualization

* A relatively little-known change in vSphere 5.5 involves how ESXi handles device drivers. ESXi 5.5 introduces the idea of _native device drivers_, eliminating a driver compatibility layer (or shim) that existed in previous versions of ESXi. (William Lam has more information on the [ESXi 5.5 native device driver architecture](http://www.virtuallyghetto.com/2013/10/esxi-55-introduces-new-native-device.html).) Unfortunately, it looks like this switch has had [a small side effect](http://www.v-front.de/2013/10/the-good-and-bad-of-new-native-driver.html): developing ESXi drivers is now limited to VMware Technology Alliance Partners, since the tools to develop drivers is no longer published under an open source license. Will this have a negative effect on the availability of ESXi device drivers?

* Andre Leibovici provides a round-up of [VMware Horizon View 5.3 limits and maximums](http://myvirtualcloud.net/?p=5518).

* Paul Meehan calls out some potential "marketing spin" in [a recent post on comparing Hyper-V and vSphere](http://www.virtualizationsoftware.com/vsphere-hyper-v-phoney-war/). You might also find [Paul's follow-up](http://www.virtualizationsoftware.com/the-phony-war-continuesshooting-blanks/) an interesting read as well.

* As you may have heard, VMware recently released a set of [VMware Tools for nested ESXi](http://www.virtuallyghetto.com/2013/11/w00t-vmware-tools-for-nested-esxi.html) instances. Vladan Seget shows how to build a [custom ISO with VMware Tools for nested ESXi](http://www.vladan.fr/how-to-build-a-custom-iso-with-vmware-tools-for-nested-esxi/) to help simplify the process of bringing up nested instances. Good stuff!

* Wade Holmes gives the solution in case you find yourself, as he did, [with three hosts and two VSAN datastores](https://vwade.wordpress.com/2013/10/31/three-hosts-two-vsan-datastores/).

I'd better wrap it up here so this doesn't get too long for folks. As always, your courteous comments and feedback are welcome, so feel free to start (or join) the discussion below.
