---
author: slowe
categories: Information
comments: true
date: 2014-09-03T09:00:00Z
slug: technology-short-take-44
tags:
- Automation
- Docker
- Hardware
- HyperV
- Linux
- Networking
- OpenFlow
- OpenStack
- Puppet
- Security
- Storage
- Virtualization
- VXLAN
- Windows
title: 'Technology Short Take #44'
url: /2014/09/03/technology-short-take-44/
wordpress_id: 3509
---

Welcome to Technology Short Take #44, the latest in my irregularly-published series of articles, links, ideas, and thoughts about various data center-related technologies. Enjoy!

## Networking

* One of the original problems with the VXLAN IETF specification was that it (deliberately) didn't include any control plane information; as a result, the process of mapping MAC addresses to VTEPs (VXLAN Tunnel Endpoints) wasn't defined, and the early implementations relied on multicast to handle this issue. To help resolve this issue, Cumulus Networks (and possibly Metacloud, I'm not sure of their involvement yet) has release [an open source project called vxfld](https://github.com/CumulusNetworks/vxfld). As described in [this Metacloud blog post](http://www.metacloud.com/opening-vxlan-openstack/), vxfld is designed to "handle VXLAN traffic from any operationg system or hardware platform that adheres to the IETF Internet-Draft for VXLAN".

* Nir Yechiel recently posted part 1 of a discussion on [the need for network overlays](http://thenetworkway.wordpress.com/2014/07/01/the-need-for-network-overlays-part-i/). This first post is more of a discussion of why VLANs and VLAN-based derivatives aren't sufficient, and why we should be looking to routing (layer 3) constructs instead. I'm looking forward to part 2 of the series.

* One ongoing discussion in the network industry these days---or so it seems---is the discussion about the interaction between network overlays and the underlying transport network. Some argue that tight integration is required; others point to streaming video services and VoIP running across the Internet and insist that no integration or interaction is needed. In [this post](http://robhirschfeld.com/2014/07/08/sdn-blind-spots/), Scott Jensen argues in favor of the former---that SDN solutions shouldn't just manage network overlays, but should also manage the configuration of the physical transport network as well. I'd love to hear from more networking pros (please disclose company affiliations) about their thoughts on this matter.

* I like the distinction made [here](http://keepingitclassless.net/2014/06/network-automation-or-sdn/) between network automation and SDN.

* Need to get a better grasp on OpenFlow? Check out [OpenFlow basics](http://keepingitclassless.net/2014/07/sdn-protocols-1-openflow-basics/) and [OpenFlow deep-dive](http://keepingitclassless.net/2014/07/sdn-protocols-2-openflow-deep-dive/).

* Here's a write-up on [connecting Docker containers using VXLAN](http://blog.thestateofme.com/2014/06/08/connecting-docker-containers-between-vms-with-vxlan/). I think there's a great deal of promise for OVS in containerized environments, but what's needed is better/tighter integration between OVS and container solutions like Docker.

## Servers/Hardware

* Is Intel having second thoughts about software-defined infrastructure? That's the core question in [this blog post](http://blogs.computerworlduk.com/idc-insight/2014/08/intels-role-in-the-future-of-software-defined-infrastructure/index.htm), which explores the future of Intel in a software-defined world and the increasing interest in non-x86 platforms like ARM.

* On the flip side, proponents who claim that platforms like ARM and others are _necessary_ in order to move forward with SDN and NFV initiatives should probably read [this article](http://www.rcrwireless.com/20140820/telecom-software/telefonica-brocade-tout-80-gbps-nfv-speeds-in-tests-tag2) on 80 Gbps performance from an off-the-shelf x86 server. Impressive.

## Security

* It's nice to see that work on OpenStack Barbican is progressing nicely; see [this article](http://thenewstack.io/openstack-barbican-cryptography-for-managing-secrets-in-the-cloud/) for a quick overview of the project and an update on the status.

## Cloud Computing/Cloud Management

* SDN Central has [a nice write-up](http://www.sdncentral.com/news/policy-open-efforts-essential-sally-johnson/2014/08/) on the need for open efforts in the policy space, which includes the Congress project.

* The use of public cloud offerings as disaster recovery targets is on the rise; note this article from Microsoft on [how to migrate on-premises workloads to Azure using Azure Site Recovery](http://azure.microsoft.com/blog/2014/08/13/migrate-on-premise-virtualized-workloads-to-azure-using-azure-site-recovery/). VMware has a similar offering via the [VMware vCloud Hybrid Service recovery-as-a-service offering](http://www.vmware.com/files/pdf/vchs/VMware-vCloud-Hybrid-Service-Disaster-Recovery-DS.pdf).

* The folks at eNovance have a write-up on [multi-tenant Docker with OpenStack Heat](http://techs.enovance.com/7104/multi-tenant-docker-with-openstack-heat). It's an interesting write-up, but not for the faint of heart---to make their example work, you'll need the latest builds of Heat and the Docker plugin (it doesn't work with the stable branch of Heat).

* Preston Bannister took [a look at cloud application backup in OpenStack](http://bannister.us/weblog/2014/08/21/cloud-application-backup-and-openstack/). His observations are, I think, rational and fair, and I'm glad to see someone paying attention to this topic (which, thus far, I think has been somewhat ignored).

* Interested in Docker and Kubernetes on Azure? See [here](http://msopentech.com/blog/2014/08/28/docker-containers-on-microsoft-azure-with-kubernetes-visualizer/) and [here](http://blog.azure.com/2014/08/28/hackathon-with-kubernetes-on-azure/) for more details.

* This article takes a look at Heat-Translator, an effort designed to provide some interoperability between TOSCA and OpenStack HOT documents for application deployment and orchestration. The portability of orchestration resources is one of several aspects you'll want to examine as you progress down the route of fully embracing a cloud computing operational model.

## Operating Systems/Applications

* Looks like we have another convert to Markdown---Anthony Burke recently talked about [how he uses Markdown](http://networkinferno.net/using-markdown-to-improve-your-life). Regular readers of this site know that I do almost _all_ of my content generation using MultiMarkdown (a variation of Markdown with some expanded syntax options). Here's a post I recently published on [some useful Markdown tools for OS X][1].

* Good to see that Ivan Pepelnjak thinks [infrastructure as code makes sense](http://blog.ipspace.net/2014/06/infrastructure-as-code-actually-makes.html). I guess that means the time I've spent with Puppet (you can browse Puppet-related posts [here][2]) wasn't a waste.

* I don't know if I've mentioned this before (sorry if that's the case), but I'm liking this "NIX4NetEng" series going on over at Nick Buraglio's site ([part 1](https://www.forwardingplane.net/2014/04/nix4neteng-1-managing-dotfiles-pwn-the-unspoken-pain-of-unix-administration/), [part 2](http://www.forwardingplane.net/2014/06/nix4neteng-2-ipv46-address-investigation-tools-whois-dig/), and [part 3](http://www.forwardingplane.net/2014/07/nix4neteng-3-ip-addressing-and-subnet-tools/)).

* Mike Foley has a blog post on how to go from [zero to Windows domain controller in only 4 reboots](http://www.yelof.com/2014/08/04/zero-to-windows-domain-controller-in-4-reboots/). Handy.

## Storage

* The OpenStack Manila project (for "filesystems as a service") was recently promoted out of Incubation. This NetApp Community post (NetApp has been a significant backer of the Manila project) shows [how to get OpenStack Manila running with DevStack and Vagrant](https://communities.netapp.com/community/netapp-blogs/the-raised-floor/blog/2014/08/22/get-openstack-manila-running-with-vagrant-and-devstack).

## Virtualization

* Running Hyper-V with Linux VMs? Ben Armstrong details what versions of Linux support the various Hyper-V features in [this post](http://blogs.msdn.com/b/virtual_pc_guy/archive/2014/06/16/what-version-of-linux-supports-what-in-hyper-v.aspx).

* Here's a quick write-up on [running VMs with VirtualBox 4.3 on a headless Ubuntu 14.04 LTS server](http://www.howtoforge.com/vboxheadless-running-virtual-machines-with-virtualbox-4.3-on-a-headless-ubuntu-14.04-lts-server).

* Nested OS X guest on top of nested ESXi on top of VMware Fusion? Must be something William Lam's tried. Go have a look at [his write-up](http://www.virtuallyghetto.com/2014/08/how-to-run-nested-mac-os-x-guest-on-nested-esxi-on-top-vmware-fusion.html).

* Here's a quick [update on Nova-Docker](http://blog-calfonso.rhcloud.com/?p=84), the effort in OpenStack to allow users to deploy Docker containers via Nova. I'm not yet convinced that treating Docker as a hypervisor in Nova is the right path, but we'll see how things develop.

* [This post](http://blog.oddbit.com/2014/08/11/four-ways-to-connect-a-docker/) is a nice write-up on the different ways to connect a Docker container to a local network.

* Weren't able to attend VMworld US in San Francisco last week? No worries. If you have access to the recorded VMworld sessions, check out Jason Boche's [list of the top 10 sessions](http://www.boche.net/blog/index.php/2014/08/28/vmworld-2014-top-ten/) for a priority list of what recordings to check out. Or need a recap of the week? See [here](http://www.vmguru.nl/2014/08/vmworld-2014-highlights/) (one of many recap posts, I'm sure).

That's it this time around; hopefully I was able to include something useful for you. As always, all courteous comments are welcome, so feel free to speak up in the comments. In particular, if there is a technology area that I'm not covering (or not covering well), please let me know---and suggestions for more content sources are certainly welcome!

[1]: {{< relref "2014-05-19-some-useful-markdown-tools-for-os-x.md" >}}
[2]: /tags/puppet/
