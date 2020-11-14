---
author: slowe
categories: Information
comments: true
date: 2013-04-08T16:18:45Z
slug: technology-short-take-31
tags:
- Automation
- iSCSI
- macOS
- Networking
- OpenFlow
- OpenStack
- Puppet
- SDN
- Security
- Storage
- Virtualization
- VMware
title: 'Technology Short Take #31'
url: /2013/04/08/technology-short-take-31/
wordpress_id: 3129
---

Welcome to Technology Short Take #31, my irregularly published series that takes a look at links, posts, articles, and thoughts from around the web related to core data center technologies. I hope that you find something useful!

## Networking

* Umair Hoodbhoy [speculates in this post](http://umairhoodbhoy.net/2013/02/14/cisco-one-controller-sdn-startup-killer/) that the inclusion of Cisco's ONE Controller in the recently-announced "Daylight" effort could mean the end for Big Switch's Floodlight. (Umair's play on words--"in Daylight there is no need for Floodlights"--is cute.)

* Of course, Big Switch recently moved to "diversify," if you will, away from just Floodlight with the introduction of Switch Light. As usual, Brent Salisbury has [an excellent write-up on Switch Light](http://networkstatic.net/big-switch-introduces-switch-light/), so I recommend reading his post. Switch Light seems like a good idea---more competition is always good, isn't that what people say?--but I wonder how much cooperation Big Switch will get from the major networking vendors with regards to OpenFlow interoperability now that Big Switch is competing even more directly with them via Switch Light.

* I think I might have mentioned this before (sorry if so), but here's a good write-up on [using the Edge Gateway CLI for monitoring and troubleshooting](http://blogs.vmware.com/vsphere/2013/01/monitoring-and-troubleshooting-using-edge-gateway-clis.html). Nice.

* Greg Ferro [examines](http://etherealmind.com/sdn-use-case-firewall-migration-in-the-enterprise/) a potential SDN use case (an OpenFlow use case) in the form of enterprise firewall migrations.

* Just getting started in the networking field? Last year, Brent Salisbury put together a couple of great posts that help "refresh the basics" of networking. Part 1 covers [Ethernet, IP, and TCP headers in Wireshark captures](http://networkstatic.net/what-are-ethernet-ip-and-tcp-headers-in-wireshark-captures/); part 2 pulls that together to show [how the headers encapsulate in the OSI stack](http://networkstatic.net/how-headers-encapsulate-in-the-osi-stack/). If you're not already familiar with this information, this is good reading.

## Servers/Hardware

Nothing this time around, but I'll stay alert for information I can include in the next Technology Short Take!

## Security

* Mounting guest disk images on the host? That's a no-no from a security perspective---see [here](https://www.berrange.com/posts/2013/02/20/a-reminder-why-you-should-never-mount-guest-disk-images-on-the-host-os/) to learn why.

* Mike Foley shared recently that the release candidate of the vSphere 5 Security Hardening Guide has been released. Check it out [here](http://communities.vmware.com/docs/DOC-22783).

## Cloud Computing/Cloud Management

* I haven't had the chance to actually try it out myself, but [Blueprint](http://devstructure.com/blueprint/) looks interesting. As the website describes it, it's designed to "reverse engineer" servers so that you can migrate them into a configuration management system like Chef or Puppet.

* Looking for a decent high-level overview of OpenStack and how it works? Check out [this article titled "In a nutshell: How OpenStack works"](http://vmartinezdelacruz.com/in-a-nutshell-how-openstack-works/). (As an aside, I think it's awesome how Ken Pepple's diagrams show up in all sorts of places. One day I hope my material proves as useful to folks.)

* If you use Puppet for configuration management and want to deploy GlusterFS, be sure to check out [this Puppet Forge module](https://forge.puppetlabs.com/thias/glusterfs). I've tested it and it works as advertised.

* This is an older article (published in May of last year), and it's a bit on the lengthy side, but I like the tack the author uses. He describes [cloud as the synthesis of many different forms of innovation within IT](http://highscalability.com/blog/2012/5/7/startups-are-creating-a-new-system-of-the-world-for-it.html), pulling together things like open source, virtualization, distributed programming, NoSQL, DevOps/NoOps, distributed teams, dynamic languages, and Big Data (among others). He then goes on to provide examples of how organizations building or leveraging clouds are synthesizing these various independent technological innovations together. If you have a few minutes (as I said, it's a bit on the lengthy side), I'd recommend reading it.

## Operating Systems/Applications

* This series is a bit older, but an interesting one nevertheless. Brian McClain, who was one of the presenters in a Cloud Foundry/BOSH session I [liveblogged at VMworld 2012][1], has his own personal blog and posted a series of articles on using BOSH with vSphere. I hadn't really considered how one might use BOSH for deploying (and managing) multi-VM applications on vSphere, but Brian provides some practical examples. Part 1 of the series is [here](http://www.brianmmcclain.com/using-bosh-with-vsphere-part-1/), followed by [part 2](http://www.brianmmcclain.com/using-bosh-with-vsphere-part-2/), [part 3](http://www.brianmmcclain.com/using-bosh-with-vsphere-part-3/), [part 4](http://www.brianmmcclain.com/using-bosh-with-vsphere-part-4/), and [part 5](http://www.brianmmcclain.com/using-bosh-with-vsphere-part-5/).

* Like using Markdown on OS X? You might find [these](http://brettterpstra.com/projects/markdown-service-tools/) handy.

* Ah, the good old days of DOSreborn as [FreeDOS](http://www.freedos.org).

* Go ahead, read up on [YAML](http://yaml.org). You know you want to. Well, YAML _is_ used in both Hiera (can be used with Puppet) and BOSH, after all.

* Here's another interesting tool that I haven't had the opportunity to actually test myself. [Oz](https://github.com/clalancette/oz/wiki) looks like it could be quite useful---especially in virtualized/cloud computing environments---but I'm struggling to determine why I should use Oz instead of OS-specific mechanisms (like a kickstart file). If anyone has used Oz and can shed some light on this question, I'd appreciate it.

* You may have heard that I recently switched from TextMate to BBEdit as my default OS X text editor (and therefore the tool whereby I do most of my content generation). As part of the switch, I found [this](http://ranea.org/bbedit-markdown.html) to be helpful. (I might post a separate entry about the switch, if enough people seem interested in reading about it.)

## Storage

* Want to understand the differences between the block and NAS implementations of VAAI? Cormac Hogan has more information in [this blog post](http://cormachogan.com/2012/11/08/vaai-comparison-block-versus-nas/).

* If you're a real storage geek (or if you're not but are having trouble sleeping), check out [this USENIX presentation on erasure coding in Windows Azure storage](https://www.usenix.org/conference/usenixfederatedconferencesweek/erasure-coding-windows-azure-storage).

* Does a dedicated iSCSI infrastructure make sense? Have a look at Ivan's [view on the question](http://blog.ioshints.info/2013/03/does-dedicated-iscsi-infrastructure.html).

* Dmitry Ukov of Mirantis has a write-up on [Swift and Ceph as object storage in OpenStack environments](http://www.mirantis.com/blog/object-storage-openstack-cloud-swift-ceph/). If you're not familiar with either of these object storage solutions, this is a decent introductory article.

## Virtualization

* [This event](http://techfieldday.com/event/sddc-symposium/) looks interesting.

* Josh Coen has an informative write-up on [the effect of jumbo frames on multi-NIC vMotion performance on 10Gb Ethernet](http://www.valcolabs.com/2013/04/03/jumbo-frames-and-multi-nic-vmotion-performance-over-10gbe/). His testing shows (warning: spoiler alert!) that jumbo frames do not provide a noticeable benefit, and one would be well advised to consider the added complexity that jumbo frames introduce when incorporating them into a design. (OK, Josh didn't say that last part---that was mine.)

* Fellow VCDX and friend Andrea Mauro (based out of Italy) takes [a look at PCoIP versus RDP](http://vinfrastructure.it/en/2013/01/pcoip-vs-rdp/). Naturally, from a performance perspective PCoIP is generally better than RDP, but Andrea points out that sometimes performance isn't the only consideration. He follows that post up with a post [examining PCoIP on WAN connections](http://vinfrastructure.it/en/2013/01/using-pcoip-on-wan-connections/), where he provides some good information on PCoIP and VPNs, WAN accelerators, and protocol tuning.

* [OVS on VirtualBox?](http://networkstatic.net/open-vswitch-on-virtualbox/) Why yes, I think I will.

* In this post, Magnus Ullberg shares a technique for [orchestrating the deployment of VDI templates](http://ullberg.us/orchestrate/214/template-build-process) using vCloud Director, SCCM, and vCenter Orchestrator.

* [This post](http://www.v-front.de/2013/04/the-vmware-tools-gui-is-gone-now-what.html) brings up an interesting change that occurred in recent versions of VMware Tools---the GUI is greatly diminished, instead requiring that you use a command-line tool to change things like time synchronization settings. Interesting.

* Everyone heard about [the vCenter Certification Automation Tool](http://blogs.vmware.com/kb/2013/04/introducing-the-vcenter-certificate-automation-tool-1-0.html), right?

* Using the vCenter Server Appliance (VCSA)? William Lam shows you a nifty trick to [automate SSL certificate regeneration in the VCSA](http://www.virtuallyghetto.com/2013/04/automating-ssl-certificate-regeneration.html).

That's it for this time. I have plenty more links I wanted to share, but I figured I'd better not let this post get any longer. As always, courteous comments are welcome, so I invite you to participate in the conversation by adding your thoughts below.

[1]: {{< relref "2012-08-28-app-cap2165-operating-cloud-foundry.md" >}}
