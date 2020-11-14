---
author: slowe
categories: Tutorial
comments: true
date: 2010-03-17T20:31:20Z
slug: a-quick-and-simple-guide-to-building-an-openbsd-vm
tags:
- BSD
- Fusion
- UNIX
- Virtualization
- VMware
title: A Quick and Simple Guide to Building an OpenBSD VM
url: /2010/03/17/a-quick-and-simple-guide-to-building-an-openbsd-vm/
wordpress_id: 1864
---

I'll start this off with a disclaimer: this post is really more for my own benefit than the benefit of anyone else.

[OpenBSD](http://www.openbsd.org) is my OS of choice when it comes to setting up a quick, simple UNIX-based virtual machine (VM). Need a virtual firewall? Use OpenBSD. Need a router? Use OpenBSD. Need a web server or an FTP server? Use OpenBSD. Need to run some network security tools? Use OpenBSD.

The problem is this: once I get an OpenBSD system up and running, it runs so well that I rarely have to go set up another one. Because there is then this length of time between installations, I always find myself forgetting the steps to take when installing an OpenBSD system. Thus, the need for this post and why I say it's really for my benefit more than anything else. Next time I need to install OpenBSD in a VM for some reason, I can quickly come back and reference my list. (I will say that the installation of OpenBSD in recent versions has gotten _much_ simpler than it was in the past.)

Oh, another disclaimer is probably necessary here, too: this is **_not_** to be considered some sort of "best practices" guide, so please don't hammer the comments with stuff like "You know, you really should...". This is just a quick and simple setup.

With those disclaimers out of the way, here's the installation procedure. This was written for use with OpenBSD 4.6:

1. Boot from the OpenBSD installation ISO image. When prompted, choose "i" to install.

2. Press Enter for the default keyboard layout (unless you need a different layout, naturally).

3. Enter the system's hostname in short form.

4. Enter the name of the network interface to configure. When installing on VMware Fusion 3.0.2 on my Macintosh, the default interface is `em0`. On VMware vSphere 4, the default interface is `vic0`.

5. Enter the IPv4 address or press Enter to use DHCP.

6. Enter the IPv6 address or press Enter to not assign an IPv6 address.

7. Press Enter to complete the configuration of network interfaces.

8. Press Enter not to perform any manual network configuration.

9. Enter and confirm the root password.

10. Press Enter to start `sshd` by default.

11. Press Enter not to start `ntpd` by default.

12. Enter "no" to indicate that you will not be running the X Window System.

13. Press Enter not to change the default console to `com0`.

14. Press Enter not to create an additional user. (I generally prefer to create an additional user after installation is complete.)

15. Press Enter to accept the default disk as the root disk. On my Mac running VMware Fusion 3.0.2, the default disk is `wd0`.

16. Press Enter to use the whole disk.

17. Press Enter to use auto layout of partitions on the disk. (I'm not sure what version of OpenBSD added this feature, but it is quite handy for simple installations.)

18. Press Enter to use the CD to install the sets. The CD in the VM should be mapped to the ISO image of the OpenBSD 4.6 install CD.

19. Press Enter to use the default CD (which showed up as `cd0` on my system).

20. Press Enter to use the default path to the sets.

21. Remove the X Window System sets by entering "-x*" and pressing Enter.

22. Verify that the X Window System sets (`xbase46.tgz`, `xetc46.tgz`, `xshare46.tgz`, `xfont46.tgz`, and `xserv46.tgz`) are unselected, then press Enter to complete set selection. OpenBSD will start installing the sets.

23. Enter the timezone, such as "US/Eastern".

24. Enter `reboot` to reboot your new OpenBSD VM. You should now be ready to perform final configuration of OpenBSD, such as using `pkg_add` to install packages or editing `rc.conf.local` to control what daemons are launched at startup. (Of course, those are tasks for an entirely different blog post).

That's it. Again, this not a best practice/ideal installation. It's just a "drop dead simple" installation in a VM for when you need to get something done quickly.
