---
author: slowe
categories: Tutorial
comments: true
date: 2017-01-18T00:00:00Z
tags:
- Linux
- Hardware
- Apple
- Networking
- Fedora
- CLI
title: Enabling an Apple MBP Wireless Adapter with Fedora 25
url: /2017/01/18/apple-mbp-wifi-fedora/
---

In this article, I want to share with you the steps I took to enable wireless networking on an older (mid-2011) 13" MacBook Pro running [Fedora][link-2] 25. This is driven by a continued need to evaluate Fedora 25, as I've run into a few potential roadblocks with [Ubuntu][link-1] 16.04 as my primary laptop OS. Using Fedora 25 instead _may_ help resolve some of these issues, which primarily center around corporate collaboration.

First, you'll want to enable the [RPM Fusion][link-4] repositories. This is pretty well documented on the RPM Fusion web site. [This link][link-3] will take you to the configuration page, which will provide links for graphical setup via your browser as well as CLI commands.

Once the RPM Fusion repositories (both Free and Nonfree) repositories are enabled, then it's just a matter of installing a few packages:

1. First, install the "kernel-devel" package appropriate for your current kernel. The command to use is:

        sudo dnf install "kernel-devel-uname-r == $(uname -r)"

    This could be user error on my part, but I've found that it's necessary to use the full package (including version) instead of just "kernel-devel". Otherwise, Fedora seems to have a tendency to install the latest package, which may not match the current kernel you're actually running.

2. Next, install the "akmods" and "broadcom-wl" packages:

        sudo dnf install akmods broadcom-wl

3. After these packages have been installed, build the kernel modules with `sudo akmods`. This will take a moment, then it should return you to the terminal prompt after an "OK" message.

Once you reboot, you should have a new network adapter (on my particular MBP, it was identified as "wlp3s0"), and you can join the wireless network of your choice.

[link-1]: https://www.ubuntu.com
[link-2]: https://getfedora.org
[link-3]: https://rpmfusion.org/Configuration
[link-4]: https://rpmfusion.org/