---
author: slowe
categories: Explanation
comments: true
date: 2015-06-25T09:55:00Z
tags:
- Linux
- CLI
- Networking
- VLAN
- Cumulus
title: VLAN Trunking with Cumulus Linux
url: /2015/06/25/vlan-trunking-cumulus-linux/
---

Following up on [my earlier post on Cumulus Linux networking concepts][xref-1], I wanted to build on that information with a guide on configuring VLAN trunking. This would be useful in a number of different scenarios: supporting multiple (VLAN-backed) port groups on [vSphere][link-1] hosts, or connecting an [Open vSwitch (OVS)][link-2] bridge on a KVM or Xen hypervisor to multiple VLANs. You might also need to use a VLAN trunking configuration to connect a Cumulus Linux-powered switch to another switch.

For this configuration, I'm going to use the new _VLAN-aware_ bridging functionality introduced in Cumulus Linux 2.5. There are two pieces involved in making this work:

1. The configuration for VLAN-aware bridge itself
2. The configuration for the individual port(s)

Let's look at each of these pieces individually.

## The VLAN-Aware Bridge

In order to provide layer 2 (switched) connectivity between front-panel ports on a Cumulus Linux-powered switch, the ports have to be part of a bridge. In this case, we'll create a _VLAN-aware_ bridge, which simplifies the configuration (in my opinion). It's a bit less "true" to the Linux way of doing things, but simpler.

Owing to its Debian roots, you'll configure the bridge by either adding a stanza to `/etc/network/interfaces` or by placing a configuration file in `/etc/network/interfaces.d`. I tend to prefer the latter approach, and here's a sample file that illustrates how to create and configure the bridge:

```
auto bridge
iface bridge
  bridge-vlan-aware yes
  bridge-ports swp1 swp3 swp4 swp5 swp8 swp10 swp16s1
  bridge-vids 3 4 6-10
  bridge-pvid 1
  bridge-stp on
  mstpctl-treeprio 20480
```

Here's a brief description of the configuration parameters illustrated here:

* The `bridge-vlan-aware` statement is what makes the bridge "VLAN-aware", naturally. You should have only a single VLAN-aware bridge per switch.
* The `bridge-ports` statement lists the front panel ports that are part of this bridge. Note that you could also use `bridge-ports glob`, which would allow you to specify a range of ports instead of listing them individually. The use of `bridge-ports glob` to include all the front panel ports in single VLAN-aware bridge is probably the typical way one would configure the switch.
* One quick side note regarding ports: the "swp16s1" listed above represents a 40GbE port that is being divided into four 10GbE ports via a breakout cable.
* The `bridge-pvid` statement specifies the native (untagged) VLAN. If the native VLAN is 1, this statement isn't required.
* `bridge-vids` declares the VLANs for the bridge (in this case, VLANs 3, 4, and 6 through 10).
* STP (RSTP, specifically) is activated on this bridge using the `bridge-stp on` statement.
* Finally, the `mstpctl-treeprio` statement sets the bridge tree root priority (must be a multiple of 4096).

By default, ports that are included in a bridge (via the `bridge-ports` statement) will inherit the VLAN IDs defined on the bridge (via the `bridge-pvid` and `bridge-vids` statements). However, it may be necessary to modify the behavior/configuration of individual ports within the bridge, so continue to the next section for more information.

## Individual Port Configuration

If you don't specify otherwise, ports included in a VLAN-aware bridge will automatically be VLAN trunks, carrying tagged traffic for all the VLANs specified for the bridge (the native VLAN will, of course, be untagged). By modifying the configuration of individual ports within the bridge, you can change the native VLAN for that port as well as prune VLANs (limit the VLANs carried on the trunk).

As before, both of these tasks are handled by making modifications to the appropriate configuration stanza in `/etc/network/interfaces` or modifications to the appropriate file in `/etc/network/interfaces.d/`:

* To change the native VLAN for that specific port, add a `bridge-pvid` statement to that port's configuration file.
* To prune (limit) VLANs on a trunk, add a `bridge-vids` statement to that port's configuration file/stanza that lists the VLAN IDs that are allowed across that port.

For example, if a port that is a member of the bridge described earlier needed to use a different native VLAN, you'd need to add `bridge-pvid 2` (or whatever value is needed) to the configuration file/stanza for that particular port. Similarly, if you needed to prune (limit) VLANs on a port that was a member of the example bridge described earlier in this post, you'd add `bridge-vids 4 6 8` to that port's configuration file/stanza (which would only allow tagged traffic on VLANs 4, 6, and 8 to traverse that port).

And that's it!

Hopefully this was somewhat helpful. I have more posts queued up, so please stay tuned. In the meantime, feel free to contact me via Twitter if you have any questions about this article. Thanks for reading!

[link-1]: http://www.vmware.com/products/vsphere/
[link-2]: http://openvswitch.org
[xref-1]: {{< relref "2015-06-01-cumulus-networking-concepts.md" >}}
