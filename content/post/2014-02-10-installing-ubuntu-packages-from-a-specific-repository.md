---
author: slowe
categories: Tutorial
comments: true
date: 2014-02-10T09:31:38Z
slug: installing-ubuntu-packages-from-a-specific-repository
tags:
- CLI
- Linux
- Ubuntu
title: Installing Ubuntu Packages from a Specific Repository
url: /2014/02/10/installing-ubuntu-packages-from-a-specific-repository/
wordpress_id: 3402
---

In this article, I'll show you how to install Ubuntu packages from a specific repository. It's not that this is a terribly difficult process, but it also isn't necessarily intuitive for those who haven't had to do it before. I ran into this while trying to install an alpha release of LXC 1.0.0 for my recent post on [automatically connecting LXC to Open vSwitch (OVS)][1].

Normally, you'd install a package using `apt-get` like this:

    apt-get install package-name

However, when you want to install a package from a specific repository, the command syntax shifts slightly so that it looks like this instead:

    apt-get install package-name/repository

So, in my particular case, I was trying to install LXC. However, I needed the alpha LXC 1.0.0 package from the `precise-backports` repository. So the command to do that looked like this:

    apt-get install lxc/precise-backports

Are there other useful `apt-get` tidbits like this that other readers might find particularly useful? Feel free to share in the comments below. Thanks for reading!

[1]: {{< relref "2014-01-23-automatically-connecting-lxc-to-open-vswitch.md" >}}
