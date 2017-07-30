---
author: slowe
categories: Tutorial
comments: true
date: 2017-02-07T00:00:00Z
tags:
- Linux
- CLI
- Virtualization
- Fedora
title: Installing VirtualBox 5.1 on Fedora 25
url: /2017/02/07/installing-virtualbox-51-fedora-25/
---

Last fall, I wrote [a piece about why I had switched][xref-1] to [VirtualBox][link-1] (from [VMware Fusion][link-2]) for my Vagrant needs. As part of my switch to [Fedora Linux][link-3] as my primary laptop OS, I revisited my choice of virtualization provider. I'll describe that re-assessment in a separate post; the "TL;DR" for this post is that I settled on VirtualBox. As it turns out, though, installing VirtualBox 5.1 on Fedora 25 isn't as straightforward as one might expect.

After a number of attempts (using a test VM to iron out the "best" procedure), here's the process I found to be the most straightforward:

1. Run `dnf check-update` and `dnf upgrade` to pick up the latest packages. If a new kernel version is installed, reboot. (I know this sounds contrived, but I've run into issues where some kernel-related packages aren't available for the kernel version you're actually running.)

2. Install the [RPMFusion][link-4] repos. You only really need the "free" repository, but you can install the "nonfree" as well if you like (it won't affect this process). I won't go through the process for how to do this; it's really well-documented on the RPMFusion web site and is pretty straightforward.

3. Next, use `dnf` to install some prerequisite packages (note: On my installation of Fedora 25, almost all of these packages were _already_ installed):

        dnf install binutils gcc make patch libgomp glibc-headers glibc-devel

4. Now you're going to install some kernel-related packages. I'm listing these separately because you want to pin the packages that get installed to the current kernel version you're actually running (this sidesteps that issue I mentioned where the available packages don't match the kernel version you're running). I would imagine you could combine these into a single command, if you really wanted to. Here's the additional packages:

        dnf install "kernel-devel-`uname -r`"
        dnf install "kernel-headers-`uname -r`"

5. Now install the "akmods" package using `dnf install akmods`. This will also install a number of dependencies.

6. Finally, you're ready to install VirtualBox itself (note the case on these packages, they _are_ case-sensitive):

        dnf install VirtualBox akmod-VirtualBox

    When this is complete, reboot your system to allow Linux to build and install the VirtualBox kernel modules it needs in order to run correctly. You may note that your next boot takes a bit longer; this is due to the kernel modules getting compiled and installed.

Note that a number of the articles I read on this topic recommended you install the "dkms" package (for dynamically rebuilding kernel modules when the kernel changes), but I found that to be counter-productive. For whatever reason, when you install the "dkms" package, Fedora also installs the debug symbols for the kernel sources. This messes up some paths, and then the kernel modules fail to build. A number of other tutorials recommended using the repository that VirtualBox maintains to distribute VirtualBox, but again I ran into problems with the kernel modules. Using the RPMFusion packages and "akmods" was the most straightforward method I found.

I hope this helps!



[link-1]: https://www.virtualbox.org/
[link-2]: http://www.vmware.com/products/fusion.html
[link-3]: https://getfedora.org/
[link-4]: https://rpmfusion.org/
[xref-1]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
