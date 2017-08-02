---
author: slowe
categories: Information
comments: true
date: 2013-10-23T09:00:00Z
slug: technology-short-take-36
tags:
- Automation
- CLI
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
- VMware
title: 'Technology Short Take #36'
url: /2013/10/23/technology-short-take-36/
wordpress_id: 3315
---

Welcome to Technology Short Take #36. In this episode, I'll share a variety of links from around the web, along with some random thoughts and ideas along the way. I try to keep things related to the key technology areas you'll see in today's data centers, though I do stray from time to time. In any case, enough with the introduction---bring on the content! I hope you find something useful.

## Networking

* This post is a bit older, but still useful in the event if you're interested in learning more about OpenFlow and OpenFlow controllers. Nick Buraglio has put together [a basic reference OpenFlow controller VM](http://www.forwardingplane.net/2013/04/basic-reference-openflow-controller-vms-running-in-centos-6-for-kvm/)--this is a KVM guest with CentOS 6.3 with the Floodlight open source controller.

* Paul Fries takes on [defining SDN](http://www.poppingclouds.com/2013/10/09/can-someone-please-define-sdn/), breaking it down into two "flavors": host dominant and network dominant. This is a reasonable way of grouping the various approaches to SDN (using SDN in the very loose industry sense, not the original control plane-data plane separation sense). I'd like to add to Paul's analysis that it's important to understand that, in reality, host dominant and network dominant systems _can_ coexist. It's not at all unreasonable to think that you might have a fabric controller that is responsible for managing/optimizing traffic flows across the physical transport network/fabric, and an overlay controller---like VMware NSX---that integrates tightly with the hypervisor(s) and workloads running on those hypervisors to create and manage logical connectivity and logical network services.

* This is an older post from April 2013, but still useful, I think. In his article titled "[OpenFlow Test Deployment Options](http://www.networkcomputing.com/data-networking-management/openflow-test-deployment-options/240152428)", Brent Salisbury---a rock star new breed network engineer emerging in the new world of SDN---discusses some practical deployment strategies for deploying OpenFlow into an existing network topology. One key statement that I really liked from this article was this one: "SDN does not represent the end of networking as we know it. More than ever, talented operators, engineers and architects will be required to shape the future of networking." New technologies don't make talented folks who embrace change obsolete; if anything, these new technologies make them more valuable.

* Great post by Ivan (is there a post by Ivan that _isn't_ great?) on [flow table explosion with OpenFlow](http://blog.ipspace.net/2013/10/flow-table-explosion-with-openflow-10.html). He does a great job of explaining how OpenFlow works and why OpenFlow 1.3 is needed in order to see broader adoption of OpenFlow.

## Servers/Hardware

* Intel announced the E5 2600 v2 series of CPUs back at Intel Developer Forum (IDF) 2013 (you can follow my IDF 2013 coverage by looking at [posts with the IDF2013 tag][1]). Kevin Houston followed up on that announcement with a useful post on [vSphere compatibility with the E5 2600 v2](http://bladesmadesimple.com/2013/09/intel-e5-2600-v2-vsphere-compatibility/). You can also get [more details on the E5 2600 v2](http://bladesmadesimple.com/2013/09/intel-unveils-12-core-xeon-cpu/) itself in this related post by Kevin as well. (Although I'm just now catching Kevin's posts, they were published almost immediately after the Intel announcements---thanks for the promptness, Kevin!)

## Security

Nothing this time around, but I'll keep my eyes posted for content to share with you in future posts.

## Cloud Computing/Cloud Management

* There's a great post by John Allspaw over at Kitchen Soap titled [Learning from Failure at Etsy](http://www.kitchensoap.com/2013/09/30/learning-from-failure-at-etsy/). In my humble opinion, this is well worth reading. The idea of Blameless Post-Mortems and a Just Culture sound like things that lots of organizations could (and probably should) put into place.

* I'm sure it's like this with other tools, but I'm kind of awed by the tremendous flexibility of Puppet. Using [this Augeas-based providers module](http://forge.puppetlabs.com/domcleal/augeasproviders) on Puppet Forge, you can actually manage individual settings within the SSH configuration file (such as PermitRoot or AllowUsers), individual settings for syslog/rsyslog, and even sysctl parameters. Nice! Puppet code examples are available [here](http://augeasproviders.com/documentation/examples.html).

* Kenneth Hui---with whom I have the privilege of co-presenting at the OpenStack Summit in Hong Kong in less than a month---has a comprehensive write-up on [using Vagrant and Chef to build an HA OpenStack installation on your laptop](http://cloudarchitectmusings.com/2013/10/07/deploying-a-ha-openstack-cloud-on-your-laptop/). (That's assuming your laptop is beefy enough, of course.)

* Here's another awesome post by Cody Bunch, this time showing [how to use Vagrant, Hiera, and Puppet to turn up OpenStack](http://openstack.prov12n.com/puppet-openstack-hiera-with-vagrant/).

* What makes up a hybrid cloud? Read some of [Chris Colotti's thoughts on the matter](http://www.chriscolotti.us/vmware/hybrid-cloud-vmware/what-makes-a-cloud-a-hybrid-cloud/).

## Operating Systems/Applications

* I found this refresher on [some of the most useful apt-get/apt-cache commands](http://www.tecmint.com/useful-basic-commands-of-apt-get-and-apt-cache-for-package-management/) to be helpful. I don't use some of them on a regular basis, and so it's hard to remember the specific command and/or syntax when you do need one of these commands.

* I wouldn't have initially considered comparing [Docker](http://www.docker.io/) and [Chef](http://www.opscode.com/chef/), but considering that I'm not an expert in either technology it could just be my limited understanding. However, this post on [why Docker and why not Chef](http://blog.relateiq.com/why-docker-why-not-chef/) does a good job of looking at ways that Docker could potentially replace certain uses for Chef. Personally, I tend to lean toward the author's final conclusions that it is entirely possible that we'll see Docker and Chef being used together. However, as I stated, I'm not an expert in either technology, so my view may be incorrect. (I reserve the right to revise my view in the future.)

## Storage

* Using Dell EqualLogic with VMFS? Better read [this heads-up](http://cormachogan.com/2013/10/07/heads-up-dell-equallogic-vmfs-issue/) from Cormac Hogan and take the recommended action right away.

* Erwin van Londen proposes some ideas for [enhancing FC error detection and notification](http://erwinvanlonden.net/2013/08/closing-the-fibre-channel-resiliency-gap/) with the idea of making hosts more aware of path errors and able to "route" around them. It's interesting stuff; as Erwin points out, though, even if the T11 accepted the proposal it would be a while before this capability showed up in actual products.

## Virtualization

* [Libguestfs](http://libguestfs.org/) is an interesting project, and in the 1.24 release they added a tool called [virt-builder](https://rwmj.wordpress.com/2013/10/05/new-tool-virt-builder/) that helps quickly and easily deploy VM images.

* Andre Leibovici is well-known for his insightful and informative coverage of Horizon View and related products/technologies, and rightfully so. As proof of that, I recently came across two articles by Andre, one on [why CBRC is so important for Horizon View and VSAN](http://feedproxy.google.com/~r/Myvirtualcloudnet/~3/ngMimGrNhlc/), and a second on [how VSAN helps Horizon View](http://myvirtualcloud.net/?p=5425). Both of these are definitely worth reading if you want a bit more detail on how one of VMware's newest technologies, VSAN, is going to impact the end-user computing (EUC) space.

* Speaking of VSAN, William Lam has a very useful article on the [additional steps that are required to completely disable VSAN on an ESXi host](http://www.virtuallyghetto.com/2013/09/additional-steps-required-to-completely.html). I indicate that this is useful because I anticipate many folks will want to try out VSAN in their lab first. Since you will likely then want to take it out of the lab and move it into production after the appropriate amount of testing and validation for your environment, this post is quite helpful.

* It's kind of funny: I was doing a bit of reading on [ZeroVM](http://zerovm.org/), a new open source lightweight hypervisor, trying to understand where it fits in the overall virtualization space. The very next day, I see [the announcement that ZeroVM is being acquired by Rackspace](http://www.rackspace.com/blog/zerovm-smaller-lighter-faster/). Anyone want to take guesses on when we'll see ZeroVM support in OpenStack?

* Nice write-up by Gabrie Van Zanten on a [potential connection issue with the VCSA in vSphere 5.5](http://www.gabesvirtualworld.com/cant-establish-connection-server-7331/).

* See this post by Ben Armstrong on [faster live migration](http://blogs.msdn.com/b/virtual_pc_guy/archive/2013/10/21/faster-live-migration-in-windows-server-2012-r2.aspx) in Hyper-V on Windows Server 2012 R2 (which is now [generally available](http://blogs.technet.com/b/windowsserver/archive/2013/10/18/announcing-the-general-availability-of-windows-server-2012-r2-the-heart-of-cloud-os.aspx)).

That's it for this time around, but feel free to continue to conversation in the comments below. If you have any additional information to share regarding any of the topics I've mentioned, please take the time to add that information in the comments. Courteous comments are always welcome!

[1]: /tags/idf2013/
