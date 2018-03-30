---
author: slowe
categories: Tutorial
comments: true
date: 2014-01-23T09:00:00Z
slug: automatically-connecting-lxc-to-open-vswitch
tags:
- Linux
- LXC
- OVS
- Ubuntu
- Networking
- Virtualization
title: Automatically Connecting LXC to Open vSwitch
url: /2014/01/23/automatically-connecting-lxc-to-open-vswitch/
wordpress_id: 3398
---

I've previously discussed using [Open vSwitch (OVS)](http://openvswitch.org/) with [Linux Containers (LXC)](http://linuxcontainers.org/) in a couple of previous posts ([here][1] and [here][2]). In this post, I'm going to show you one way to have your containers automatically connected to OVS on startup without having to use [libvirt](http://libvirt.org/).

I tested this configuration using Ubuntu 12.04 with the Linux 3.8.0 kernel and an alpha release of LXC 1.0.0 from the `precise-backports` repository. The version of LXC in the 12.04 repositories (version 0.7.5, if I recall correctly) isn't new enough to support the specific feature I'm describing here, so plan accordingly.

If you aren't familiar with LXC, I'd suggest you first read [my LXC introductory post][3]. You'll probably also find the post on [using LXC, OVS, and GRE tunnels][1] useful, as some of the information there is applicable here also.

Ready? Let's get started.

## Configuring the Container

You can create your container using the standard LXC tools:

    lxc-create -n cn-01 -t ubuntu

As you may already know, this will create a container named "cn-01" based on the Ubuntu template. The configuration for this container will be found, by default, at `/var/lib/lxc/cn-01/config`. By default, unless you've changed the configuration of your system, this container will be configured to use virtual Ethernet network interfaces and be attached to the default LXC bridge.

The changes required to make the container connect to OVS are, fortunately, quite minimal. First, remove or comment out the `lxc.network.link` line. This is the configuration parameter that causes the container to attach to the default LXC bridge (normally called "lxcbr0").

Next, add a configuration line to run a script after creating the network interfaces. In my examples here, I'll assume the script is called "ovsup" and is stored in the `/etc/lxc/` directory. The configuration parameter should look something like this:

    lxc.network.script.up = /etc/lxc/ovsup

(Note that there is also a corresponding `lxc.network.script.down` configuration parameter, but I won't be using it in this example.)

Once you've made these changes to the container's configuration, then you're ready to create the actual script.

## Creating the Network Attachment Script

Your script---the one referenced on the `lxc.network.script.up` in the container's configuration file---should look something like this:

```
#!/bin/bash

BRIDGE="br-int"
ovs-vsctl --may-exist add-br $BRIDGE
ovs-vsctl --if-exists del-port $BRIDGE $5
ovs-vsctl --may-exist add-port $BRIDGE $5
```

(Please click [here](https://gist.github.com/scottslowe/8569424) to see this as a GitHub Gist.)

LXC passes five parameters to the script when it is called:

1. The name of the container

2. The configuration section of the container's configuration ("net" in this case)

3. Either "up" or "down", depending on which configuration option is calling the script (`lxc.network.script.up` passes "up", `lxc.network.script.down` passes "down")

4. The type of networking ("veth" in this case)

5. The name of the interface (randomly generated unless you have included `lxc.network.veth.peer` in the container's configuration)

This simple script doesn't really need anything other than the interface name, so it only uses parameter 5 (the `$5` in the script). The script first ensures that the appropriate OVS bridge exists (creating it if necessary), then deletes the interface from the OVS bridge (if it exists) and adds it back to the OVS bridge.

(Note: If you are using the `lxc.network.script.down` configuration parameter, you could eliminate the line to delete the port from the OVS bridge and place it in the down script instead. Or, you could write logic into the script to see if "down" is being called and delete the port. There are a variety of ways to approach the situation.)

Using this configuration, when you start the container the host-side virtual Ethernet interface created by LXC will be automatically added to OVS, and your container will have whatever network connectivity is dictated by the OVS configuration. This could include tunneled connectivity (as described [here][1]) or bridged connectivity.

If you have any questions, feedback, or corrections, please feel free to speak up in the comments below. I encourage reader interaction!

[1]: {{< relref "2013-11-26-lxc-open-vswitch-and-gre-tunnels.md" >}}
[2]: {{< relref "2013-12-03-connecting-lxc-to-open-vswitch-using-libvirt.md" >}}
[3]: {{< relref "2013-11-25-a-brief-introduction-to-linux-containers-with-lxc.md" >}}
