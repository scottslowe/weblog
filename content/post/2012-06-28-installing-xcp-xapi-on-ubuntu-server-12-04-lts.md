---
author: slowe
categories: Tutorial
comments: true
date: 2012-06-28T10:19:51Z
slug: installing-xcp-xapi-on-ubuntu-server-12-04-lts
tags:
- Linux
- Ubuntu
- Virtualization
- Xen
title: Installing XCP-XAPI on Ubuntu Server 12.04 LTS
url: /2012/06/28/installing-xcp-xapi-on-ubuntu-server-12-04-lts/
wordpress_id: 2654
---

As part of my 2012 projects (see [here][1] plus an update [here][2]), I've been familiarizing myself with the Xen hypervisor. To that end, I've been working to get XCP (Xen Cloud Platform, the open source version of XenServer) running on a test system in my home office. It's been a bit of a struggle, but I think I've finally got it, and I wanted to share the information here.

Here are the steps that I followed to install XCP-XAPI on a system running Ubuntu Server 12.04 LTS. Much of the credit goes to [Project Kronos](http://wiki.xen.org/wiki/Project_Kronos) and this page in particular on [the XCP toolstack on a Debian-based installation](http://wiki.xen.org/wiki/XCP_toolstack_on_a_Debian-based_distribution). While the XCP toolstack page is quite helpful, I found that the instructions were confusing (for me, at least). Hence, I'm writing up my steps in the hopes that they will prove useful to someone else.

## Before Installing XCP-XAPI

Before I started the installation of the XCP-XAPI packages, I first made sure that all the various networking interfaces were configured correctly and working as expected. For my particular system (a Dell Latitude E6400), this meant installing some proprietary Broadcom wireless firmware (using the `firmware-b43-lpphy-installer` package) and configuring `/etc/network/interfaces`. I also ran `apt-get update` and `apt-get upgrade` to install the latest versions of all packages. Credit goes to [this page](http://www.linuxhomenetworking.com/wiki/index.php/Quick_HOWTO_:_Ch13_:_Linux_Wireless_Networking#Debian_.2F_Ubuntu) for instructions on how to configure the wireless NIC from the CLI, with one small correction. In the `/etc/network/interfaces` file, I had to use this as the pre-up parameter (the example on the page above used "-Bw" instead of "-B"):

    pre-up wpa_supplicant -B -Dwext -iwlan0  
     -c/etc/wpa_supplicant/wpa_supplicant.conf

(Naturally you'd write that all as one line; it's broken here for readability.)

## Installing XCP-XAPI

First, I edited `/etc/apt/sources.list` to add repositories for the XCP-XAPI packages. Although Ubuntu 12.04 LTS provides XCP-XAPI packages, the Project Kronos page indicated that there were dependency issues, so I went with these packages instead. If anyone has any experience with the Ubuntu-provided XCP-XAPI packages, please let me know.

I added these lines:

	deb http://ppa.launchpad.net/ubuntu-xen-org/xcp-unstable/ubuntu precise main  
	deb-src http://ppa.launchpad.net/ubuntu-xen-org/xcp-unstable/ubuntu precise main

Then I ran this command:

    apt-key adv --keyserver keyserver.ubuntu.com --recv-keys 9273A937

That was followed by:

    apt-get update
    apt-get install xcp-xapi

From there, `apt-get` started fetching and installing the necessary packages (and there are quite a few). When prompted, I selected to use OpenvSwitch as the networking backend (instead of standard Linux bridging).

Before rebooting---which you'll need to do in order to boot into the Xen kernel---I also performed the following steps.

1. I edited the `/etc/init.d/xend` file to prevent xend from starting up. I did this using two `sed` commands. First, I ran `sed -i -e 's/xend_start$/#xend_start/' /etc/init.d/xend`; that command was followed by `sed -i -e 's/xend_stop$/#xend_stop/' /etc/init.d/xend`. Finally, I ran `update-rc.d xendomains disable`.

2. I set the XAPI toolstack as the default by editing `/etc/default/xen` and changing the line `TOOLSTACK=""` to `TOOLSTACK="xapi"`.

3. I made the Xen kernel the default grub entry by editing `/etc/default/grub` and setting `GRUB_DEFAULT="Xen 4.1-amd64"`. Then I ran `update-grub`.

4. I added a symbolic link from `/usr/share/qemu-linaro/keymaps` to `/usr/share/qemu/keymaps`.

At this point, the system is ready for a reboot.

## Verifying that XCP-XAPI Works

I'm still working on a more comprehensive set of tests, but some basic tests should tell you whether the system is working or not:

1. Run `xe host-list`. It should return a single host, which is the system you just built.

2. Run `xe vm-list`. The information returned should list a single VM described as the control domain on the host.

3. Run `xe network-list`. You should get back a network for each network interface, plus another network for XenAPI communications.

4. Run `xe sr-list`. You should get back a single SR (storage repository) used for XenServer Tools ISOs.

(By the way, note that `xe` requires a username and password, even if you're running it against the local host. I found it easiest to create a password file and then create an alias for `xe` that included the `-pwf` parameter.)

I have future posts planned that will talk more about networking and SRs with this Ubuntu-based XCP-XAPI system, so stay tuned for those. Also, if you have any tips or tricks for making this process easier (or if I've stated something incorrectly), please speak up in the comments. Thanks!

[1]: {{< relref "2012-01-02-some-projects-for-2012.md" >}}
[2]: {{< relref "2012-06-25-mid-year-project-update.md" >}}
