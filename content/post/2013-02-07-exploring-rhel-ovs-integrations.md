---
author: slowe
categories: Explanation
comments: true
date: 2013-02-07T15:30:16Z
slug: exploring-rhel-ovs-integrations
tags:
- Linux
- Networking
- OVS
title: Exploring RHEL-OVS Integrations
url: /2013/02/07/exploring-rhel-ovs-integrations/
wordpress_id: 3088
---

In this post, I'm going to explore some integrations between Red Hat Enterprise Linux (RHEL)/RHEL variants and Open vSwitch (OVS). This post will lay the foundation for a future post describing OVS automation using Puppet.

Over the past few weeks, I've been rebuilding my home lab to focus on some core projects/technologies for the upcoming year (more on that [in this post][1]). As part of the rebuild effort, I've rebuilt my hosts to run CentOS 6.3 x64, KVM, Libvirt, GlusterFS, and OVS, and have crafted Puppet code to help install (and configure, in some cases) most of these components. This post, as with some others, is an offshoot of the lab rebuild work.

(In case you were wondering, the other posts that have come out of the home lab rebuild project include [building Libvirt RPMs][2], [using Mock to build Sanlock RPMs][3], [using Mock to build libssh2 RPMs][4], [using Mock to build Libvirt RPMs][5], and [using Puppet for user accounts and configuration files][6]. I'm sure there will be more.)

The information in this post will build upon some base OVS knowledge, so if you aren't familiar with OVS, then you might want to go back and read some of my earlier OVS posts (these will at least get you started):

[Some Insight into Open vSwitch Configuration][7]  

[Link Aggregation and LACP with Open vSwitch][8]  

[VLANs with Open vSwitch Fake Bridges][9]  

[Running Host Management on Open vSwitch][10]  

[Layer 3 Routing with Open vSwitch][11]

Finally, before we get into the real meat of the information, I'd like to point out that the information in this post builds upon the information already included in the OVS documentation (see the `README.RHEL` file in the distribution tarball). I'm merely attempting to provide some additional context and examples on how these can be used.

Now that the preliminaries are out of the way...

The basic gist of the RHEL-OVS integration is support for the use of the scripts in `/etc/sysconfig/network-scripts` to do some configuration of OVS itself. For example, creating bridges, establishing bonds, configuring internal ports---all these tasks can be done using an `ifcfg-<name>` script.

Let's start with a very basic example. Let's say that you want to create a single OVS bridge named `ovsbr0`. To do that, you'd create a file named `ifcfg-ovsbr0` in the `/etc/sysconfig/network-scripts` directory, and make the contents look like this:

    DEVICE="ovsbr0"
    ONBOOT="yes"
    DEVICETYPE="ovs"
    TYPE="OVSBridge"
    BOOTPROTO="none"
    HOTPLUG="no"

Now let's say that you want to create a link aggregate on that bridge. To create a link aggregate on `ovsbr0` (the bridge you just created), create a file named `ifcfg-bond0` in the same directory, and make its contents look like this:

    DEVICE="bond0"
    ONBOOT="yes"
    DEVICETYPE="ovs"
    TYPE="OVSBond"
    OVS_BRIDGE="ovsbr0"
    BOOTPROTO="none"
    BOND_IFACES="eth0 eth1"
    OVS_OPTIONS="bond_mode=balance-tcp lacp=active"
    HOTPLUG="no"

Note that this is an LACP-enabled link aggregate, so you'll also have to configure your physical switch appropriately.

Finally, suppose that you want to create an internal interface (it will appear as a physical interface to Linux, but is hosted on OVS) across which you can run your management traffic. No problem! Just create a file named `ifcfg-mgmt0` and make the contents of the file look like this:

    DEVICE="mgmt0"
    BOOTPROTO="static"
    ONBOOT="yes"
    DEVICETYPE="ovs"
    TYPE="OVSIntPort"
    IPADDR=10.11.12.13
    NETMASK=255.255.255.0
    OVS_BRIDGE="ovsbr0"
    HOTPLUG="no"

Each of these scripts will create the corresponding structures/configurations within OVS. **There is a drawback, however.** In order for these changes to take effect, you must restart the network (perform a `service network restart`). This doesn't appear to be an OVS limitation of any sort; if you've read any of the other OVS posts, you know that you can make these changes live via the `ovs-vsctl` command. Instead, it simply appears to be a limitation of the fact that these scripts are only evaluated during a network stop/start event.

Once these scripts have been evaluated, though, you should end up with a configuration that looks something like this (UUIDs have been changed to protect the innocent):

``` text
542de17b-4eb5-4eff-f736-3c760e40dff3
    Bridge "ovsbr0"
        Port "mgmt0"
            Interface "mgmt0"
                type: internal
        Port "ovsbr0"
            Interface "ovsbr0"
                type: internal
        Port "bond0"
            Interface "eth0"
            Interface "eth1"
    ovs_version: "1.7.1"
```

Given the limitation, I can imagine that a natural question to ask would be, "Why use this integration, which requires a network restart, when you could just make the configuration changes yourself?" Excellent question. I see this as a way of establishing a "baseline" configuration for OVS, upon which you could (dynamically) add all the other configurations you need. In addition, because the base OVS configuration exists as a set of files, this opens up some other interesting possibilities. I'll explore those possibilities in a future post.

As always, courteous comments are welcome, so feel free to add your questions, thoughts, corrections, or clarifications in the comments below.


[1]: {{< relref "2013-02-07-looking-ahead-my-2013-projects.md" >}}
[2]: {{< relref "2013-01-16-building-libvirt-1-0-1-rpms-for-centos-6-3.md" >}}
[3]: {{< relref "2013-01-22-using-mock-to-build-sanlock-2-4-rpms-for-centos-6-3.md" >}}
[4]: {{< relref "2013-01-22-using-mock-to-build-libssh2-1-4-1-rpms-for-centos-6-3.md" >}}
[5]: {{< relref "2013-01-22-using-mock-to-build-libvirt-1-0-1-rpms-for-centos-6-3.md" >}}
[6]: {{< relref "2013-01-29-puppet-user-accounts-and-configuration-files.md" >}}
[7]: {{< relref "2012-10-04-some-insight-into-open-vswitch-configuration.md" >}}
[8]: {{< relref "2012-10-19-link-aggregation-and-lacp-with-open-vswitch.md" >}}
[9]: {{< relref "2012-10-19-vlans-with-open-vswitch-fake-bridges.md" >}}
[10]: {{< relref "2012-10-30-running-host-management-on-open-vswitch.md" >}}
[11]: {{< relref "2012-10-31-layer-3-routing-with-open-vswitch.md" >}}
