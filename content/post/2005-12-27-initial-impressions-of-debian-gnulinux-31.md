---
author: slowe
categories: Review
comments: false
date: 2005-12-27T17:52:19Z
slug: initial-impressions-of-debian-gnulinux-31
tags:
- Debian
- Linux
- RedHat
title: Initial Impressions of Debian GNU/Linux 3.1
url: /2005/12/27/initial-impressions-of-debian-gnulinux-31/
wordpress_id: 147
---

As I've mentioned in a couple of previous posts, I have several servers running Red Hat Linux 9.0 that I am looking to upgrade to a newer distribution. I've tried a couple of recent versions of CentOS (a clone version of Red Hat Enterprise Linux), but ran into [problems with ntpd][1] (I may have [finally resolved][2] those). I'd heard good things about [Debian GNU/Linux][3], so I decided to give that a try as well.

Prior [experience with Ubuntu][4] (as well as [some time with Kubuntu][5]) led me to believe that Debian would be equally polished. So, I downloaded two ISO's for Debian 3.1 (aka "Sarge"), the latest stable release. Information I'd gained from some online research indicated by this release included a 2.6.8 version of the kernel, but upon installation I found I was running a patched 2.4 version instead. After a couple of attempts to upgrade the kernel via `apt-get`, I finally managed to locate an appropriate package name in the stable distribution and installed it.

Unfortunately, the 2.6.8 kernel cause the system to fail to boot, listing numerous segmentation faults and finally just halting partway into the boot process. I was extremely disappointed---after having spent that time downloading the software and then installing it, it was now completely unusable.

I'm sure that Debian fans will point out that I surely did something incorrect and things normally don't happen that way. Perhaps that is true; I will certainly be the first to admit that I may have done something incorrectly while using `apt-get` to upgrade the kernel. I would be nice to know exactly _what_ it was that went wrong, though.

In the meantime, I'm going to shelve Debian GNU/Linux as a possible replacement OS and continue searching. At this point, [OpenBSD][6] is starting to look really good...

[1]: {{< relref "2005-12-19-ntpd-on-centos-42.md" >}}
[2]: {{< relref "2005-12-23-centos-ntpd-problem-mostly-resolved.md" >}}
[3]: http://www.debian.org/
[4]: {{< relref "2005-12-08-ubuntu-510.md" >}}
[5]: {{< relref "2005-12-01-kubuntu-510.md" >}}
[6]: http://www.openbsd.org/
