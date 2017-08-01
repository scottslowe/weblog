---
author: slowe
categories: Tutorial
comments: true
date: 2012-11-05T12:29:13Z
slug: compiling-libvirt-1-0-0-on-ubuntu-12-04-and-12-10
tags:
- Interoperability
- Libvirt
- Linux
- OSS
- Ubuntu
- Virtualization
title: Compiling libvirt 1.0.0 on Ubuntu 12.04 and 12.10
url: /2012/11/05/compiling-libvirt-1-0-0-on-ubuntu-12-04-and-12-10/
wordpress_id: 2924
---

In case you hadn't noticed late last week, the open source virtualization API project [libvirt](http://libvirt.org) released version 1.0.0--a major milestone, obviously. To celebrate the 1.0.0 release, here are instructions for compiling the libvirt 1.0.0 release on Ubuntu 12.04 and 12.10.

(If you need instructions on compiling an earlier release of libvirt on Ubuntu, see [this post][1]. It works for libvirt 0.10.1 and 0.10.2 on Ubuntu 12.04 LTS; I haven't yet tested it on Ubuntu 12.10.)

These instructions assume that you have built a plain-jane Ubuntu system (either 12.04 LTS or 12.10). I tested these instructions on an Ubuntu 12.04.1 LTS VM, freshly built and updated using `apt-get update && apt-get upgrade`, on a freshly-built Ubuntu 12.10 VM (also up to date), and a physical system running an up-to-date instance of Ubuntu 12.04.1 that also had KVM and an earlier version of libvirt installed. These instructions worked in all three cases.

Ready? Let's get started!

1. Download the libvirt-1.0.0.tar.gz tarball from the [libvirt.org HTTP server](http://libvirt.org/sources/).

2. Extract the tarball using `tar -xvzf libvirt-1.0.0.tar.gz`.

3. Using `apt-get`, ensure that the following packages are installed: gcc, make, pkg-config (already installed in my testing), libxml2-dev, libgnutls-dev, libdevmapper-dev, libcurl4-gnutls-dev, and python-dev. (Interestingly enough, earlier versions also required libyajl-dev, but this version didn't seem to need it.) You can probably omit the libcurl4-gnutls-dev package if you don't want ESX support; I did want ESX support, so I included it.

4. From within the extracted libvirt-1.0.0 directory, run the `configure` command. I specified particular directories because that's where the prebuilt binaries from Ubuntu are normally placed, so the final command was `./configure --prefix=/usr --localstatedir=/var --sysconfdir=/etc --with-esx=yes`.

5. The above command does _not_ include Xen support; if you need Xen support, you'll need to add `--with-xen=yes` to the command, and your list of prerequisite packages will change (there are additional packages you'll need to install).

6. Once the `configure` command completes, then complete the process with `make` and `make install`. Note that I used `sudo make install` here because I'm installing files into privileged system locations.

7. Finally, to verify that everything seems to be working as expected, restart libvirt with `initctl stop libvirt-bin` and `initctl start libvirt-bin`. (Yes, you could use `initctl restart`, but this allows you to see the clean stop and start of the libvirt daemon.) As an aside, note that this step only works if you either a) had a previous version of libvirt installed, or b) create the `initctl` job for libvirt. I chose the first approach.

8. As a final verification step, run `virsh --version` or `libvirtd --version` to verify that you're running libvirt 1.0.0.

That's it! Now, as others have pointed out, this will create some potential system administration challenges, in that `apt-get` will still suggest new libvirt packages to install when you try to update the system. I _think_ that there are some commands you can run to keep the manually compiled version instead of the version `apt-get` is suggesting, but I haven't yet determined exactly which commands to use. (If you have more information, please speak up in the comments!)

I have a series of new libvirt-related blog posts in the works, so stay tuned for those. In the meantime, feel free to post any questions, corrections, or clarifications in the comments below. Courteous comments are always welcome.

[1]: {{< relref "2012-09-07-compiling-libvirt-0-10-1-on-ubuntu-12-04.md" >}}
