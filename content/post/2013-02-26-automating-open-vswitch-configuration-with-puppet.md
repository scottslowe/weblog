---
author: slowe
categories: Tutorial
comments: true
date: 2013-02-26T09:00:00Z
slug: automating-open-vswitch-configuration-with-puppet
tags:
- Automation
- Linux
- Networking
- OVS
- Puppet
- RedHat
title: Automating Open vSwitch Configuration with Puppet
url: /2013/02/26/automating-open-vswitch-configuration-with-puppet/
wordpress_id: 3100
---

In this post, I'm going to show you a way to automate the configuration of [Open vSwitch (OVS)](http://openvswitch.org) using [Puppet](http://www.puppetlabs.com). While this method will work, it is not without its drawbacks (which I'll explain later in the post). I freely admit that there might be better ways of accomplishing this task; this is one simple way that I discovered.

I wish I could say that this method of automating OVS with Puppet was clever, but---to be honest---it's really more of a brutish hack that anything else. In an earlier post, I described [some integrations between RHEL and OVS][1] that allows you to use interface configuration files in `/etc/sysconfig/network-scripts` to configure portions of OVS. For example, you could use this interface configuration file to create an OVS internal interface for management traffic:

``` text
DEVICE="mgmt0"
BOOTPROTO="static"
ONBOOT="yes"
DEVICETYPE="ovs"
TYPE="OVSIntPort"
IPADDR=10.11.12.13
NETMASK=255.255.255.0
OVS_BRIDGE="ovsbr0"
HOTPLUG="no"
```

Further, based on a number of the Puppet-related posts I've written ([this one][2], for example), you probably know that Puppet has the ability to enforce the presence and contents of file-based resources.

So, Puppet can create and manage files, and files can be used to configure OVS. You can probably see where this is going. That's right---if we use Puppet to manage interface configuration scripts, we can automate the configuration of OVS (only on systems running RHEL or RHEL variants, naturally).

Here's a snippet of Puppet code that could be used to automate the configuration of OVS to use an internal interface for management traffic:

``` puppet
# This code declares a file resource to manage an interface
# configuration script on RHEL/RHEL variants for automated
# configuration of OVS.
#
file {'/etc/sysconfig/network-scripts/ifcfg-mgmt0':
  ensure                =>  'present',
  source                =>  'puppet:///modules/module-name/ovs-ifcfg-mgmt0',
}
```

As I said, this is a bit of a brutish hack---not an elegant solution, but one that works. Naturally, because this builds on the RHEL-OVS integrations, you'll need to do an `ifdown` and/or `ifup` to make the change(s) effective. (One could likely use an `exec` statement in the Puppet manifest to run these commands for you.) Another drawback is that this only works on RHEL and RHEL variants, whereas both OVS and Puppet are far more widely supported. Still, it might come in handy in some situations.

If you have corrections, clarifications, or suggestions, please feel free to speak up in the comments below. Courteous comments are both encouraged and welcomed!


[1]: {{< relref "2013-02-07-exploring-rhel-ovs-integrations.md" >}}
[2]: {{< relref "2012-07-05-using-puppet-with-multiple-operating-systems.md" >}}
