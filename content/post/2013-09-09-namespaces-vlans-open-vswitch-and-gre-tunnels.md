---
author: slowe
categories: Tutorial
comments: true
date: 2013-09-09T09:00:00Z
slug: namespaces-vlans-open-vswitch-and-gre-tunnels
tags:
- CLI
- GRE
- Interoperability
- KVM
- Linux
- Networking
- OVS
- Ubuntu
title: Namespaces, VLANs, Open vSwitch, and GRE Tunnels
url: /2013/09/09/namespaces-vlans-open-vswitch-and-gre-tunnels/
wordpress_id: 3274
---

In this post, I'm going to show you how I combined Linux network namespaces, VLANs, [Open vSwitch (OVS)](http://openvswitch.org/), and GRE tunnels to do something interesting. Well, I found it interesting, even if no one else does. However, I will provide this disclaimer up front: while I think this is technically interesting, I don't think it has any real, practical value in a production environment. (I'm happy to be proven wrong, BTW.)

This post builds on information I've provided in previous posts:

* [Using VLANs with OVS and libvirt][1] (plus the [update here][2])

* [Using GRE Tunnels with Open vSwitch][3]

* [Introducing Linux Network Namespaces][4]

It may pull pieces from a few other posts, but the bulk of the info is found in these. If you haven't already read these, you might want to take a few minutes and go do that---it will probably help make this post a bit more digestible.

After working a bit with network namespaces---and knowing that OpenStack Neutron uses network namespaces in certain configurations, especially to support overlapping IP address spaces---I wondered how one might go about integrating multiple network namespaces into a broader configuration using OVS and GRE tunnels. Could I use VLANs to multiplex traffic from multiple namespaces across a single GRE tunnel?

To test my ideas, I came up with the following design:

![Testing topology](/public/img/ns-vlan-ovs-gre.png)

As you can see in the diagram, my test environment has two KVM hosts. Each KVM host has a network namespace and a running guest domain. Both the network namespace and the guest domain are connected to an OVS bridge; the network namespace via a veth pair and the guest domain via a vnet port. A GRE tunnel between the OVS bridges connects the two hosts.

The idea behind the test environment was that the VM on one host would communicate with the veth interface in the network namespace on the other host, using VLAN-tagged traffic over a GRE tunnel between them.

Let's walk through how I built this environment to do the testing.

I built KVM Host 1 using Ubuntu 12.04.2, and installed KVM, libvirt, and OVS. On KVM Host 1, I built a guest domain, attached it to OVS via a libvirt network, and configured the VLAN tag for its OVS port with this command:

    ovs-vsctl set port vnet0 tag=10

In the guest domain, I configured the OS (also Ubuntu 12.04.2) to use the IP address 10.1.1.2/24.

Also on KVM Host 1, I created the network namespace, created the veth pair, moved one of the veth interfaces, and attached the other to the OVS bridge. This set of commands is what I used:

    ip netns add red
    ip link add veth0 type veth peer name veth1
    ip link set veth1 netns red
    ip netns exec red ip addr add 10.1.2.1/24 dev veth1
    ip netns exec red ip link set veth1 up
    ovs-vsctl add-port br-int veth0
    ovs-vsctl set port veth0 tag=20

Most of the commands listed above are taken straight from the [network namespaces article][4] I wrote, but let's break it down anyway just for the sake of full understanding:

* The first command adds the "red" namespace.

* The second command creates the veth pair, creatively named veth0 and veth1.

* The third command moves veth1 into the red namespace.

* The next two commands add an IP address to veth1 and set the interface to up.

* The last two commands add the veth0 interface to an OVS bridge named br-int, and then set the VLAN tag for that port to 20.

When I'm done, I'm left with KVM Host 1 running a guest domain on VLAN 10 and a network namespace on VLAN 20. (Do you see how I got there?)

I repeated the process on KVM Host 2, installing Ubuntu 12.04.2 with KVM, libvirt, and OVS. Again, I built a guest domain (also running Ubuntu 12.04.2), configured the operating system to use the IP address 10.1.2.2/24, attached it to OVS via a libvirt network, and configured its OVS port:

    ovs-vsctl set port vnet0 tag=20

Similarly, I also created a new network namespace and pair of veth interfaces, but I configured them as a "mirror image" of KVM Host 1, reversing the VLAN assignments for the guest domain (as shown above) and the network namespace:

    ip netns add blue
    ip link add veth0 type veth peer name veth1
    ip link set veth1 netns blue
    ip netns exec blue ip addr add 10.1.1.1/24 dev veth1
    ip netns exec blue ip link set veth1 up
    ovs-vsctl add-port br-int veth0
    ovs-vsctl set port veth0 tag=10

That leaves me with KVM Host 2 running a guest domain on VLAN 20 and a network namespace on VLAN 10.

The final step was to [create the GRE tunnel between the OVS bridges][3]. However, after I established the GRE tunnel, I configured the GRE port to be a VLAN trunk using this command (this command was necessary on both KVM hosts):

    ovs-vsctl set port gre0 trunks=10,20,30

So I now had the environment I'd envisioned for my testing. VLAN 10 had a guest domain on one host and a veth interface on the other; VLAN 20 had a veth interface on one host and a guest domain on the other. Between the two hosts was a GRE tunnel configured to act as a VLAN trunk.

Now came the critical test---would the guest domain be able to ping the veth interface? This screen shot shows the results of my testing; this is the guest domain on KVM Host 1 communicating with the veth1 interface in the separate network namespace on KVM Host 2:

[![Ping results](/public/img/dom2ns-ping-test-small.png)](/public/img/dom2ns-ping-test-fullsize.png)

Success! Although not shown here, I also tested all other combinations as well, and they worked. (Note you'd have to use `ip netns exec ping ` to ping from the veth1 interface in the network namespace.) I now had a configuration where I could integrate multiple network namespaces with GRE tunnels and OVS. Unfortunately---and this is where the whole "technically interesting but practically useless" statement comes from---this isn't really a usable configuration:

* The VLAN configurations were manually applied to the OVS ports; this means they disappeared if the guest domains were power-cycled. (This could be fixed using libvirt portgroups, but I hadn't bothered with building them in this environment.)

* The GRE tunnel had to be manually established and configured.

* Because this solution uses VLAN tags inside the GRE tunnel, you're still limited to about 4,096 separate networks/network namespaces you could support.

* The entire process was manual. If I needed to add another VLAN, I'd have to manually create the network namespace and veth pair, manually move one of the veth interfaces into the namespace, manually add the other veth interface to the OVS bridge, and manually update the GRE tunnel to trunk that VLAN. Not very scalable, IMHO.

However, the experiment was not a total loss. In figuring out how to tie together network namespaces and tunnels, I've gotten a better understanding of how all the pieces work. In addition, I have a lead on an even better way of accomplishing the same task: using OpenFlow rules and tunnel keys. This is the next area of exploration, and I'll be sure to post something when I have more information to share.

In the meantime, feel free to share your thoughts and feedback on this post. What do you think---technically interesting or not? Useful in a real-world scenario or not? All courteous comments (with vendor disclosure, where applicable) are welcome.

[1]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
[2]: {{< relref "2012-11-12-libvirt-ovs-integration-revisited.md" >}}
[3]: {{< relref "2013-05-07-using-gre-tunnels-with-open-vswitch.md" >}}
[4]: {{< relref "2013-09-04-introducing-linux-network-namespaces.md" >}}
