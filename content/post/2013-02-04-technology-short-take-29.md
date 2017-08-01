---
author: slowe
categories: Information
comments: true
date: 2013-02-04T09:00:00Z
slug: technology-short-take-29
tags:
- Automation
- Cisco
- CLI
- Encryption
- HyperV
- KVM
- Linux
- Microsoft
- NetApp
- Networking
- OpenStack
- Puppet
- Security
- Storage
- UCS
- Virtualization
- VMware
title: 'Technology Short Take #29'
url: /2013/02/04/technology-short-take-29/
wordpress_id: 3082
---

Welcome to Technology Short Take #29! This is another installation in my irregularly-published series of links, thoughts, rants, and raves across various data center-related fields of technology. As always, I hope you find something useful here.

## Networking

* Who out there has played around with [Mininet](http://mininet.github.com/) yet? Looks like this is another tool I need to add to my toolbox as I continue to explore networking technologies like OpenFlow, Open vSwitch, and others.

* William Lam has a recent post on some [useful VXLAN commands found in ESXCLI](http://blogs.vmware.com/vsphere/2013/01/useful-vxlan-commands-in-esxcli-5-1.html) with vSphere 5.1. I'm a CLI fan, so I like this sort of stuff.

* I still have a lot to learn about OpenFlow and networking, but [this article](http://highscalability.com/blog/2012/6/4/openflowsdn-is-not-a-silver-bullet-for-network-scalability.html) from June of last year (it appears to have been written by Ivan Pepelnjak) discusses some of the potential scalability concerns around early versions of the OpenFlow protocol. In particular, the use of OpenFlow to perform granular per-flow control when there are thousands (or maybe only hundreds) of flows presents a scalability challenge (for now, at least). In my mind, this isn't an indictment of OpenFlow, but rather an indictment of the way that OpenFlow is being used. I think that's the point Ivan tried to make as well---it's the architecture and how OpenFlow is used that makes a difference. (Is that a reasonable summary, Ivan?)

* Brad Hedlund (who will [be my co-worker][1] starting on 2/11) created [a great explanation of network virtualization](http://bradhedlund.com/2013/01/28/network-virtualization-a-next-generation-modular-platform-for-the-virtual-network/) that clearly breaks down the components and explains their purpose and function. Great job, Brad.

* One of the things I like about Open vSwitch (OVS) is that it is so incredibly versatile. Case in point: here's a post on [using OVS to connect LXC containers running on different hosts](https://s3hh.wordpress.com/2012/05/28/connecting-containers-on-several-hosts-with-open-vswitch/) via GRE tunnels. Handy!

## Servers/Hardware

* Cisco UCS is pretty cool in that it makes automation of compute hardware easier through such abstractions as server profiles. Now, you can also [automate UCS with Chef](http://www.velankani.net/cisco-ucs-automation-with-opscode-chef/). I traded a few tweets with some Puppet folks, and they indicated they're looking at this as well.

* Speaking of Puppet and hardware, I also saw a mention on Twitter about a Puppet module that will manage the configuration of a NetApp filer. Does anyone have a URL with more information on that?

* Continuing the thread on configuration management systems running on non-compute hardware (I suppose this shouldn't be under the "Servers/Hardware" section any longer!), I also found references to running [CFEngine on network apliances](http://cfengine.com/network-appliances) and [running Chef on Arista](http://wiki.opscode.com/display/chef/Chef+on+Arista) switches. That's kind of cool. What kind of coolness would result from even greater integration between an SDN controller and a declarative configuration management tool? Hmmm

## Security

* Want full-disk encryption in Ubuntu, using AES-XTS-PLAIN64? Here's a [detailed write-up](https://57un.wordpress.com/2013/02/01/full-disk-encryption-using-ubuntu-in-most-secure-mode-with-aes-xts-plain64/) on how to do it.

* In posts and talks I've given about personal productivity, I've spoken about the need to minimize "friction," that unspoken drag that makes certain tasks or workflows more difficult and harder to adopt. Tal Klein has a great post on [how friction comes into play with security](http://blogs.bromium.com/2013/01/24/the-friction-affliction/) as well.

## Cloud Computing/Cloud Management

* If you, like me, are constantly on the search for more quality information on OpenStack and its components, then you'll probably find this post on [getting Cinder up and running](http://rconradharris.com/2013/01/14/getting-cinder-up-and-running.html) to be helpful. (I did, at least.)

* Mirantis---recently the recipient of $10 million in funding from various sources---posted a write-up in late November 2012 on [troubleshooting some DNS and DHCP service configuration issues](http://www.mirantis.com/blog/dns-dhcp-service-configuration-openstack-nova/) in OpenStack Nova. The post is a bit specific to work Mirantis did in integrating an InfoBlox appliance into OpenStack, but might be useful in other situation as well.

* I found [this article](http://goodsquishy.com/2012/12/introducing-openstack-packstack/) on Packstack, a tool used to transform Fedora 17/18, CentOS 6, or RHEL 6 servers into a working OpenStack deployment (Folsom). It seems to me that lots of people understand that getting an OpenStack cloud up and running is a bit more difficult than it should be, and are therefore focusing efforts on making it easier.

* DevStack is another proof point of the effort going into make it easier to get OpenStack up and running, although the focus for DevStack is on single-host development environments (typically virtual themselves). [Here's](http://www.scalegrid.net/blog/?p=11) one write-up on DevStack; [here's](http://openstack.prov12n.com/nesting-openstack-in-openstack/) another one by Cody Bunch, and yet [another one](http://networkstatic.net/openstack-quantum-devstack-on-a-laptop-with-vmware-fusion/) by the inimitable Brent Salisbury.

## Operating Systems/Applications

* If you're interested in learning Puppet, there are a great many resources out there; in fact, I've already mentioned many of them in previous posts. I recently came across [these Example42 Puppet Tutorials](http://www.example42.com/?q=Example42PuppetTutorials). I haven't had the chance to review them myself yet, but it looks like they might be a useful resource as well.

* Speaking of Puppet, the `puppet-lint` tool is very handy for ensuring that your Puppet manifest syntax is correct and follows the style guidelines. The tool has recently been updated to help _fix_ issues as well. Read [here](http://bombasticmonkey.com/2013/01/28/fix-simple-problems-with-puppet-lint/) for more information.

## Storage

* Greg Schulz (aka StorageIO) has a couple of VMware storage tips posts you might find useful reading. Part 1 is [here](http://storageioblog.com/?p=4172), part 2 is [here](http://storageioblog.com/?p=4180). Enjoy!

* Amar Kapadia suggests that [adding LTFS to Swift](http://www.buildcloudstorage.com/2012/08/cold-storage-using-openstack-swift-vs.html) might create an offering that could give AWS Glacier a real run for the money.

* Gluster interests me. Perhaps it shouldn't, but it does. For example, the idea of hosting VMs on Gluster (similar to the setup described [here](http://blog.jebpages.com/archives/fedora-17-openstack-and-gluster-3-3/)) seems quite interesting, and the work being done to [integrate KVM/QEMU with Gluster](http://www.gluster.org/2012/11/integration-with-kvmqemu/) also looks promising. If I can ever get my home lab into the right shape, I'm going to do some testing with this. Anyone done anything with Gluster?

* Erik Smith has a very informative write-up on [why FIP snooping is important when using FCoE](http://brasstacksblog.typepad.com/brass-tacks/2013/01/yet-another-reason-why-enabling-fip-snooping-is-important-when-using-fcoe.html).

* Via [this post](http://www.17od.com/2012/12/19/ten-useful-openstack-swift-features/) on ten useful OpenStack Swift features, I found [this page](http://docs.openstack.org/developer/swift/development_saio.html) on how to build the "Swift All in One," a useful VM for learning all about Swift.

## Virtualization

* There's no GUI for it, but it's kind of cool that you can indeed [create VM anti-affinity rules in Hyper-V](http://www.ravichaganti.com/blog/?p=2712) using PowerShell. This is another example of how Hyper-V continues to get more competent. Ignore Microsoft and Hyper-V at your own risk

* Frank Denneman takes a quick look at [using user-defined NetIOC network resource pools](http://frankdenneman.nl/2013/01/21/direct-ip-storage-and-using-netioc-user-defined-network-resource-pools-for-qos/) to isolate and protect IP-based storage traffic from within the guest (i.e., using NFS or iSCSI from within the guest OS, not through the VMkernel). Naturally, this technique could be used to "protect" or "enhance" other types of important traffic flows to/from your guest OS instances as well.

* Andre Leibovici has [a brief write-up](http://myvirtualcloud.net/?p=4210) on the PowerShell module for the Nicira Network Virtualization Platform (NVP). Interesting stuff

* This write-up by Falko Timme on [using BoxGrinder to create virtual appliances for KVM](http://www.howtoforge.com/creating-virtual-redhat-centos-scientific-linux-fedora-appliances-for-kvm-with-boxgrinder-fedora-17) was interesting. I might have to take a look at BoxGrinder and see what it's all about.

* In case you hadn't heard, OVF 2.0 has been announced/released by the DMTF. Winston Bumpus of VMware's Office of the CTO has more information in [this post](http://cto.vmware.com/ovf-2-0-is-here/). I also found the [OVF 2.0 frequently asked questions (FAQs)](http://dmtf.org/about/faq/ovf_faq) to be helpful. Of course, the real question is how long it will be before vendors add support for OVF 2.0, and how extensive that support actually is.

And that's it for this time around! Feel free to share your thoughts, suggestions, clarifications, or corrections in the comments below. I encourage your feedback, and thanks for reading.

[1]: {{< relref "2013-01-25-new-year-new-challenges-new-opportunities.md" >}}
