---
author: slowe
categories: Tutorial
comments: true
date: 2013-12-03T09:00:00Z
slug: connecting-lxc-to-open-vswitch-using-libvirt
tags:
- Libvirt
- Linux
- LXC
- Networking
- OVS
- Virtualization
title: Connecting LXC to Open vSwitch Using Libvirt
url: /2013/12/03/connecting-lxc-to-open-vswitch-using-libvirt/
wordpress_id: 3369
---

In this post I'm going to expand a little bit on using [libvirt](http://libvirt.org/) to connect Linux containers (created using [LXC](http://linuxcontainers.org/)) to [Open vSwitch (OVS)](http://openvswitch.org/). I made brief mention of this in my post on [using LXC with libvirt][1], but did not provide any details. This post aims to provide those details.

I'm assuming that you're already familiar with LXC, OVS, and libvirt. If you aren't familiar with these projects, I suggest you have a look back at other articles I've written about them in the past. One of the easiest ways to do that is to browse articles [tagged LXC](/tags/lxc/), [tagged OVS](/tags/ovs/), and/or [tagged Libvirt](/tags/libvirt/). Further, I'm using Ubuntu 12.04 LTS in my environment, so if you're using another Linux distribution please note that some commands and/or package names might be different.

The basic process for connecting a Linux container to OVS using libvirt looks something like this:

1. Create one or more virtual networks in libvirt to "front-end" OVS.

2. Create your container(s) using standard LXC user-space tools.

3. Create libvirt XML definitions for your container(s).

4. Start the container(s) using `virsh`.

Steps 2, 3, and 4 were covered in my previous post on using LXC and libvirt, so I won't repeat them here. Step 1 is the focus here. (If you are a long-time reader and/or well-versed with libvirt and OVS, there isn't a great deal of new information here; I just wanted to present it in the context of LXC for the sake of completeness.)

To create a libvirt virtual network to front-end OVS, you need to create an XML definition that you can use with `virsh` to define the virtual network. Here's an example XML definition (click [here](https://gist.github.com/scottslowe/7748398) for an option to download this snippet):

``` xml
<network>
  <name>bridged</name>
  <uuid>ff5f8075-31a7-4cc9-21bf-5c8d144f58f0</uuid>
  <forward mode='bridge'/>
  <bridge name='br-ex' />
  <virtualport type='openvswitch'/>
  <portgroup name='untagged' default='yes'>
  </portgroup>
</network>
```

A few notes about this XML definition:

* You normally wouldn't include the UUID, as that is generated automatically by libvirt. If you were using this XML to create the virtual network from scratch, I would recommend just deleting the UUID line.

* The network is named "bridged", and points to the OVS bridge named `br-ex`. In this particular case, `br-ex` is a simple OVS bridge that contains a single physical interface.

* This particular virtual network only has a single portgroup configured for untagged traffic. If you wanted to provide a virtual network that supported multiple VLANs, you could add more portgroups with the VLAN tags as I describe in my post on [using VLANs with OVS and libvirt][2]. You'd then modify the container's XML definition to point to the appropriate portgroup, and in this way you could easily support running multiple containers across multiple VLANs on a single host.

* A libvirt virtual network can only point to a single bridge, so if you wanted to support both bridged (as shown here) as well as tunneled connectivity (perhaps as described in [my post on LXC, OVS, and GRE tunnels][3]), you would need to create a second XML definition that creates a separate virtual network. You could then modify the container's XML definition to point to the new network you just created.

In the bullets above, I mentioned modifying the container's XML definition. In particular, I'm referring to the `<interface type='network'>` portion of the container's XML definition. To use a libvirt network for a container's network connectivity, you'd specify `<source network='bridged'/>` (replacing "bridged" with whatever the name of your virtual network is; I'm using the name provided in the sample XML code above). For multiple interfaces in the container, simply supply multiple `<interface type='network'>` entries in the container's XML definition, and configure the source network for each of them appropriately.

Hopefully this post provides some additional details and information on using libvirt to connect Linux containers to OVS. If you have any questions, or if you have more information to share on this topic, please feel free to speak up in the comments below. I encourage and welcome all courteous feedback!

[1]: {{< relref "2013-11-27-linux-containers-via-lxc-and-libvirt.md" >}}
[2]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
[3]: {{< relref "2013-11-26-lxc-open-vswitch-and-gre-tunnels.md" >}}
