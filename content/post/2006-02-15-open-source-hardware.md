---
author: slowe
categories: News
comments: false
date: 2006-02-15T21:47:08Z
slug: open-source-hardware
tags:
- BSD
- Hardware
- Linux
- OSS
- Sun
- Virtualization
- Interoperability
title: Open Source Hardware
url: /2006/02/15/open-source-hardware/
wordpress_id: 180
---

[Sun](http://www.sun.com/) joins [IBM](http://www.ibm.com/) in trying to use the open source software model to help with hardware as well. (Thanks to [Linux-Watch](http://www.linux-watch.com/) for the news.) Creating a new project called [OpenSPARC](http://opensparc.sunsource.net/nonav/index.html), Sun is open sourcing the specifications for its latest SPARC processor, the UltraSPARC T1, code-named "Niagara".

According to the [Linux-Watch article](http://www.linux-watch.com/news/NS6472325496.html), the effort is intended to help drive the development of ports of Linux and BSD that can take full advantage of the CoolThreads technology in the UltraSPARC T1, which provides 32 threads of execution. This allows the T1 to provide much greater throughput at lower clock speeds with dramatically lower power consumption.

In addition to the processor architecture and code, Sun is also open sourcing its HyperVisor API information. Like other vendors' hypervisor efforts, the idea is to allow multiple operating systems or multiple instances of an operating system to run simultaneously on the same hardware. Again, ports of Linux and BSD that are designed to take full advantage of the UltraSPARC T1 architecture and HyperVisor API are beneficial to Sun because they can help drive sales of their hardware.

It's a good idea, really, if you think about it. Sun's big into open source these days, after creating the [OpenSolaris](http://opensolaris.org/os/) project in an effort to open source the entire [Solaris](http://www.sun.com/software/solaris/) operating system. However, Solaris is really the only operating system that can run well on Sun's SPARC hardware, and helping other alternatives to run equally well on SPARC hardware would encourage more people to buy SPARC hardware. With any luck, Sun could create the kind of momentum and mystique around their SPARC hardware as they've done with their [AMD](http://www.amd.com/)-based "Galaxy" servers.

It'll be interesting to see how it plays out.
