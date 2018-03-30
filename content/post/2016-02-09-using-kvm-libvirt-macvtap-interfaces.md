---
author: slowe
categories: Tutorial
comments: true
date: 2016-02-09T00:00:00Z
tags:
- Linux
- CLI
- KVM
- Libvirt
- Networking
- Virtualization
title: Using KVM with Libvirt and macvtap Interfaces
url: /2016/02/09/using-kvm-libvirt-macvtap-interfaces/
---

In this post, I'm going to show you how to use [KVM][link-1] with [Libvirt][link-2] and macvtap interfaces for networking (as opposed to a Linux bridge or [Open vSwitch][link-3]). Along the way, I'll provide some context around why this sort of approach might be interesting/useful to you.

In this post, I'll assume you are already somewhat familiar with KVM and Libvirt; if you need an introduction to either of those, you might find [this post][xref-1], [this post][xref-2], and [this post][xref-3] (all from my site) helpful.

Macvtap interfaces are related to macvlan interfaces (they share the same kernel driver), so you'll want to first be sure your Linux kernel supports the macvlan driver. These commands will help you know if your kernel is good to go:

    modprobe macvlan
    lsmod | grep macvlan

If you get an error or the `lsmod` command returns no results, then you may have a problem using the macvlan driver. I tested this on [Debian][link-8] 8.1, [Ubuntu][link-9] 14.04, and [CentOS][link-10] 7.1, and all of them worked without any issues. If you have a relatively recent distribution of Linux, you should be fine.

Once you have KVM and Libvirt installed and working correctly, you'll start by defining a Libvirt network to which any VMs you create will be connected. This snippet of XML code defines a Libvirt network that uses macvtap interfaces associated with the `eth1` physical interface:

``` xml
<network>
  <name>macvtap-net</name>
  <forward mode="bridge">
    <interface dev="eth1"/>
  </forward>
</network>
```

You would use the `virsh net-define` command with this XML to define the actual Libvirt network. Assuming the XML code above was stored in a file named `macvtap-def.xml`, you'd run this command:

    virsh net-define macvtap-def.xml

Then you'd set the resulting Libvirt network to autostart and start it:

    virsh net-autostart macvtap-net
    virsh net-start macvtap-net

Then use `virt-install` (or the tool of your choice) to create a KVM guest domain attached to this new Libvirt network. CirrOS has a pre-built QCOW2 disk image (look [here][link-4]) that you can use with `virt-install` using a command like this (line-wrapped for readability):

    virt-install --name=cirros --ram=256 --vcpus=1 \
    --disk path=/home/user/cirros-0.3.2-x86_64-disk.img,format=qcow2 \
    --import --network network:macvtap-net,model=virtio --vnc

Obviously, you'd need to change `/home/user` in the above command to the actual path you're using in your environment (wherever you downloaded the CirrOS image).

Once this command completes, you'll have a KVM guest domain running and attached to a macvtap interface. You can see the new macvtap interface by running `ip link list`; you'll see an entry similar to the one shown below:

```
5: macvtap0@eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UNKNOWN mode DEFAULT group default qlen 500
    link/ether 52:54:00:5c:15:d6 brd ff:ff:ff:ff:ff:ff
```

If you get into the KVM guest domain (you can do this by running `virsh vncdisplay cirros` and noting the VNC display where the guest's console is accessible, then using SSH port forwarding to make that port accessible to your system), you'll see that the MAC address of the macvtap interface on the host is identical to the MAC address of the virtual interface in the guest. This is a by-product of how macvtap/macvlan interfaces work; since there is no "intermediate" device or TAP interface, the MAC addresses on the host and in the guest will match (it is essentially the _same_ interface in both places).

So why would one want to use macvtap interfaces, instead of a Linux bridge or Open vSwitch? One argument might be simplicity; aside from the macvlan kernel driver, there is nothing else to install, nothing else to configure, and no daemons to run. Of course, simplicity is a double-edged sword; there are also fewer features available in this sort of configuration. If you don't need the extra functionality that you could obtain using the Linux bridge or Open vSwitch, then macvtap interfaces might be the right approach.

## Other Resources

If you're interested in playing with this but don't want to spend a bunch of time building VMs and such, then go get [Vagrant][link-5] and the virtualization provider of your choice (I use [VMware Fusion][link-7]). After you've install those, visit [my GitHub "learning-tools" repository][link-6] and look in the `kvm-macvtap` directory. I've built a learning environment there that you can use to experiment with this functionality.



[link-1]: http://www.linux-kvm.org/page/Main_Page
[link-2]: http://libvirt.org/
[link-3]: http://openvswitch.org/
[link-4]: http://download.cirros-cloud.net/0.3.2/
[link-5]: http://www.vagrantup.com/
[link-6]: https://github.com/scottslowe/learning-tools
[link-7]: http://www.vmware.com/products/fusion/
[link-8]: https://www.debian.org/
[link-9]: http://www.ubuntu.com/
[link-10]: https://www.centos.org/
[xref-1]: {{< relref "2012-08-17-installing-kvm-and-open-vswitch-on-ubuntu.md" >}}
[xref-2]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
[xref-3]: {{< relref "2012-11-07-using-vlans-with-ovs-and-libvirt.md" >}}
