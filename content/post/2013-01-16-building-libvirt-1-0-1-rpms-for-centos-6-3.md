---
author: slowe
categories: Tutorial
comments: true
date: 2013-01-16T17:27:36Z
slug: building-libvirt-1-0-1-rpms-for-centos-6-3
tags:
- CentOS
- CLI
- Libvirt
- Linux
- OSS
- Virtualization
title: Building Libvirt 1.0.1 RPMs for CentOS 6.3
url: /2013/01/16/building-libvirt-1-0-1-rpms-for-centos-6-3/
wordpress_id: 3052
---

In previous articles, I've shown you how to [compile libvirt 0.10.1 on CentOS 6.3][1], but---as several readers have pointed out in the comments to that and other articles---compiling packages from source may not be the best long-term approach. Not only does it make it difficult to keep the system up-to-date, it also makes automating the configuration of the host rather difficult. In this post, I'll show you how to rebuild a source RPM for libvirt 1.0.1 so that it will install (and work) under CentOS 6.3. (These instructions should work for RHEL 6.3, too, but I haven't tested them.)

## Overview

The process for rebuilding a source RPM isn't too terribly difficult, assuming that you can get the dependencies worked out. Here's a quick look at the steps involved:

1. Create a set of build directories for source RPMs.

2. Download the source RPM and install all prerequisites/dependencies onto the system.

3. Rebuild the source RPM for the destination system.

Let's take a look at each of these steps in a bit more detail.

## Create the Build Environment

The CentOS wiki has [a great page](http://wiki.centos.org/HowTos/SetupRpmBuildEnvironment) for how to set up an RPM build environment. I won't repeat all the steps here (refer to the wiki page instead), but here's a quick summary of what's involved:

1. Install the necessary packages (typically you'll need the rpmbuild, redhat-rpm-config, make, and gcc packages). You might also need certain development libraries, but this will vary according the source RPMs you're rebuilding (more on that in the next section).

2. Create the necessary directories under your home directory.

I'll assume that you've followed the steps outlined in [the CentOS wiki](http://wiki.centos.org/HowTos/SetupRpmBuildEnvironment) to set up your environment appropriately before continuing with the rest of this process.

## Download the Source RPM and Install Prerequisites

The libvirt 1.0.1 source RPMs are available directly from the libvirt HTTP server, easily downloaded with `wget`:

    wget http://libvirt.org/sources/libvirt-1.0.1-1.fc17.src.rpm

You can just download the source RPM to your home directory. Before you can build a new RPM from the source RPM, though, you'll first need to install all the various prerequisites that libvirt requires. Most of them can be installed easily using `yum` with a command like this:

    yum install xhtml1-dtds augeas libudev-devel \
    libpci-access-devel yajl-devel, libpcap-devel libnl-devel \
    avahi-devel radvd ebtables qemu-img iscsi-initiator-utils \
    parted-devel device-mapper-devel numactl-devel netcfg-devel \
    systemtap-sdt-devel scrub numad libblkid-devel

There are two dependencies, though---sanlock and libssh2--that require versions newer than what are available in the CentOS/EPEL repositories. For those, you'll need to recompile your own RPMs. Fortunately, this is pretty straightforward. The next two sections provide more details on getting these prerequisites handled.

### Building an RPM for sanlock

To build a CentOS 6.3 RPM for version 2.4 of sanlock (the minimum version needed by libvirt 1.0.1), first use `wget` to download a Fedora 17 version of the source RPM. I've wrapped the URL with a backslash for readability:

    wget http://dl.fedoraproject.org/pub/fedora/linux/updates/17/SRPMS\
    /sanlock-2.4-3.fc17.src.rpm

Next, install an prerequisite library using `yum install libaio-devel`.

Finally, use `rpmbuild` to rebuild the sanlock source RPM:

    rpmbuild --rebuild sanlock-2.4-3.fc17.src.rpm

This process should proceed without any problems. The resulting RPMs that are created will be found in `~/rpmbuild/RPMS/x86_64` (assuming you are, as I am, using a 64-bit build of CentOS).

Building the RPMs, however, isn't enough---you need to install them so that you can build the libvirt RPMs. So install the sanlock-devel and sanlock-lib RPMs using `yum locainstall` (do this command from the directory where the RPMs reside):

    yum localinstall sanlock-devel-* sanlock-lib-*

That should take care of the sanlock dependency.

### Building an RPM for libssh2

To build a CentOS 6.3 RPM for version 1.4.1 of libssh2 (libvirt 1.0.1 requires at least version 1.3.0), first download the source RPM using `wget` (I've wrapped the URL here for readability):

    wget http://dl.fedoraproject.org/pub/fedora/linux/releases\
    /17/Everything/source/SRPMS/l/libssh2-1.4.1-2.fc17.src.rpm

(That's a lowercase L in the URL just after SRPMS.)

Once you have the source RPM downloaded, then just rebuild the source RPM:

    rpmbuild --rebuild libssh2-1.4.1-2.fc17.src.rpm

Then install the resulting RPMs using `yum localinstall`:

    yum localinstall libssh2-1.4.1-* libssh2-devel-*

That takes care of the last remaining dependency. You're now ready to compile the RPMs for libvirt 1.0.1.

## Build the Libvirt RPM

This part is almost anticlimactic. Just use the `rpmbuild` command:

    rpmbuild --rebuild libvirt-1.0.1-1.fc17.src.rpm

If you've successfully installed all the necessary prerequisites, then the RPM compilation process should proceed without any issues.

Once the RPM compilation process is complete, you'll find libvirt 1.0.1 RPMs in the `~/rpmbuild/RPMS/x86_64` directory (assuming a 64-bit version of CentOS) which you can easily install with `yum localinstall`, or post to your own custom repository.

I hope this post helps someone. If you have any questions, or if you spot an error, please speak up in the comments below. All courteous comments are welcome!

[1]: {{< relref "2012-09-06-compiling-libvirt-0-10-1-on-centos-6-3.md" >}}
