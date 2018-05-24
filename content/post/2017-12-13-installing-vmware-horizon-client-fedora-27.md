---
author: slowe
categories: Tutorial
comments: true
date: 2017-12-13T21:30:00Z
tags:
- VMware
- VDI
- Linux
- Fedora
title: Installing the VMware Horizon Client on Fedora 27
url: /2017/12/13/installing-vmware-horizon-client-fedora-27/
---

In this post, I'll outline the steps necessary to install the VMware Horizon client for Linux on [Fedora][link-3] 27. Although VMware provides an "install bundle," the bundle does not, unfortunately, address any of the prerequisites that are necessary in order for the Horizon client to work. Fortunately, some other folks shared their experiences, and building on their knowledge I was able to make it work. I hope that this post will, in turn, help others who may find themselves in the same situation.<!--more-->

Based on information found [here][link-1] and [here][link-2], I took the following steps before attempting to install the VMware Horizon client for Linux:

1. First, I installed the libpng12 package using `sudo dnf install libpng12`.

2. I then created a symbolic link for the `libudev.so.0` library that the Horizon client requires:

        sudo ln -s /usr/lib64/libudev.so.1 /usr/lib64/libudev.so.0

3. I created a symbolic link for the `libffi.so.5` library the Horizon client expects to have available:

        sudo ln -s /usr/lib64/libffi.so.6 /usr/lib64/libffi.so.5

With these packages and symbolic links in place, I proceeded to install the VMware Horizon client using the install bundle downloaded from the public VMware web site (for version 4.6.0 of the client). Per the guidelines in [this GitHub gist][link-2], I deselected most all of the options (Smart Card, Real-Time Audio-Video, and Multimedia Redirection were all deselected; if I recall correctly, only Virtual Printing, Client Drive Redirection, and USB Redirection were left selected). The installation proceeded without incident, and the scan at the end reported success on all fronts.

Once the installation was complete, I was able to launch the Horizon client and proceed without further issues.

Here's hoping this information helps others who may be looking to use the Horizon Client for Linux. If you have questions, feel free to [hit me up on Twitter][link-4].



[link-1]: https://ask.fedoraproject.org/en/question/96796/vmware-horizon-client-fedora-24/
[link-2]: https://gist.github.com/pcurylo/e0893230d3f50f9143f97ba46b15add5
[link-3]: https://getfedora.org/
[link-4]: https://twitter.com/scott_lowe
