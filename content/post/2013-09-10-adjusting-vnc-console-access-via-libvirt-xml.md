---
author: slowe
categories: Explanation
comments: true
date: 2013-09-10T09:00:00Z
slug: adjusting-vnc-console-access-via-libvirt-xml
tags:
- CLI
- KVM
- Libvirt
- Linux
- Virtualization
title: Adjusting VNC Console Access via Libvirt XML
url: /2013/09/10/adjusting-vnc-console-access-via-libvirt-xml/
wordpress_id: 3276
---

It's been a little while since I talked about [libvirt](http://libvirt.org/) directly, but in this post I'd like to go back to libvirt to talk about how you can edit a guest domain's libvirt XML definition to adjust access to the VNC console for that guest domain.

This is something of a follow-up to my post on [using SSH to access the VNC consoles of KVM guests][1], in which I showed how to use SSH tunneling (including multi-hop tunnels) to access the VNC console of a KVM guest domain. That post was necessary because, by default, libvirt binds the VNC server for a guest domain to the KVM host's loopback address. Thus, the _only_ way to access the VNC console of a KVM guest domain is via SSH, and then tunneling VNC traffic to the host's loopback address.

However, it's possible to change this behavior. Consider the following snippet of libvirt XML code; this is responsible for setting up the VNC access to the guest domain's console:

    <graphics type='vnc' port='-1' autoport='yes'/>

This is the default configuration; it binds a VNC server for this guest domain to the host's loopback address and auto-allocates a port for VNC (the first guest gets screen 0/port 5900, the second guest gets screen 1/port 5901, etc.). With this configuration, you must use SSH and tunneling in order to gain access to the guest domain's VNC console.

Now consider this libvirt XML configuration:

    <graphics type='vnc' port='-1' autoport='yes' listen='0.0.0.0'/>

With this configuration in place, the VNC console for that guest domain will be available on _any_ interface on the host, but the port will still be auto-allocated (first guest domain powered on gets port 5900, second guest domain powered on gets port 5901, etc.). With this configuration, anyone with a VNC viewer on your local network can gain access to the console of the guest domain. As you might expect, you could change the `'0.0.0.0'` to a specific IP address assigned to the KVM host, and you could limit access to the VNC port via IPTables (if you so desired).

If you so desired, you can password-protect the VNC console for the guest domain using this snippet of libvirt XML configuration:

    <graphics type='vnc' port='-1' autoport='yes' listen='0.0.0.0' passwd='protectme'/>

Now, the user attempting to access the guest domain's VNC console must know the password specified by the `passwd` parameter in the configuration. Otherwise, this configuration is the same as the previous configuration, and can be limited/protected in a variety of ways (limited to specific interfaces and/or controlled via IPTables).

For more details, you can refer to the full reference on the [libvirt guest domain XML configuration](http://libvirt.org/formatdomain.html).

[1]: {{< relref "2013-08-21-accessing-vnc-consoles-of-kvm-guests-via-ssh.md" >}}
