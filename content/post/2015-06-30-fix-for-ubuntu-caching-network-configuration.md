---
author: slowe
categories: Information
comments: true
date: 2015-06-30T11:00:00Z
tags:
- Ubuntu
- Linux
- CLI
- Networking
title: A Fix for Ubuntu Apparently Caching Network Configuration
url: /2015/06/30/fix-for-ubuntu-caching-network-configuration/
---

I've been wrestling with an [Ubuntu][link-1] network configuration issue over the last couple of weeks (off and on between working on other projects), and today I finally found a fix for the problem. The issue was that Ubuntu wouldn't pick up changes to network interfaces. The fix is so simple I'm almost embarrassed to talk about it (it seems like something that I should have known), but I'm posting it here in case others run into the same issue.

Here's a bit more context: I was switching some of the network interfaces in my Ubuntu 14.04.2 servers from a "standard" network configuration to using VLAN interfaces (after all, it seemed like such a shame to not more fully utilize the 10GbE and 40GbE interfaces in these servers). Before the reconfiguration, the servers had a network interface configuration file (located in `/etc/network/interfaces.d` and sourced in `/etc/network/interfaces`) that looked something like this:

```
auto p55p1
iface p55p1 inet static
address 172.16.3.201
netmask 255.255.255.0
```

This interface was connected to a port on a [Cumulus Linux][link-2]-powered [Dell][link-3] S6000-ON that was configured as an access port on a particular VLAN. Everything seemed to work just fine.

After reconfiguration (done using [Ansible][link-4], more on that in a future post), I had _two_ interface configuration files---one for the physical interface, and one for the VLAN interface. The physical interface file looked like this:

```
auto p55p1
iface p55p1 inet manual
up ip link set p55p1 up
down ip link set p55p1 down
```

And the VLAN interface file looked like this:

```
auto p55p1.3
iface p55p1.3 inet static
address 172.16.3.201
netmask 255.255.255.0
```

At the same time, the switch port on the Dell S6000-ON was being taken from an access port to a VLAN trunk port (using the configuration found in [this post][xref-1]). It was a pretty simple reconfiguration---just move the IP address from the physical interface to the VLAN interface. Piece of cake, right?

Except that I saw all sorts of really weird results---HostA could ping HostB, but HostB couldn't ping HostA. Restarting the network interfaces using a variety of methods (`initctl`, `service networking restart`, `ip link set`, and `ifup`/`ifdown`) all resulted in what appeared to be _reverting_ the changes. I'd see the IP address pop back onto the physical interface, which resulted in the same IP address on two different interfaces. This caused multiple routes in the routing table, etc. Very odd.

Rebooting would fix it, but come on---rebooting a server to make network changes? What is this, Windows in the 1990s? (Sorry, couldn't resist!) I tried the #ubuntu-server IRC channel on Freenode; although the folks there were helpful, no solution presented itself.

Finally, today, I found the solution. As described [here][link-5], Ubuntu will apparently totally ignore changes in the network configuration files for devices that have already been defined and then changed (which was the case here---the physical interface was defined, then changed). The fix is to run the command `ip addr flush dev <devicename>` after taking the interface down. Simple! I incorporated this command into the Ansible tasks in my playbook, re-ran the playbook to institute some changes (with a `--limit` switch to restrict operation to a particular host), and it worked perfectly.

The takeaway, therefore, is to be sure to flush the device configuration (using `ip addr flush dev <devicename>`) when you need to move the IP address from one interface to another so as to avoid the same issues that I was seeing.

Here's hoping this helps someone else!

[link-1]: http://www.ubuntu.com
[link-2]: http://cumulusnetworks.com
[link-3]: http://www.dell.com
[link-4]: http://www.ansible.com/home
[link-5]: http://askubuntu.com/questions/509975/how-can-i-change-the-network-configuration-on-ubuntu-14-04-server
[xref-1]: {{< relref "2015-06-25-vlan-trunking-cumulus-linux.md" >}}
