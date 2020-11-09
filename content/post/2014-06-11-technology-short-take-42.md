---
author: slowe
categories: Information
comments: true
date: 2014-06-11T09:00:00Z
slug: technology-short-take-42
tags:
- Automation
- Fusion
- Hardware
- HyperV
- Linux
- Microsoft
- Networking
- Neutron
- NSX
- OpenStack
- Puppet
- SDN
- Security
- Storage
- UCS
- VMware
- Virtualization
- Cisco
- UCS
- vSphere
- macOS
title: 'Technology Short Take #42'
url: /2014/06/11/technology-short-take-42/
wordpress_id: 3460
---

Welcome to Technology Short Take #42, another installation in my ongoing series of irregularly published collections of news, items, thoughts, rants, raves, and tidbits from around the Internet, with a focus on data center-related technologies. Here's hoping you find something useful!

## Networking

* Anthony Burke's series on VMware NSX continues with [part 5](http://networkinferno.net/installing-vmware-nsx-part-5).

* Aaron Rosen, a Neutron contributor, recently published a post about a Neutron extension called Allowed-Address-Pairs and how you can use it [to create high availability instances using VRRP](http://blog.aaronorosen.com/implementing-high-availability-instances-with-neutron-using-vrrp/) (via keepalived). Very cool stuff, in my opinion.

* Bob McCouch has a post over at Network Computing (where I've recently started blogging as well---see [my first post](http://www.networkcomputing.com/cloud-infrastructure/virtual-machines-vs-containers-a-matter-of-scope/a/d-id/1269190)) discussing his view on how software-defined networking (SDN) will [trickle down](http://www.networkcomputing.com/sdn-waiting-for-the-trickle-down-effect/a/d-id/1204407) to small and mid-sized businesses. He makes comparisons among server virtualization, 10 Gigabit Ethernet, and SDN, and feels that in order for SDN to really hit this market it needs to be "not a user-facing feature, but rather a means to an end" (his words). I tend to agree---focusing on SDN is focusing on the mechanism, rather than focusing on the problems the mechanism can address.

* Want or need to use multiple external networks in your OpenStack deployment? Lars Kellogg-Stedman shows you how in this post on [multiple external networks with a single L3 agent](http://blog.oddbit.com/2014/05/28/multiple-external-networks-wit/).

## Servers/Hardware

* There was some noise this past week about Cisco UCS moving into the top x86 blade server spot for North America in Q1 2014. Kevin Houston takes a moment to explore some ideas why Cisco was so successful in [this post](http://bladesmadesimple.com/2014/06/the-secret-to-how-cisco-took-the-1-blade-server-spot/). I agree that Cisco had some innovative ideas in UCS---integrated management and server profiles come to mind---but my biggest beef with UCS right now is that it is still primarily a north/south (server-to-client) architecture in a world where east/west (server-to-server) traffic is becoming increasingly critical. Can UCS hold on in the face of a fundamental shift like that? I don't know.

## Security

* Need to scramble some data on a block device? Check out [this command](http://www.commandlinefu.com/commands/view/13440/securely-destroy-data-on-given-device-hugely-faster-than-devurandom). (I love the commandlinefu.com site. It reminds me that I still have so much yet to learn.)

## Cloud Computing/Cloud Management

* Want to play around with OpenDaylight and OpenStack? Brent Salisbury has [a write-up](http://networkstatic.net/updated-devstack-opendaylight-vm-image-for-openstack-icehouse/) on how to OpenStack Icehouse (via DevStack) together with OpenDaylight.

* Puppet Labs has released a module that allows users to programmatically (via Puppet) provision and configure Google Compute Platform (GCP) instances. More details are available in [the Puppet Labs blog post](http://puppetlabs.com/blog/automate-google-compute-engine-puppet).

* I love how developers come up with these themes around certain projects. Case in point: "Heat" is the name of the project for orchestrating resources in OpenStack, HOT is the name for the format of Heat templates, and [Flame is the name of a new project to automatically generate Heat templates](http://dev.cloudwatt.com/en/blog/introducing-flame-automatic-heat-template-generation.html).

## Operating Systems/Applications

* I can't imagine that anyone has been immune to the onslaught of information on Docker, but here's an article that might be helpful if you're still looking for [a quick and practical introduction](http://developerblog.redhat.com/2014/05/15/practical-introduction-to-docker-containers/).

* Many of you are probably familiar with Razor, the project that former co-workers Nick Weaver and Tom McSweeney created when they were at EMC. Tom has since moved on to CSC (via the vCHS team at VMware) and has launched a "next-generation" version of Razor called [Hanlon](https://github.com/csc/Hanlon). Read more about Hanlon and why this is a new/separate project in Tom's blog post [here](http://osclouds.wordpress.com/2014/05/22/announcing-hanlon-and-the-hanlon-microkernel/).

* Looking for a bit of clarity around [CoreOS](https://coreos.com) and [Project Atomic](http://www.projectatomic.io)? I found [this post by Major Hayden](http://major.io/2014/05/13/coreos-vs-project-atomic-a-review/) to be extremely helpful and informative. Both of these projects are on my radar, though I'll probably focus on CoreOS first as the (currently) more mature solution.

* Linux Journal has a nice multi-page [write-up on Docker containers](http://www.linuxjournal.com/content/docker-lightweight-linux-containers-consistent-development-and-deployment) that might be useful if you are still looking to understand Docker's basic building blocks.

* I really enjoyed Donnie Berkholz' piece on [microservices and the migrating Unix philosophy](http://redmonk.com/dberkholz/2014/05/20/microservices-and-the-migrating-unix-philosophy/). It was a great view into how composability can (and does) shift over time. Good stuff, I highly recommend reading it.

* cURL is an incredibly useful utility, especially in today's age of HTTP-based REST API. Here's a list of [9 uses for cURL that are worth knowing](http://httpkit.com/resources/HTTP-from-the-Command-Line/). [This article](http://blogs.plexibus.com/2009/01/15/rest-esting-with-curl/) on testing REST APIs with cURL is handy, too.

* And for something entirely differentI know that folks love to beat up AppleScript, but it's [cross-application tasks like this](http://iworkautomation.com/keynote/examples-doc-from-outline.html) that make it useful.

## Storage

* Someone recently brought [the open source Open vStorage project](http://www.openvstorage.com/) to my attention. Open vStorage compares itself to VMware VSAN, but supporting multiple storage backends and supporting multiple hypervisors. Like a lot of other solutions, it's implemented as a VM that presents NFS back to the hypervisors. If anyone out there has used it, I'd love to hear your feedback.

* Erik Smith at EMC has published a series of articles on "virtual storage networks." There's some interesting content there---I haven't finished reading all of the posts yet, as I want to be sure to take the time to digest them properly. If you're interested, I suggest starting out with [his introductory post](http://brasstacksblog.typepad.com/brass-tacks/2014/04/an-introduction-to-virtual-storage-networks.html) (which, strangely enough, _wasn't_ the first post in the series), then moving on to [part 1](http://brasstacksblog.typepad.com/brass-tacks/2014/04/virtual-storage-networks-part-1.html), [part 2](http://brasstacksblog.typepad.com/brass-tacks/2014/04/virtual-storage-networks-part-2-requirements-for-multi-tenant-storage.html), and [part 3](http://brasstacksblog.typepad.com/brass-tacks/2014/05/virtual-storage-networks-part-3-multi-tenant-capable-topologies.html).

## Virtualization

* Did you happen to see this write-up on [migrating a VMware Fusion VM to VMware's vCloud Hybrid Service](https://blogs.vmware.com/vcloud/2014/04/migrate-vmware-fusion-vm-vcloud-hybrid-service.html)? For now---I believe there are game-changing technologies out there that will alter this landscape---one of the very tangible benefits of vCHS is its strong interoperability with your existing vSphere (and Fusion!) workloads.

* Need a listing of the IP addresses in use by the VMs on a given Hyper-V host? Ben Armstrong [shares a bit of PowerShell code](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/04/18/listing-all-the-ip-addresses-used-by-vms.aspx) that produces just such a listing. As Ben points out, this can be pretty handy when you're trying to track down a particular VM.

* vCenter Log Insight 2.0 was recently announced; Vladan Seget has [a decent write-up](http://www.vladan.fr/vmware-vcenter-log-insight-2-0-faster-machine-learning-capabilities/). I'm thinking of putting this into my home lab soon for gathering event information from VMware NSX, OpenStack, and the underlying hypervisors. I just need more than 24 hours in a day

* William Lam has [an article on lldpnetmap](http://www.virtuallyghetto.com/2014/05/quick-tip-lldpnetmap-a-handy-utility-to-map-pnic-to-pswitch-on-esxi.html), a little-known utility for mapping ESXi interfaces to physical switches. As the name implies, this relies on LLDP, so switches that don't support LLDP or that don't have LLDP enabled won't work correctly. Still, a useful utility to have in your toolbox.

* Technology previews of the next versions of Fusion (Fusion 7) and Workstation (Workstation 11) are available; see Eric Sloof's articles ([here](http://www.ntpro.nl/blog/archives/2639-VMware-Fusion-7-Technology-Preview-Now-Available.html) and [here](http://www.ntpro.nl/blog/archives/2638-VMware-Workstation-11-Technology-Preview-Now-Available.html) for Fusion and Workstation, respectively) for more details.

* vSphere 4 (and associated pieces) are [no longer under general support](http://planetvm.net/blog/?p=2684). Sad face, but time stops for no man (or product).

* Having some problems with VMware Fusion's networking? Cody Bunch channels his inner Chuck Norris to [kick VMware Fusion networking in the teeth](http://openstack.prov12n.com/kicking-vmware-fusion-networking-in-the-teeth/).

* Want to preview OS X Yosemite? Check out William Lam's [guide to using Fusion or vSphere to preview the new OS X beta release](http://www.virtuallyghetto.com/2014/06/test-drive-apple-osx-10-10-yosemite-on-vmware-fusion-vsphere.html).

I'd better wrap this up now, or it's going to turn into [one of Chad's posts](http://virtualgeek.typepad.com/virtual_geek/2014/03/a-few-thoughts-and-opinions-on-vsan-and-hyperconvergence.html). (Just kidding, Chad!) Thanks for taking the time to read this far!
