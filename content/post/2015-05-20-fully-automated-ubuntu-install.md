---
author: slowe
categories: Tutorial
comments: true
date: 2015-05-20T12:20:00Z
tags:
- Linux
- Ubuntu
- Networking
title: Building a Fully Automated Ubuntu Installation Process
url: /2015/05/20/fully-automated-ubuntu-install/
---

Recently on Twitter, I mentioned that I had managed to successfully create a fully automated process for installing [Ubuntu Server][link-6] 14.04.2, along with a method for bootstrapping [Ansible][link-5]. In this post, I'm going to describe the installation process I built and the components that went into making it work. I'll discuss the Ansible bootstrap process in a separate post. I significantly doubt that there is anything new or unique here, but hopefully this information will prove helpful to others facing similar challenges.

Before I continue, allow me to briefly discuss why I didn't use a system like  [Cobbler][link-2] instead of putting together my own system. Cobbler is a great tool. For me, though, this was also about deepening my own knowledge. I wanted to better understand the various components involved and how they interacted, and I didn't feel I would really be able to do that with a "prebuilt" system like Cobbler. If you are more interested in getting something up and running as opposed to learning more about how it works (and that's OK), then I'd recommend you skip this post and go download Cobbler. If, on the other hand, you want to make this into more of a learning process, then read on!

## Overview

There are 3 major components to this automated installation process:

1. PXE Boot Infrastructure
2. Local HTTP source of the Ubuntu 14.04 installation files
3. Ubuntu Preseed File

I'll describe each of these below in varying levels of detail. All of these items are already reasonably well-documented elsewhere on the Internet, and I'll include resources that I found helpful in each section. However, to avoid mass duplication, I'm going to try to keep this high-level, refer you to other useful resources, and point out the potholes I found along the way.

## PXE Boot Infrastructure

Getting the PXE boot infrastructure up and running is pretty straightforward; there aren't really a lot of "gotchas" to trip you up along the way. One article I personally found helpful was [this write-up by Brian Carpio][link-3] (I like the way he organized things in his configuration), but this is just one of many different articles that provide an overview of how to set up a PXE boot infrastructure.

I chose to set up a new Ubuntu 14.04.2 VM that would run TFTP (via the `tftpd-hpa` package) and HTTP (via Apache). I already had an existing ISC DHCP server instance, so I modified that configuration to point to the PXE server (via the `filename` and `next-server` options in the DHCP configuration). On this server I set up the PXE boot files in a way that was similar to how Brian Carpio described in his post:

* Linux kernel and initial RAM disk image copied into the `pxe` subdirectory of the TFTP root directory (which on my system was `/var/lib/tftpboot`)
* The `pxelinux.0` file in the TFTP root directory
* A menu module (`vesamenu.c32`) and a menu configuration file (more on that in a moment) in the `pxelinux.cfg` folder off the TFTP root directory

There are a lot of different examples of menu configuration files out there, but nothing that I found did a very good job of explaining the various options or how those options may (or may not) affect the Ubuntu installer. In the end, I found that there were a few options that _had_ to be included in the menu configuration file---even if they were also in the preseed file, which I'll discuss in the next section---in order for the installation to be fully automated. Here's a snippet from the menu configuration file; this is for a generic Ubuntu 14.04 installation:

    LABEL generic
        MENU LABEL Install generic Ubuntu 14.04
        KERNEL pxe/ubuntu/ubuntu-14.04-x86_64
        IPAPPEND 1
        APPEND initrd=pxe/ubuntu/ubuntu-14.04-x86_64.img ksdevice=eth0 \
        locale=en_US.UTF-8 keyboard-configuration/layoutcode=us \
        interface=eth0 hostname=unassigned \
        url=http://192.168.100.240/preseed/14.04-generic-nolvm.cfg \
        live-installer/net-image=http://192.168.100.240/ubuntu/14.04.2/install/filesystem.squashfs

(Note I've line-wrapped and added backslashes to improve readability.)

Let's break this down just a bit:

* The `LABEL` and `MENU LABEL` commands are used in building and displaying the menu
* The `KERNEL` line specifies the Linux kernel to be booted; this was copied to the PXE server (the path you see in this example is relative to the root path of the TFTP server)
* The `IPAPPEND 1` line passes some additional networking information to the installer.

The `APPEND` line, though, is where it really gets complicated. Let's take a closer look:

* The `initrd=` portion points to the initial RAM disk image copied over from the install CD. I copied this into the same `pxe` subdirectory as the Linux kernel image.
* `ksdevice=eth0` tells the installer to use eth0 for network connectivity.
* The `locale=en_US.UTF-8` and `keyboard-configuration/layoutcode=us` parameters were required to prevent the Ubuntu installer from prompting for those values.
* The `interface=eth0` parameter prevented the Ubuntu installer from prompting for the network interface to use. Specifying this in the preseed file wasn't enough.
* The `hostname=unassigned` parameter ensured the Ubuntu installer wouldn't prompt for hostname (even though the hostname was already provided in the preseed file, as you'll see shortly).
* The `url` value tells the installer where to get the preseed file (via HTTP, in this case).
* The `live-installer/net-image` parameter was a last-minute fix obtained from [this site][link-4]. Without this parameter, the installation will error out and won't continue.

Even though many of the options on the `APPEND` line were also included in the Ubuntu preseed file, I found it was still necessary to include them here in order to get a fully automated installation.

In my particular environment, I wanted a fully customized installation routine for each server, so I customized the menu commands to provide specific hostnames and references to specific preseed files (this is in addition to the generic install routine shown above). This allows me to boot a server, select the appropriate configuration from the PXE boot menu, and in a few minutes have an appropriately configured Ubuntu 14.04.2 instance with the right hostname and other settings already baked in.

## Local HTTP Source

At this point, you have a fully functional PXE boot environment, but no way of serving up the installation files. You could also use an existing HTTP server instance if you have one, and just adjust the URL provided in the menu command (see previous section) and the preseed file (see next section). I chose to install the Apache web server (via `apt-get install apache2`) on the same server that's providing the PXE boot infrastructure. Either way, once you have an HTTP server up and running, you need to make the installation files available via the web server. 

There's a couple of different ways to accomplish this task: you can copy the files from the installation media, or mount the installation media (ISO) at a mount point under the web server's document root. I chose the second path:

    mount -o loop ubuntu-14.04.2-server-amd64.iso /var/www/html/ubuntu/14.04.2

Obviously, you'll want to provide the correct filenames and/or paths in that mount command, and you'll also want to edit `/etc/fstab` if you want the ISO mounted automatically when you (re)boot the server.

## Preseed File

There's only one piece missing now: the preseed file that will automate the installation of Ubuntu. You've built a PXE boot infrastructure and you've got an HTTP server available to serve up both the preseed file (as provided by the `url` parameter in the PXE menu command) and the installation files.

To keep this post from getting too much longer I've created [a GitHub Gist][link-7] that contains a sanitized version of the preseed file I'm using. I do want to call out a few things about this preseed file:

* You'll note some duplication between information provided in the preseed file and the parameters on the `APPEND` line of the PXE menu command. This is not a mistake---I found that you actually had to include them in both places. (For example, lines 6-8 and line 15 are found in both places).
* Lines 12 and 13 tell the Ubuntu installer where to find the installation files. This should correspond to the information for the HTTP server you set up in the previous section (and where the installation files were made available on that HTTP server).
* Line 15 should again correspond to the correct URL (server and path) for where the initial filesystem is available.
* The partitioning recipe (lines 28 through 53) is for an LVM-based system. It creates a primary partition for /boot, followed by a logical volume for / and a logical volume for swap. There aren't a lot of great resources out there on the partitioning recipes, but I did find [this documentation][link-8] and [this example][link-9] of how to create partitioning recipes. I settled on this particular recipe after a fair amount of trial and error. (This recipe is intended to be used with a small physical disk---in my case, a low-cost 32GB SSD).
* The `preseed/late_command` option also deserves a bit of explanation. When you PXE boot and install over HTTP from an internal server, the `/etc/apt/sources.list` file contains _only_ references to the internal server---not to the "usual" repositories. The first `wget` command fixes that. The second `wget` command installs a customized `/etc/apt/apt.conf` that specifies an Apt proxy (using apt-cacher-ng).

Clearly, you'll want to customize the preseed file to match your environment (for example, to provide the correct hostname, domain name, URLs, etc.). In my case, I created multiple, customized preseed files (one for each server I knew I wanted to build). Each preseed file has the correct hostname, volume group and logical volume names, etc. The various PXE menu commands (each server I wanted to build has its own PXE boot menu option) then reference the specific preseed for that particular server.

You'll host these preseed files on the HTTP server you set up earlier.

## Pulling it all Together

With the addition of the preseed file, you now have all the components you need for a fully automated installation. Let's see how all this comes together:

1. You boot a new server, and it obtains an IP address from a DHCP server (which you've configured, via the `filename` and `next-server` options, to direct the new server to the PXE server).
2. The new server retrieves the Linux kernel and initial RAM disk image (via TFTP using the network device specified in the `ksdevice` option) and boots up.
3. The installer retrieves the preseed file from the internal HTTP server (using the URL provided in the `url` parameter of the `APPEND` line) and parses the preseed file to see how the installation should proceed.
4. The preseed file tells the installer to retrieve the installation files from the internal HTTP server (via the `d-i mirror/http/hostname` and `d-i mirror/http/directory` directives).
5. The installer installs the system, using the internal HTTP server as the source mirror and using the preseed file to answer any questions along the way.
6. At the end of the installation, the `preseed/late-command` runs and copies over two files (hosted on the internal HTTP server) to the newly-built Ubuntu installation.
6. When the installer completes, the new server reboots. You're done!

## Summary

In this post I've tried to describe some of the lessons I learned in building a fully automated Ubuntu installation process. Hopefully this post proves helpful to others seeking to accomplish the same task. In a future post I will describe the method I created to take newly-built servers installed using this process and bootstrap them into Ansible.

Thanks for reading!

[link-1]: https://www.unix-ag.uni-kl.de/~bloch/acng/
[link-2]: http://cobbler.github.io/
[link-3]: http://www.briancarpio.com/2012/04/04/system-automation-part-1/
[link-4]: http://www.michaelm.info/blog/?p=1378
[link-5]: http://www.ansible.com/home
[link-6]: http://www.ubuntu.com/server
[link-7]: https://gist.github.com/scottslowe/9116c0bf80f931a5eca2
[link-8]: http://ftp.dc.volia.com/pub/debian/preseed/partman-auto-recipe.txt
[link-9]: https://wikitech.wikimedia.org/wiki/Partman_Recipes
