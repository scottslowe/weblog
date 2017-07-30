---
author: slowe
categories: Tutorial
comments: true
date: 2012-08-17T12:03:26Z
slug: installing-kvm-and-open-vswitch-on-ubuntu
tags:
- KVM
- Linux
- Networking
- OVS
- Ubuntu
- Virtualization
title: Installing KVM and Open vSwitch on Ubuntu
url: /2012/08/17/installing-kvm-and-open-vswitch-on-ubuntu/
wordpress_id: 2722
---

This is the first of a number of posts in which I'll be discussing Ubuntu Linux, KVM, and the Open vSwitch (OVS). I hope that you find the posts helpful.

Let me start this post by providing this disclaimer: I'm still very early in the learning curve with KVM and OVS, so I can't promise you that this post will be absolutely perfect. It is, however, a pretty decent starting point. I've gone through this process several times, and there is only one sticking point that I haven't yet resolved (which I'll describe later on).

I'm going to assume that you are reasonably familiar with Ubuntu, as that's the platform I'm using for all my testing (specifically, Ubuntu Server 12.04 LTS with a 64-bit kernel). I'm also assuming that you are prefacing all these commands with `sudo` or that you've attained root privileges via the method of your choice.

We'll start this process by ensuring that our Ubuntu installation is up-to-date:

    apt-get update && apt-get dist-upgrade

In my case, this brought me up to Ubuntu 12.04.1, running the 3.2.0-29 kernel.

Next, we'll install KVM and a couple associated packages:

    apt-get install kvm libvirt-bin virtinst

This command will install KVM, libvirt, a couple of command-line utilities, and a whole bunch of other required packages.

Next, to prepare the system for the installation of OVS, we'll remove the default libvirt bridge (named `virbr0`). This ensures that the bridge compatibility portion of OVS can load without any potential conflicts:

    virsh net-destroy default
    virsh net-autostart --disable default

At this point, because we aren't using the default Linux bridge, we can remove the ebtables package using the following command. _(Note: I am not 100% sure that this step is necessary, but almost all of the guides I followed included this step. If anyone has more information, please let me know in the comments.)_

    aptitude purge ebtables

You're now ready to install OVS (this should be all on one line; I've added a line breaks---noted by a backslash---for readability):

    apt-get install openvswitch-controller openvswitch-brcompat \
    openvswitch-switch openvswitch-datapath-source

This command installs the various OVS components and---as with KVM---a large number of required packages. Depending on the speed of your Internet connection, you might want to go get a cup of coffee.

Once the OVS packages are installed, we'll need to enable bridge compatibility mode for OVS. To enable bridge compatibility mode, edit `/etc/default/openvswitch-switch` and change this line:

    #BRCOMPAT=no

Change it by removing the hash mark (uncommenting it) and specifying yes, like this:

    BRCOMPAT=yes

At this point, you can try starting OVS with `service openvswitch-switch start`, but you might get an error about a module being in the wrong format or not built for your kernel. If you removed the default libvirt bridge (`virbr0`) using the `virsh` commands earlier, then you _should_ be OK, but if you get an error about the module being in the wrong format go ahead and run this command to build and install the necessary module:

    module-assistant auto-install openvswitch-datapath

Once this process finishes, let's go ahead and reboot. After the system reboots and comes back up, check the status of KVM and OVS with the following commands.

To check KVM, use this command:

    virsh -c qemu:///system list

This will return a list of running VMs (which is probably empty on your system).

To check OVS, first use this command to see if the kernel modules loaded properly:

    lsmod | grep brcom

This should return an entry containing `brcompat_mod` and `openvswitch_mod`. Follow that with this command:

    service openvswitch-switch status

The output should show several OVS processes in the running state.

Assuming that everything is working properly, you should now be able to run `ovs-vsctl show` and get a very simple response that basically indicates that OVS has no configuration.

We are now nearing the end; only a couple of steps remain. The last thing we need to do is create an OVS bridge that will allow KVM guests to communicate with the outside world. This is a two-step process. The first step is to use `ovs-vsctl` to create a bridge and add at least one physical interface:

    ovs-vsctl add-br br0
    ovs-vsctl add-port br0 eth0

Obviously, you'll want to substitute the correct physical interface for `eth0` in the commands above. After running these commands, then running `ovs-vsctl show` will return something like this:

    bc12c8d2-6900-42dd-9c1c-30e8ecb99a1b
        Bridge "br0"
            Port "eth0"
                Interface "eth0"
            Port "br0"
                Interface "br0"
                    type: internal
        ovs_version: "1.4.0+build0"

The final step is to edit `/etc/network/interfaces` so that the bridge comes up automatically. This is where I ran into problems. No matter what I tried, I could get the bridge to come up, but the physical interface attached to the bridge would not come up automatically. I could easily manually bring it up (using `ifconfig eth0 up`), but it wouldn't come up automatically on boot. If anyone has any ideas, I'm open to them.

In any case, here's the `/etc/network/interfaces` that I used (IP addresses and domain names have been changed to protect the innocent):

    # This file describes the network interfaces available on your system
    # and how to activate them. For more information, see interfaces(5).
    
    # The loopback network interface
    auto lo
    iface lo inet loopback
    
    # The primary network interface
    auto eth0
    iface eth0 inet manual
    
    # The OVS bridge interface
    auto br0
    iface br0 inet static
    address 192.168.1.200
    network 192.168.1.0
    netmask 255.255.255.0
    broadcast 192.168.1.255
    gateway 192.168.1.1
    bridge_ports eth0
    bridge_fd 9
    bridge_hello 2
    bridge_maxage 12
    bridge_stp off
    dns-search mydomain.local
    dns-nameservers 192.168.1.1 192.168.1.2

I tried several different variations for `eth0`, but none of them would bring the interface up automatically when I boot the system. Everything else seems to work just fine, so I've settled with manually bringing the interface up after a reboot until I figure out how to fix it. Suggestions are welcome.

In a future post (very soon), I'll be talking more about OVS and what it looks like when you boot up KVM guests on OVS, so stay tuned for that.

As always, thoughts, corrections, questions, and clarifications are welcome---just speak up in the comments below. Thanks!
