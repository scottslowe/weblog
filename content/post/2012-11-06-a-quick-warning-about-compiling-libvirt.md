---
author: slowe
categories: Rant
comments: true
date: 2012-11-06T09:00:00Z
slug: a-quick-warning-about-compiling-libvirt
tags:
- Interoperability
- Libvirt
- Linux
- OSS
- Ubuntu
- Virtualization
title: A Quick Warning About Compiling libvirt
url: /2012/11/06/a-quick-warning-about-compiling-libvirt/
wordpress_id: 2929
---

If you've been reading my site for any length of time, you know that I've been working with [libvirt](http://libvirt.org/), the open source virtualization API, for a while. As part of my testing with libvirt, I've needed to stay on the "cutting edge" of libvirt's development (so to speak), so I've been compiling my own version of libvirt from the source packages available from the [libvirt HTTP server](http://libvirt.org/sources/).

You can see some of the posts I've written about compiling libvirt:

[Compiling libvirt 1.0.0 on Ubuntu 12.04 and 12.10][1]  

[Compiling libvirt 0.10.1 on Ubuntu 12.04][2]  

[Compiling libvirt 0.10.1 on CentOS 6.3][3]

While these instructions work, there is an important consideration to keep in mind here: updating the system. When you compile and install your own binaries, the system has no way of knowing how to apply updates. For example, on Ubuntu, this means that `apt-get`, `aptitude`, and Update Manager are not aware that a newer version of libvirt is present on the system. Thus, when you go to update the system---you _do_ keep your system updated, right?---then things will break. (Note that there are workarounds for `apt-get` and `aptitude`, but it does not appear that these workarounds also work with Update Manager.)

If you are considering following my instructions to compile your own binaries for libvirt, please be sure to keep this consideration in mind. This sort of "gotcha" might be fine for a small home-based lab like mine, but it probably isn't the sort of issue you'd like to introduce into any sort of production (or quasi-production) environment.

Thanks to Theron Conrey for highlighting the significance of this concern. Be sure to check out Theron's comment on [this article][1] for more information on a potential Ubuntu PPA that might help with this issue.

[1]: {{< relref "2012-11-05-compiling-libvirt-1-0-0-on-ubuntu-12-04-and-12-10.md" >}}
[2]: {{< relref "2012-09-07-compiling-libvirt-0-10-1-on-ubuntu-12-04.md" >}}
[3]: {{< relref "2012-09-06-compiling-libvirt-0-10-1-on-centos-6-3.md" >}}
