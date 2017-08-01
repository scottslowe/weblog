---
author: slowe
categories: Tutorial
comments: false
date: 2005-11-04T09:48:20Z
slug: openbsd-pcn0-driver-issue-resolved
tags:
- BSD
- Networking
- Virtualization
- UNIX
title: OpenBSD pcn0 Driver Issue Resolved
url: /2005/11/04/openbsd-pcn0-driver-issue-resolved/
wordpress_id: 112
---

Well, sort of resolved. I was never able to make the `pcn` driver (from [OpenBSD][1] 3.8) actually work under [VMware][2], but I did find information on how to disable the `pcn` driver and revert to the older `le` driver.

This [archived Neohapsis discussion][3], followed by a quick e-mail exchange with the author of the thread (who, thankfully, was very responsive and very helpful) led me to the solution. Some of it I had to improvise on the fly, but here's the overall process:

1. Boot from the OpenBSD 3.8 boot CD image (I pointed the virtual CD-ROM drive in the VM directly to the corresponding ISO image).

2. At the OpenBSD `boot>` prompt, type "-c" (without quotes) and press Enter. This takes you into User Kernel Config, or UKC.

3. At the UKC prompt, type "disable pcn" to disable the `pcn` driver.

4. Type "quit" at the next UKC prompt to exit the kernel config and proceed with the boot process. If you watch the boot process, you will see OpenBSD load the `le` driver and identify the virtual NIC as `le1`.

That's all well and good, but how do you make the changes stick between reboots? Here's how.

1. Once you've gotten OpenBSD fully installed and are rebooting for the first time after installation, follow the steps above to use the `le` driver for the next reboot.

2. Use the "config -e -o nbsd bsd" command (see the relevant [man page][4] for details) to modify the kernel again, only this time saving the changes to the file named `nbsd`.

3. Upon the next reboot after using the config command to create a new kernel file, specify the name of that new kernel file (`nbsd` in our example here) at the OpenBSD `boot>` prompt.

4. Assuming that everything works OK (it did for me), rename the original kernel to "bsd.original" and rename your new kernel to just "bsd". Then, upon the next reboot, the `pcn` driver should be disabled and everything should work just fine.

Using this process, I now have two OpenBSD 3.8 VMs running and haven't experienced any issues. Now, on to installing [ClamAV][5] on OpenBSD...._(more details soon)_

**UPDATE:** The `pcn0` network driver from OpenBSD 3.9 works perfectly under VMware (at least, on ESX Server). I have posted more information [here][6].

[1]: http://www.openbsd.org/
[2]: http://www.vmware.com/
[3]: http://archives.neohapsis.com/archives/openbsd/2005-08/1314.html
[4]: http://www.openbsd.org/cgi-bin/man.cgi?query=config&apropos=0&sektion=0&manpath=OpenBSD+3.8&arch=i386&format=html
[5]: http://www.clamav.net/
[6]: {{< relref "2006-05-18-openbsd-39-on-esx-server.md" >}}