---
author: slowe
categories: Explanation
comments: true
date: 2013-12-18T09:00:00Z
slug: managing-open-vswitch-with-puppet
tags:
- Automation
- Libvirt
- Linux
- LXC
- Networking
- OVS
- Puppet
title: Managing Open vSwitch with Puppet
url: /2013/12/18/managing-open-vswitch-with-puppet/
wordpress_id: 3388
---

In this post, I'm going to show you how to manage [Open vSwitch (OVS)](http://openvswitch.org/) using the popular open source configuration management tool [Puppet](http://www.puppetlabs.com/). This is not the first time I've written about this topic; in the past I showed you how to [automate OVS configuration with Puppet][1] via a hack utilizing [some RHEL-OVS integrations][2]. This post, however, focuses on the use of an actual Puppet module that will manage the configuration of OVS, a much cleaner solution---in my view, at least---than leveraging the file-based integrations I discussed earlier.

The Puppet module I'll be using and discussing in this post is the L23Network module (found [here](https://github.com/xenolog/l23network) on GitHub). This is an extremely flexible and useful module, capable of not only configuring and managing network interfaces but also capable of managing the configuration of OVS. The latter functionality---managing the configuration of OVS---will be the primary focus of this article (with one exception).

The L23Network module is pretty well-documented, so I won't bother regurgitating the documentation here. Instead, I'll just try to provide some specific examples, and tie those examples back to some of the various OVS configurations I've shown you in earlier posts.

First, let's get "the one exception" I mentioned earlier out of the way. In OVS environments, you'll often need to bring up a physical interface without assigning that interface an IP address. For example, consider a physical interface that is providing bridged connectivity to guest domains (VMs) on an OVS bridge. You'll want the interface to be up, but the interface does not need an IP address. Using the L23Network module, you can accomplish that with this piece of code in your manifest:

    l23network::l3::ifconfig {'eth1': ipaddr => 'none'}

Now that `eth1` is up, you could create a bridge to which to attach it with this code:

    l23network::l2::bridge {'br-ex': }

And then you could actually attach `eth1` like this:

    l23network::l2::port {'eth1': bridge => 'br-ex'}

You could then provide multi-VLAN bridged connectivity to guest domains via libvirt as I explained in my post on [using VLANs with libvirt and OVS][3]. (Or, if you are [using LXC with libvirt and OVS][4], you could provide multi-VLAN bridged connectivity to containers.)

The L23Network module can also work with other types of interfaces, not just physical interfaces. Want to create an internal interface, perhaps to use as a tunnel endpoint for GRE tunnel as I described [here][5]? Use this snippet of Puppet code:

    l23network::l2::port {'tep0': bridge => 'br-tun', type => 'internal'}

You could then assign the newly-created `tep0` interface an IP address on your transport network like this:

    l23network::l3::ifconfig {'tep0': ipaddr => '10.1.1.1/24'}

(In theory, you could also use the L23Network module to create an internal interface so as to [run host management through OVS][6], but then you could run into issues communicating with the Puppet server over the same interfaces the Puppet server is configuring.)

I haven't yet used L23Network to create/manage patch ports or GRE ports, but the documentation indicates the module is capable of doing so. This is an area that I plan to explore in a bit more detail in the near future (in my copious free time).

Based on the snippets I've given you above, it should be pretty straightforward how to combine these various pieces together to fully configure and manage OVS instances across a large number of systems. However, if you have any questions, feel free to post them in the comments below. I also welcome all other courteous feedback; you are encouraged to start (or join) the conversation.

[1]: {{< relref "2013-02-26-automating-open-vswitch-configuration-with-puppet.md" >}}
[2]: {{< relref "2013-02-07-exploring-rhel-ovs-integrations.md" >}}
[3]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
[4]: {{< relref "2013-12-03-connecting-lxc-to-open-vswitch-using-libvirt.md" >}}
[5]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[6]: {{< relref "2012-10-30-running-host-management-on-open-vswitch.md" >}}
