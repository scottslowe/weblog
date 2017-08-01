---
author: slowe
categories: Explanation
comments: false
date: 2006-03-22T21:40:41Z
slug: installing-vmware-workstation-on-ubuntu
tags:
- Linux
- Ubuntu
- Virtualization
- VMware
title: Installing VMware Workstation on Ubuntu
url: /2006/03/22/installing-vmware-workstation-on-ubuntu/
wordpress_id: 208
---

Once I'd gotten [Ubuntu up and running on my HP nc8230 laptop][1], the next order of business was---due to business needs---get a copy of Windows XP Professional running under [VMware Workstation](http://www.vmware.com/products/ws/) on Ubuntu. While I'm not a huge Windows fan (I prefer [Mac OS X](http://www.apple.com/macosx/) and Linux to Windows, generally), I also recognize the need for Windows in a world where your customers all run Windows.

I'd never installed VMware Workstation on a Linux host before, so this would be a new experience for me. It couldn't be that hard, right? Well, it wasn't as easy as I had hoped it would be, that's for sure.

In order to get VMware Workstation 5.5.1 to install on Ubuntu 5.10, here's what I had to do:

1. Copy the VMware Workstation 5.5.1 software onto the machine.

2. Using `apt-get`, install gcc, g++, and the appropriate Linux headers.

3. Untar the VMware Workstation software and run the installation script. When prompted, go ahead and compile a custom vmmon module.

That should do it. One site I found while preparing for this also suggested installing the "build-essential" package, but I didn't install this and VMware Workstation seems to run just fine.

Coming up soon: installation of Solaris x86 under VMware Workstation running on Ubuntu Linux!

[1]: {{< relref "2006-03-22-installing-ubuntu-on-an-hp-compaq-nc8230.md" >}}
