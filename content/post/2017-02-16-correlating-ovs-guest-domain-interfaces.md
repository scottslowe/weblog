---
author: slowe
categories: Education
comments: true
date: 2017-02-16T00:00:00Z
tags:
- OVS
- OSS
- CLI
- Linux
- Networking
title: Correlating OVS and Guest Domain Interfaces
url: /2017/02/16/correlating-ovs-guest-domain-interfaces/
---

I've written a fair amount about [Open vSwitch (OVS)][link-1], including some articles on using it with KVM and [Libvirt][link-2]. One thing I haven't discussed in such environments, though, is the potential challenge of mapping network interfaces in a guest domain to the corresponding OVS interface (for the purposes of troubleshooting, for example). There is no single command that will provide a guest-to-OVS interface map (as far as I know), but this information is easily gathered using a couple commands.

## Gathering Information About the Guest Interface

First, we'll need to gather some information about the interface from the guest domain's perspective. There are two ways we can do this: from within the guest OS itself, or by interrogating Libvirt.

### Working from Within the Guest OS

Inside the guest domain (I'm assuming you're using a relatively recent Linux distribution), you only need to use standard commands like `ip link list` or `ip addr list`. The goal is to obtain the MAC address assigned to the particular guest interface. So, for example, if you wanted to get the MAC address for the guest "eth0" interface, you'd run:

        ip link list eth0

To isolate only the MAC address from the output of that command, just add some standard UNIX/Linux utilities to the mix:

        ip link list eth0 | grep 'link/ether' | cut -f6 -d' '

For a Windows guest, there's a couple commands you can use. Recent versions of Windows support a command called `getmac`, which will list the MAC addresses of the interfaces in the system. Unfortunately, it doesn't really provide any helpful way of knowing which interface is which. Your better option is `ipconfig /all`, which will list all the interfaces in the system, along with their IP address and MAC address. Since Windows doesn't come with tools like `grep` or `awk` to help isolate portions of the output, you'll need to manually parse the output to find and record the MAC address of the interface.

### Interrogating Libvirt

When you're using Libvirt, you can also interrogate Libvirt about the guest domain to gather the necessary information. (If you're unfamiliar with using `virsh` to work with KVM guests, see [this post][xref-1].)

Assuming you know the same of the guest domain in question (which hopefully you do), you can run `virsh edit <domain>` to show the Libvirt XML of the domain in question. You can also run `virsh dumpxml <domain>` to dump the XML to standard output. Fortunately, since you're doing this on a Linux box, you can take advantage of some extra tools. For example, if `xmllint` is installed, you can do this:

    virsh dumpxml domain-name | xmllint --xpath //mac -

This takes the XML output from `virsh` and passes it through `xmllint`, which uses an XPath query to find the MAC address. I'll leave it as an exercise for the reader to further refine that output using `cut`.

## Finding the Matching OVS Interface

Once you have the MAC address of the interface in the guest domain, you can use `ovs-vsctl` to determine which OVS port is connected to that interface. Specifically, you can use this command:

    ovs-vsctl list interface vnet0

In the output of the command, look for the "external_ids" line; it will contain an entry called "attached-mac", and that represents the MAC address of the interface in the guest domain OS attached to this particular OVS port.

However, we can refine the output of information from OVS even more, and this time we don't even have to revert to piping the output through multiple Linux tools (although you _could_ do that if desired). Just use this command instead:

    ovs-vsctl get interface vnet0 external_ids

This will return only the "external_ids" value for the "vnet0" interface, making it super-easy to find the "attached-mac" value and compare it with the MAC address obtained earlier.

What if you have lots and lots of ports on your OVS bridge? No problem, just run this set of commands:

    for i in $(ovs-vsctl list-ports br0); do echo $i && ovs-vsctl get interface $i external_ids; done

And you'll get some output that lists each interface name followed by the value of the "external_ids" field. From there it should be a piece of cake to match it against the MAC address from the guest, thereby giving you the specific OVS port the guest domain is using.

Enjoy!



[link-1]: https://openvswitch.org/
[link-2]: https://libvirt.org/
[xref-1]: {{< relref "2012-08-21-working-with-kvm-guests.md" >}}
