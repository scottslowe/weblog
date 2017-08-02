---
author: slowe
categories: Tutorial
comments: true
date: 2013-12-02T09:00:00Z
slug: connecting-bare-metal-workloads-using-gre-tunnels-and-ovs
tags:
- GRE
- KVM
- Linux
- LXC
- Networking
- OVS
- Virtualization
title: Connecting Bare Metal Workloads Using GRE Tunnels and OVS
url: /2013/12/02/connecting-bare-metal-workloads-using-gre-tunnels-and-ovs/
wordpress_id: 3340
---

In this post, I'll discuss how you could use [Open vSwitch (OVS)](http://openvswitch.org/) and GRE tunnels to connect bare metal workloads. While OVS is typically used in conjunction with a hypervisor such as KVM or Xen, you're certainly not restricted to _only_ using it on hypervisors. Similarly, while GRE tunnels are commonly used to connect VMs or containers, you're definitely not restricted from using them with bare metal workloads as well. In this post, I'll explore how you would go about connecting bare metal workloads over GRE tunnels managed by OVS.

This post, by the way, was sparked in part by a comment on my article on [using GRE tunnels with OVS][1], in which the reader asked: "Is there a way to configure bare Linux (Ubuntu)with OVS installed...to serve as a tunnel endpoint?" Hopefully this post helps answer that question. (By the way, the key to understanding how this works is in understanding OVS traffic patterns. If you haven't yet read my post on [examining OVS traffic patterns][2], I highly recommend you go have a look right now. Seriously.)

Once you have OVS installed (maybe [this is helpful][3]?), then you need to create the right OVS configuration. That configuration can be described, at a high level, like this:

* Assign an IP address to a physical interface. This interface will be considered the "tunnel endpoint," and therefore should have an IP address that is correct for use on the transport network.

* Create an OVS bridge that has no physical interfaces assigned.

* Create an OVS internal interface on this OVS bridge, and assign it an IP address for use inside the GRE tunnel(s). This interface will be considered the primary interface for the OS instance.

* Create the GRE tunnel for connecting to other tunnel endpoints.

Each of these areas is described in a bit more detail in the following sections.

## Setting Up the Transport Interface

When setting up the physical interface---which I'll refer to as the transport interface moving forward, since it is responsible for transporting the GRE tunnel across to the other endpoints---you'll just need to use an IP address and routing entries that enable it to communicate with other tunnel endpoints.

Let's assume that we are going to have tunnel endpoints on the 192.168.1.0/24 subnet. On the bare metal OS instance, you'd configure a physical interface (I'll assume `eth0`, but it could be any physical interface) to have an IP address on the 192.168.1.0/24 subnet. You could do this automatically via DHCP or manually; the choice is yours. Other than ensuring that the bare metal OS instance can communicate with other tunnel endpoints, _no additional configuration is required._ (I'm using "required" as in "necessary to make it work." You may want to increase the MTU on your physical interface and network equipment in order to accommodate the GRE headers in order to optimize performance, but that isn't required in order to make it work.)

Once you have the transport interface configured and operational, you can move on to configuring OVS.

## Configuring OVS

If you've been following along at home with all of my OVS-related posts (you can browse all posts using [the OVS tag](/tags/ovs/), you can probably guess what this will look like (hint: it will look a little bit like the configuration I described in my post on running [host management through OVS][4]). Nevertheless, I'll walk through the configuration for the benefit of those who are new to OVS.

First, you'll need to create an OVS bridge that has no physical interfaces---the so-called "isolated bridge" because it is isolated from the physical network. You can call this bridge whatever you want. I'll use the name `br-int` (the "integration bridge") because it's commonly used in other environments like OpenStack and NVP/NSX.

To create the isolated bridge, use `ovs-vsctl`:

    ovs-vsctl add-br br-int

Naturally, you would substitute whatever name you'd like to use in the above command. Once you've created the bridge, then add an OVS internal interface; this internal interface will become the bare metal workload's primary network interface:

    ovs-vsctl add-port br-int mgmt0 -- set interface mgmt0 type=internal

You can use a name other than `mgmt0` if you so desire. Next, configure this new OVS internal interface at the operating system level, assigning it an IP address. This IP address should be taken from a subnet "inside" the GRE tunnel, because it is only via the GRE tunnel that you'll want the workload to communicate.

The following commands will take care of this part for you:

    ip addr add 10.10.10.30/24 dev mgmt0
    ip link set mgmt0 up

The process of ensuring that the `mgmt0` interface comes up automatically when the system boots is left as an exercise for the reader (hint: use `/etc/network/interfaces`).

At this point, the bare metal OS instance will have **two** network interfaces:

* A physical interface (we're assuming `eth0`) that is configured for use on the transport network. In other words, it has an IP address and routes necessary for communication with other tunnel endpoints.

* An OVS internal interface (I'm using `mgmt0`) that is configured for use inside the GRE tunnel. In other words, it has an IP address and routes necessary to communicate with other workloads (bare metal, containers, VMs) via the OVS-hosted GRE tunnel(s).

Because the bare metal OS instance sees two interfaces (and therefore has visibility into the routes both "inside" and "outside" the tunnel), you may need to apply some policy routing configuration. See [my introductory post on Linux policy routing][5] if you need more information.

The final step is establishing the GRE tunnel.

## Establishing the GRE Tunnel

The commands for establishing the GRE tunnel have been described numerous times, but once again I'll walk through the process just for the sake of completeness. I'm assuming that you've already completed the steps in the previous section, and that you are using an OVS bridge named `br-int`.

First, add the GRE port to the bridge:

    ovs-vsctl add-port br-int gre0

Next, configure the GRE interface on that port:

    ovs-vsctl set interface gre0 type=gre options:remote_ip=<IP address of remote tunnel endpoint>

Let's say that you've assigned 192.168.1.10 to the transport interface on this system (the bare metal OS instance), and that the remote tunnel endpoint (which could be a host with multiple containers, or a hypervisor running VMs) has an IP address of 192.168.1.15. On the bare metal system, you'd configure the GRE interface like this:

    ovs-vsctl set interface gre0 type=gre options:remote_ip=192.168.1.15

On the remote tunnel endpoint, you'd configure the GRE interface like this:

    ovs-vsctl set interface gre0 type=gre options:remote_ip=192.168.1.10

In other words, each GRE interface points to the transport IP address on the _opposite_ end of the tunnel.

Once the configuration on both ends is done, then you should be able to go into the bare metal OS instance and ping an IP address inside the GRE tunnel. For example, I used this configuration to connect a bare metal Ubuntu 12.04 instance, a container running on an Ubuntu host, and a KVM VM running on an Ubuntu host (I had a full mesh topology with STP enabled, as described here). I was able to successfully ping between the bare metal OS instance, the container, and the VM, all inside the GRE tunnel.

## Summary, Caveats, and Other Thoughts

While this configuration is interesting as a "proof of concept" that OVS and GRE tunnels can be used to connect bare metal OS instances and workloads, there are a number of considerations and/or caveats that you'll want to think about before trying something like this in a production environment:

* The bare metal OS instance has visibility both "inside" and "outside" the tunnel, so there isn't an easy way to prevent the bare metal OS instance from communicating outside the tunnel to other entities. This might be OK---or it might not. It all depends on your requirements, and what you are trying to achieve. (In theory, you _might_ be able to provide some isolation using network namespaces, but I haven't tested this at all.)

* If you want to create a full mesh topology of GRE tunnels, you'll need to enable STP on OVS.

* There's nothing preventing you from attaching an OpenFlow controller to the OVS instances (including the OVS instance on the bare metal OS) and pushing flow rules down. This would eliminate the need for STP, since OVS won't be in MAC learning mode. This means you could easily incorporate bare metal OS instances into a network virtualization-type environment. However

* There's no easy way to provide a separation of OVS and the bare metal OS instance. This means that users who are legitimately allowed to make administrative changes to the bare metal OS instance could also make changes to OVS, which could easily "break" the configuration and cause problems. My personal view is that this is why you rarely see this sort of OVS configuration used in conjunction with bare metal workloads.

I still see value in explaining how this works because it provides yet another example of how to configure OVS and how to use OVS to help provide advanced networking capabilities in a variety of environments and situations.

If you have any questions, I encourage you to add them in the comments below. Likewise, if I have overlooked something, made any mistakes, or if I'm just plain wrong, please speak up below (courteously, of course!). I welcome all useful/pertinent feedback and interaction.

[1]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[2]: {{< relref "2013-05-15-examining-open-vswitch-traffic-patterns.md" >}}
[3]: {{< relref "2013-10-14-installing-open-vswitch-on-ubuntu-with-puppet.md" >}}
[4]: {{< relref "2012-10-30-running-host-management-on-open-vswitch.md" >}}
[5]: {{< relref "2013-05-29-a-quick-introduction-to-linux-policy-routing.md" >}}
