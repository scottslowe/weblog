---
author: slowe
categories: Tutorial
comments: true
date: 2013-11-26T09:00:00Z
slug: lxc-open-vswitch-and-gre-tunnels
tags:
- GRE
- Linux
- LXC
- Networking
- OVS
- Virtualization
title: LXC, Open vSwitch, and GRE Tunnels
url: /2013/11/26/lxc-open-vswitch-and-gre-tunnels/
wordpress_id: 3338
---

This post will show you how to use [Open vSwitch (OVS)](http://openvswitch.org/) to connect LXC containers on different hosts together using GRE tunnels. This process is quite similar to [using OVS and GRE tunnels to connect VMs][1], and even has some similarity to [connecting network namespaces over a GRE tunnel][2].

If you're not familiar with LXC, my [introductory post on LXC][3] might be useful. I also found this post on [networking with LXC](http://containerops.org/2013/11/19/lxc-networking/) to be extremely helpful, as was this [overview of LXC on Ubuntu 12.04](http://www.stgraber.org/2012/05/04/lxc-in-ubuntu-12-04-lts/). As you might guess, I used Ubuntu 12.04.3 LTS for my testing; note that if you are using a different distribution, the specific commands and/or file locations may differ.

At a high level, the process for creating this configuration looks like this:

* Create your containers using virtual Ethernet (veth) networking.

* Create a OVS bridge that does not contain any physical interfaces and attach a GRE interface configured to connect to a remote container host.

* Attach the containers' veth interfaces to this new OVS bridge.

Let's take a look at each of these steps in a bit more detail.

## Create the Containers

My [LXC introduction post][3] covers this in a fair amount of detail, but I'll walk you through the steps again just for the sake of completeness. Additionally, I wanted to use containers with two separate interfaces: a "public" interface that could reach the outside world, and a "private" interface for inter-container communications.

To create the containers, you can simply use `lxc-create` to create the basic container. For example, if you are also using Ubuntu 12.04 64-bit for your testing, then creating a container with the same release and architecture would require only this command (note you might need to prepend `sudo` to this command, depending on your specific configuration):

    lxc-create -t ubuntu -n <container name>

Once the base container is created, you can then edit the container configuration (which is found at `/var/lib/lxc/<container name>/config` by default) to add the second interface. You'd add this text to the configuration:

    lxc.network.type = veth
    lxc.network.flags = up
    lxc.network.veth.pair = c1eth1
    lxc.network.ipv4 = 10.10.10.10/24

In my specific testing, I left the container's first network interface attached to the default `lxcbr0` Linux bridge, which provides connectivity to external networks via NAT. If you wanted bridged connectivity, you'd need to set that up separately. Also, feel free to substitute a different name for `c1eth1` above; this just provides a user-recognizable name for the host-facing side of the veth pair that provides connectivity for the container.

After you've created and configured the container(s) appropriately, start the container with `lxc-start -n <container name>` to ensure that it boots successfully and without any unexpected errors. Then shut it down and start it again with the console detached, like so:

    lxc-start -d -n <container name>

Now that the container is up and running, you're ready to configure OVS.

## Create and Configure the OVS Bridge

If you're not familiar with connecting things with GRE tunnels using OVS, I suggest you take a look [here][1] and [here][2]. You will probably also find this post on [examining OVS traffic patterns][4] to be helpful in understanding _why_ you will configure OVS in this particular way.

Create the OVS bridge you'll use for inter-container connectivity using `ovs-vsctl`, like this (feel free to substitute whatever name you want for "br-int" in the command below):

    ovs-vsctl add-br br-int

Add a GRE interface to this bridge:

    ovs-vsctl add-port br-int gre0

Then configure the GRE interface appopriately:

    set interface gre0 type=gre options:remote_ip=192.168.1.100

The IP address specified above should point to a reachable interface on the remote host where the other container(s) is/are running. Also, you are welcome to use a different name for the port and interface than what I used (I used `gre0`), but be sure the port and interface match (as far as I know you can't use one name for the port and a different name for the interface).

If you are connecting only two hosts with containers, then you'll repeat this configuration on the second host, except that its GRE tunnel will point back to the first host (the first host specifies the IP address of the second host, the second host specifies the IP address of the first host).

If you are connecting more than two hosts, you'll either want to a) manually build a non-looping topology of GRE tunnels, or b) build a full mesh of GRE tunnels and enable STP, as outlined [here][5].

Once you've created the OVS bridge and the GRE interface, you're ready for the final step: connecting the containers to OVS.

## Attach the Containers to OVS

The final step is to connect the container's veth interface to OVS. If you're using an early enough version of OVS that still contains the Linux bridge compatibility code, then you can just specify the name of the OVS bridge with the `lxc.network.link` directive in the container's configuration file. Newer versions of OVS, however, lack the Linux bridge compatibility layer, so this doesn't work. In this case, you'll need to manually attach the veth interfaces to OVS.

To manually attach the container's veth interface to OVS, you must first identify the veth interface. This is why I used the `lxc.network.veth.pair` configuration directive in the container to assign a user-recognizable name. If you didn't use that directive, you can use the `ip link list` command to list the interfaces.

Once you've identified the correct veth interface, simply add it to OVS with this command:

    ovs-vsctl add-port br-int c1eth1

I used the `c1eth1` name in the command above; you'd substitute the correct name for the appropriate veth interface for your container(s).

Once that is done, you should be able to ping over the GRE tunnel using the IP addresses assigned to the inter-container communication interfaces (which should be recognized as eth1 in the containers). Congratulations---you've just connected containers on two different hosts together over a GRE tunnel!

There are a couple of things to note about this configuration:

* Note that we only used a single interface on the host. Obviously, we could have used more (for example, we could have use eth0 for host management and eth1 for host-to-host GRE tunnel connections), but using only a single host interface allows us to use this method in public cloud environments that only provide a single interface.

* While the GRE tunnel provides encapsulation, it does not provide encryption.

* The process of attaching the containers' veth interface to OVS is manual right now, so that won't scale in an environment of any size. I'm exploring ways to help automate that process. One possible avenue is the use of [libvirt](http://libvirt.org/), so stay tuned for posts describing any progress I make in that area.

If you have any questions, corrections, suggestions, or clarifications, please feel free to post them in the comments below. All feedback is welcome and appreciated.

[1]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[2]: {{< relref "2013-09-09-namespaces-vlans-open-vswitch-and-gre-tunnels.md" >}}
[3]: {{< relref "2013-11-25-a-brief-introduction-to-linux-containers-with-lxc.md" >}}
[4]: {{< relref "2013-05-15-examining-open-vswitch-traffic-patterns.md" >}}
[5]: {{< relref "2013-11-22-an-update-on-using-gre-tunnels-with-open-vswitch.md" >}}
