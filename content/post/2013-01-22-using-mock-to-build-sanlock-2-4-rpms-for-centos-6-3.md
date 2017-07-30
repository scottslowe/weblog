---
author: slowe
categories: Tutorial
comments: true
date: 2013-01-22T11:43:09Z
slug: using-mock-to-build-sanlock-2-4-rpms-for-centos-6-3
tags:
- CLI
- Linux
- OSS
- CentOS
title: Using Mock to Build Sanlock 2.4 RPMs for CentOS 6.3
url: /2013/01/22/using-mock-to-build-sanlock-2-4-rpms-for-centos-6-3/
wordpress_id: 3058
---

The topic of this post might seem a bit strange, but it will all make sense later. In this post, I'll show you how to use Mock to build RPMs for sanlock 2.4 for use with CentOS 6.3.

The information in this post is based on information found in two other very helpful pages:

[Using Mock to test package builds](http://fedoraproject.org/wiki/Using_Mock_to_test_package_builds)  

[How to rebuild a package from Fedora or EPEL for RHEL, CentOS, or SL](https://www.zabbix.org/wiki/Docs/howto/rebuild_rpms)

I tested these instructions on a newly-built CentOS 6.3 VM, installed using the "Minimal Desktop" option. I haven't tested it on other RHEL variants or other versions, so keep that in mind.

First, you'll want to activate EPEL. You'll do that by downloading the RPM and using `yum localinstall` to install it. You can also use `rpm` to install it directly from the URL, but I prefer using `yum localinstall`. (Note that the URL for the EPEL RPM is line-wrapped here for readability.)

    wget http://fedora.mirrors.pair.com/epel/6\
    i386/epel-release-6-8.noarch.rpm
    yum localinstall epel-release-6-8.noarch.rpm

Once EPEL is installed, then install Mock and related tools:

    yum install fedora-packager

This will download and install Mock and related tools such as rpmbuild.

Next, create a user under which you'll run all these commands, and make sure this account is a member of the `mock` group:

    useradd makerpm -G mock
    passwd makerpm

From here on, you'll want to be running as this user you just created, so switch to that user with `su - makerpm`.

The first step while running as the user you created is to setup the RPM build environment:

    rpmdev-setuptree

Now that the directory structure is created, use `wget` to download the source RPM for sanlock 2.4-3 from the Fedora 17 update repository (the URL is line-wrapped here for readability):

    wget http://dl.fedoraproject.org/pub/fedora/linux/updates\
    /17/SRPMS/sanlock-2.4-3.fc17.src.rpm

Move the source RPM to the `rpmbuild/SRPMS` directory:

    mv sanlock-2.4-3.fc17.src.rpm ~/rpmbuild/SRPMS

And, finally, rebuild the RPMs with `mock`:

    mock --rebuild ~/rpmbuild/SRPMS/sanlock-2.4-3.fc17.src.rpm

Assuming everything completes successfully (it did on my CentOS 6.3 VM), then you'll end up with a group of RPMs found in `/var/lib/mock/epel-6-x86_64/results` (the exact directory will vary based on OS version and build; I was using 64-bit CentOS 6.3). You should then be able to install those RPMs onto a CentOS 6.3 system using `yum localinstall` and the prerequisites will be managed properly.

Enjoy!
