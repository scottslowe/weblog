---
author: slowe
categories: Tutorial
comments: true
date: 2012-09-06T17:22:30Z
slug: compiling-libvirt-0-10-1-on-centos-6-3
tags:
- CLI
- CentOS
- KVM
- Libvirt
- Linux
- OSS
- Virtualization
title: Compiling libvirt 0.10.1 on CentOS 6.3
url: /2012/09/06/compiling-libvirt-0-10-1-on-centos-6-3/
wordpress_id: 2810
---

In my post on installing [KVM and Open vSwitch on Ubuntu][1], I mentioned that I was using [libvirt](http://libvirt.org/index.html) as the virtualization API/toolkit by which to manage KVM. Unfortunately, that particular version of libvirt didn't have built-in support for [Open vSwitch](http://openvswitch.org/); that was added in libvirt 0.9.11 (I _think_ that I was using 0.9.8 in my testing). In any case, I noticed that libvirt had released 0.10.1 on August 31, and it included a couple of OVS fixes regarding VLANs and VLAN support (among a host of other fixes; see [here](http://libvirt.org/news.html) for more details). Considering that OVS is a key focus area for me (it's foundational to OpenStack Quantum), I thought I'd upgrade my test system to the latest version of libvirt. Because the release was so new, there weren't yet any precompiled binary packages of the latest version, which meant I had to compile and install it on my own.

My first attempt to do so was using [Ubuntu](http://www.ubuntu.com/) 12.04.1 LTS, and while it _appeared_ successful at first, I soon found that it was horribly broken. While I do really like Ubuntu as a desktop Linux distribution, I'm not entirely convinced (and even less so now) about it's place as a server distribution. With that in mind, and considering the enormous popularity of [Red Hat](http://www.redhat.com/), I decided to rebuilt my test system with [CentOS](http://www.centos.org/) 6.3 (the "community version" of Red Hat Enterprise Linux 6.3) and try it again there. In order to use libvirt, I naturally had to get KVM installed on CentOS 6.3 first; [here][2] is the process I used.

Once KVM was installed, this attempt at compiling the latest version of libvirt was much more successful, so I wanted to document the process for others.

I started out with a CentOS 6.3 x86_64 system that already had KVM and libvirt installed from packages (see [this article][2] for full details), and from there I followed these steps:

1. First I downloaded the tarball for libvirt 0.10.1 from the [libvirt.org HTTP server](http://libvirt.org/sources/). You could probably use `curl` or `wget` to do this directly from the CentOS server; in my case, I downloaded it on my Mac and then used SFTP to push it to the CentOS server.

2. Next, I extracted the files using `gunzip -c libvirt-0.10.1.tar.gz | tar xvf -`.

3. I used `cd` to change into the newly-created libvirt-0.10.1 directory that resulted from the commands in step 2.

4. Through process of elimination (in other words, running `configure` again and again) I determined that the following packages need to be installed before you'll be able to successfully compile libvirt 0.10.1: gcc, make, libxml2-devel, gnutls-devel, device-mapper-devel, python-devel, and libnl-devel. I used `yum install` to install each of these packages.

5. With all the dependencies now satisfied, I ran `configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc` from the extracted libvirt-0.10.1 directory. (I selected the specified directories because that matched where the existing binaries were on the system, although there's probably an easier/better way.

6. The above command results in a libvirt that supports KVM, OpenVZ, LXC, etc., but doesn't support Xen or ESX. You'd have to add the `--with-xen` or `--with-esx` parameters to the `configure` command to include support for those. Following completion of the previous command, it was very straightforward to build libvirt. Just run `make`, followed by `make install`, and finally `ldconfig`.

7. Finally, I restarted libvirtd with `service libvirtd restart`. The daemon restarted on my system cleanly and without any errors.

Failing to run `ldconfig` at the end of the make/make install series, by the way, resulted in numerous errors when trying to run `virsh`. 

From there, I was able to run `libvirtd --version` or `virsh --version` to verify that the system was, in fact, running libvirt 0.10.1. All in all, it was pretty straightforward, and I was pleasantly surprised that it wasn't more complicated or troublesome.

Now, if only the story with Open vSwitch and CentOS 6.3 was as simplebut that is a post for another day. Until then, feel free to speak up in the comments below.

[1]: {{< relref "2012-08-17-installing-kvm-and-open-vswitch-on-ubuntu.md" >}}
[2]: {{< relref "2012-09-06-installing-kvm-on-centos-6-3.md" >}}
