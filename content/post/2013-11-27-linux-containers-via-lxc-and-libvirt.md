---
author: slowe
categories: Tutorial
comments: true
date: 2013-11-27T09:00:00Z
slug: linux-containers-via-lxc-and-libvirt
tags:
- CLI
- Libvirt
- Linux
- LXC
- Networking
- OVS
- Virtualization
title: Linux Containers via LXC and Libvirt
url: /2013/11/27/linux-containers-via-lxc-and-libvirt/
wordpress_id: 3349
---

One of the cool things about [libvirt](http://libvirt.org/) is the ability to work with multiple hypervisors and virtualization technologies, including Linux containers using [LXC](http://linuxcontainers.org/). In this post, I'm going to show you how to use libvirt with LXC, including leveraging libvirt to help automate attaching containers to [Open vSwitch (OVS)](http://openvswitch.org/).

If you aren't familiar with Linux containers and LXC, I invite you to have a look at [my introductory post on Linux containers and LXC][1]. It should give you enough background to make this post make sense.

To use libvirt with an LXC container, there are a couple of basic steps:

1. Create the container using standard LXC user-space tools.

2. Create a libvirt XML definition for the container.

3. Define the libvirt container domain.

4. Start the libvirt container domain.

The first part, creating the container, is pretty straightforward:

    lxc-create -t ubuntu -n cn-02

This creates a container using the Ubuntu template and calls it `cn-01`. As you may recall from [my introductory LXC post][1], this creates the container's configuration and root filesystem in `/var/lib/lxc` by default. (I'm assuming you are using Ubuntu 12.04 LTS, as I am.)

Once you have the container created, you next need to get it into libvirt. Libvirt uses a standard XML-based format for defining VMs, containers, networks, etc. At first, I thought this might be the most difficult section, but thanks to [this page](https://wiki.ubuntu.com/SergeHallyn_libvirtlxc) I was able to create a template XML configuration. I've uploaded this template XML configuration to GitHub as a gist, which you can view [here](https://gist.github.com/scottslowe/7647800).

Simply take this XML template and save it as something like `lxc-template.xml` or similar. Then, after you've created your container using `lxc-create` as above, you can easily take this template and turn it into a specific container configuration with only one command. For example, suppose you created a container named "cn-02" (as I did with the command I showed earlier). If you wanted to customize the XML template, just use this simple Unix/Linux command:

    sed 's/REPLACE/cn-02/g' lxc-template.xml > cn-02.xml

Once you have a container-specific libvirt XML configuration, then defining it in libvirt is super-easy:

    virsh -c lxc:// define cn-02.xml

Then start the container:

    virsh -c lxc:// start cn-02

And connect to the container's console:

    virsh -c lxc:// console cn-02

When you're done with the container's console, press "Ctrl-]" (that's Control and right bracket at the same time); that will return you to your host.

Pretty handy, eh? Further, since you're now controlling your containers via libvirt, you can leverage libvirt's networking functionality as well---which means that you can easily create libvirt virtual networks backed by OVS and automatically attach containers to OVS for advanced networking configurations. You only need to create an OVS-backed virtual network like I describe in [this post on VLANs with OVS and libvirt][2].

I still need to do some additional investigation and testing to see how the networking configuration in the container's `config` file interacts with the networking configuration in the libvirt XML file. For example, how do you define multiple network interfaces? Can you control the name of the veth pairs that show up in the host? I don't have any answers for these questions (yet). If you know the answers, feel free to speak up in the comments!

All courteous feedback and interaction is welcome, so I invite you to start (or join) the discussion via the comments below.

[1]: {{< relref "2013-11-25-a-brief-introduction-to-linux-containers-with-lxc.md" >}}
[2]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
