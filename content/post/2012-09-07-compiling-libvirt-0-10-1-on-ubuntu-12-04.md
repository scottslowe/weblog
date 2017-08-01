---
author: slowe
categories: Tutorial
comments: true
date: 2012-09-07T11:53:28Z
slug: compiling-libvirt-0-10-1-on-ubuntu-12-04
tags:
- CLI
- KVM
- Libvirt
- Linux
- Ubuntu
- Virtualization
title: Compiling libvirt 0.10.1 on Ubuntu 12.04
url: /2012/09/07/compiling-libvirt-0-10-1-on-ubuntu-12-04/
wordpress_id: 2819
---

I guess I'm a glutton for punishment, because after [successfully compiling libvirt 0.10.1 on CentOS 6.3][1], I came back to try it again on Ubuntu (my first attempt was _not_ successful). Here is the process I followed to get libvirt 0.10.1 compiled and running on Ubuntu 12.04.

I started with a clean install of Ubuntu 12.04, then installed KVM and Open vSwitch per [these instructions][2]. At the end of those instructions, I had an Ubuntu Server 12.04.1 system running KVM and libvirt 0.9.8.

Once I verified the system was working as expected (I ran a few `virsh` commands to check for errors), I followed these steps:

1. As with the CentOS effort, I first downloaded the tarball for libvirt 0.10.1 from the [libvirt.org HTTP server](http://libvirt.org/sources/).

2. Next, I extracted the files using `gunzip -c libvirt-0.10.1.tar.gz | tar xvf -`.

3. Using `apt-get`, I installed the following packages: libxml2-dev, libgnutls-dev, libyajl-dev, libnl-dev, pkg-config, libdevmapper-dev, libcurl4-gnutls-dev, and python-dev. (Note that you can omit the libcurl4-gnutls-dev package if you don't want ESX support in libvirt.) Just like with the CentOS procedure, I determined the necessary list of packages by repeatedly running the `configure` command.

4. With all the dependencies now satisfied, I ran `configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --with-esx=yes` from the extracted libvirt-0.10.1 directory. I specified these particular directories because that matched where the existing binaries were on the system (there's probably an easier/better way).

5. The above command results in a libvirt that supports KVM, OpenVZ, LXC, etc., including ESX. You'd have to add the `--with-xen` parameter to the `configure` command to include support for Xen; that might also change the list of dependencies that need to be installed first.

6. After the `configure` command completed successfully, it was very straightforward to finish the compilation. Just run `make` followed by `make install`.

7. Finally, I restarted libvirtd with `initctl stop libvirt-bin` and `initctl start libvirt-bin`. I probably could have used `initctl restart`, but I wanted to see the shutdown and startup separately. In this case, the libvirt daemon shutdown and started back up cleanly and without any errors.

From there, I was able to run `libvirtd --version` or `virsh --version` to verify that the system was, in fact, running libvirt 0.10.1.

The one big difference between this effort and the CentOS effort earlier was that Open vSwitch was installed and working on this system, so I ran `ovs-vsctl show` to double-check the OVS configuration and operation. I noted that the system had re-created the default bridge, so I got rid of that with these two commands:

    virsh net-destroy default
    virsh net-autostart --disable default

That removed `virbr0` from the OVS configuration, leaving only the OVS bridge that I created during the KVM+Open vSwitch installation earlier.

Now that my test system has both OVS and a version of libvirt that supports OVS, the next steps will be to conduct some more in-depth libvirt+OVS tests and document the results. Stay tuned!

[1]: {{< relref "2012-09-06-compiling-libvirt-0-10-1-on-centos-6-3.md" >}}
[2]: {{< relref "2012-08-17-installing-kvm-and-open-vswitch-on-ubuntu.md" >}}
