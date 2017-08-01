---
author: slowe
categories: Tutorial
comments: true
date: 2012-09-06T17:15:26Z
slug: installing-kvm-on-centos-6-3
tags:
- CentOS
- KVM
- Linux
- OSS
- Virtualization
title: Installing KVM on CentOS 6.3
url: /2012/09/06/installing-kvm-on-centos-6-3/
wordpress_id: 2807
---

After spending some time working with KVM on Ubuntu (see [this post][1]), I thought it might be worthwhile to try the same thing on a different Linux distribution. I like Ubuntu (generally), but wanted to try it on a Red Hat/CentOS system. So, here's a look at installing KVM on CentOS 6.3. To be honest, it's actually pretty simple. For another look at the process, see [this HowtoForge post](http://www.howtoforge.com/virtualization-with-kvm-on-a-centos-6.3-server).

For reasons that will become clear in a future post, I did _not_ install Open vSwitch during this process. (Short story: There's a known bug in Open vSwitch caused by a backport of a kernel fix to the kernel version used by RedHat/CentOS 6.3, and I haven't been able to find a fix yet.)

I started this process with a minimal install (the default option) of 64-bit CentOS 6.3.

First, use `yum groupinstall` (handy feature, by the way) to install the virtualization-related packages (line-wrapped here for readability):

    yum groupinstall Virtualization "Virtualization Client" \
    "Virtualization Platform" "Virtualization Tools"

[This page](http://www.web-manual.net/linux-3/how-to-install-kvm-virtualization-on-rhel-6centos-6/) breaks down these four groups and lists the individual packages contained in each.

However, the `libvirtd` daemon wouldn't start after this process. In reviewing the log files (found, by default, at `/var/log/libvirt`), I found that it was failing due to a problem with multicast DNS (mDNS). That was fixed with:

    yum install avahi
    service start avahi-daemon

[This site](http://blog.mattbrock.co.uk/2012/02/12/virtualisation-with-kvm-and-lvm-on-centos-6-via-the-command-line/) alluded to the need for `avahi` to be installed, but I was a bit surprised that it didn't get installed automatically during the `yum groupinstall` process. Once `avahi` was installed, `libvirtd` started cleanly. I was then able to run `virsh` without any issues or errors.

Normally, from here you'd continue with setting up a Linux bridge, etc. I stopped here with the intention of first [upgrading libvirt to the latest build][2] and then installing Open vSwitch, but there are plenty of other "how to's" that outline any additional follow-up steps.

I hope this helps someone!

[1]: {{< relref "2012-08-17-installing-kvm-and-open-vswitch-on-ubuntu.md" >}}
[2]: {{< relref "2012-09-06-compiling-libvirt-0-10-1-on-centos-6-3.md" >}}
