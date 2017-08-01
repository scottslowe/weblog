---
author: slowe
categories: Tutorial
comments: true
date: 2013-01-22T13:59:15Z
slug: using-mock-to-build-libvirt-1-0-1-rpms-for-centos-6-3
tags:
- CentOS
- CLI
- Libvirt
- Linux
- OSS
- Virtualization
title: Using Mock to Build Libvirt 1.0.1 RPMs for CentOS 6.3
url: /2013/01/22/using-mock-to-build-libvirt-1-0-1-rpms-for-centos-6-3/
wordpress_id: 3066
---

In this third post on using Mock to build RPMs for CentOS 6.3, I'm going to show you how to use Mock to build RPMs for Libvirt 1.0.1 that you can install on CentOS. As you'll see later, this post builds on the previous two posts (one on [using Mock to build RPMs for sanlock 2.4][1] and one on [using Mock to build RPMs for libssh2 1.4.1][2]).

Here's a quick overview of the process:

1. Set up Mock and the environment.

2. Install prerequisites into the Mock environment.

3. Build the Libvirt RPMs.

Let's take a closer look at each of these steps.

## Setting Up Mock and the Environment

The first phase in the process is to set up Mock and the environment for building the RPMs. Fortunately, this is relatively simple.

First, activate EPEL. My preferred method for activating the EPEL repository is to download the RPM, then use `yum localinstall` to install it, like this:

    wget http://fedora.mirrors.pair.com/epel/6/i386/\
    epel-release-6-8.noarch.rpm
    yum localinstall epel-release-6-8.noarch.rpm

(Note that I've line-wrapped the URL with a backslash to make it more readable. That line-wrapped command actually works just as it is in the shell as well.)

Next, you'll need to install Mock and related RPM-building tools:

    yum install fedora-packager

Third, create a dedicated user for building RPMs. I use "makerpm" as my username, but you could use something else. Just make sure that the name makes sense to you, and that the new user is a member of the `mock` group:

    useradd makerpm -G mock
    passwd makerpm

From this point on, you'll want to be running as this user you just created, so switch to that user with `su - makerpm`. This ensures that the RPMs are built under this dedicated user account.

The final step in setting up Mock and the build environment is to run the following command while running as the dedicated user you created:

    rpmdev-setuptree

Now you're ready to move on to the next phase: installing prerequisites into the Mock environment.

## Installing Prerequisites Into the Mock Environment

One of the great things about Mock is that it creates an isolated chroot into which it installs all the necessary prerequisites for a particular package. This helps ensure that the package's dependencies are managed correctly. However, if you are trying to build a package where dependencies don't exist in the repositories, then you have to take a few additional steps. When you're trying to build libvirt 1.0.1 RPMs for use with CentOS 6.3, you'll find yourself in _exactly_ this situation. Libvirt has dependencies on newer versions of sanlock-devel and libssh2-devel than are available in the repositories.

Fortunately, there is a workaround---and here's where those other posts on Mock will come in handy. Use the instructions posted [here][1] to build RPMs for sanlock 2.4, and use the instructions [here][2] to build RPMs for libssh2 1.4.1.

Once the RPMs are built (and they should build without any major issues, based on my testing), then use these commands to get them into the isolated Mock environment (I've line-wrapped here with backslashes for readability):

    mock -r epel-6-x86_64 --init
    mock -r epel-6-x86_64 --install \
    ~/rpmbuild/RPMS/sanlock-lib-2.4-3.el6.x86_64.rpm \
    ~/rpmbuild/RPM/sanlock-devel-2.4-3.el6.x86_64.rpm \
    ~/rpmbuild/RPMS/libssh2-1.4.1-2.el6.x86_64.rpm \
    ~/rpmbuild/RPMS/libssh2-devel-1.4.1-2.el6.x86_64.rpm

This will install these packages into the Mock environment, **not onto the general Linux installation.**

Once you've gotten these packages compiled and installed, then you're ready for the final phase: building the libvirt RPMs.

## Building the Libvirt RPMs

As in my earlier post on [compiling Libvirt 1.0.1 RPMs for CentOS 6.3][3], this final step is almost anti-climactic. That's good, though, because it means you've done all the previous steps perfectly.

First, fetch the source RPM from the libvirt HTTP server:

    wget http://libvirt.org/sources/libvirt-1.0.1-1.fc17.src.rpm

Next, move the source RPM into the `~/rpmbuild/SRPMS` directory:

    mv libvirt-1.0.1-1.fc17.src.rpm ~/rpmbuild/SRPMS

Finally, run Mock to rebuild the RPMs:

    mock -r epel-6-x86_64 --no_clean \
    ~/rpmbuild/SRPMS/libvirt-1.0.1-1.fc17.src.rpm

Note that the `--no-clean` parameter is required here to prevent Mock from cleaning out the chroot and getting rid of the packages you installed into the environment earlier.

This command should run without any errors or problems, and produce a set of RPMs (typically) found in `/var/lib/mock/epel-6-x86_64/results`. You can then take these RPMs and install them on another CentOS 6.3 system using `yum localinstall`.

## Testing the RPMs

To verify that everything worked as expected, I tested the RPMs using these steps:

1. Using a clean CentOS 6.3 VM (built using the "Minimal Desktop" option), I used `yum groupinstall` to install the Virtualization, Virtualization Client, Virtualization Platform, and Virtualization Tools groups. This installed version 0.9.10 of libvirt.

2. I then installed the updated version of libvirt using `yum localinstall`. I had to specify the dependencies manually on the command line; I anticipate that had I been using a real repository, this would not have been the case. The updated libvirt, sanlock, and libssh2 packages all installed correctly.

3. I started the libvirtd service (it worked), and ran `virsh --version`. It returned version 1.0.1.

I imagine there might be more comprehensive/better ways of testing the RPMs that I built, but they seemed to work fine on my end. If anyone has any other suggestions for how we can double-check to ensure the packages are working correctly, feel free to speak up in the comments below. I also welcome any other corrections, suggestions, or questions in the comments. Courteous comments are always welcome.

[1]: {{< relref "2013-01-22-using-mock-to-build-sanlock-2-4-rpms-for-centos-6-3.md" >}}
[2]: {{< relref "2013-01-22-using-mock-to-build-libssh2-1-4-1-rpms-for-centos-6-3.md" >}}
[3]: {{< relref "2013-01-16-building-libvirt-1-0-1-rpms-for-centos-6-3.md" >}}
