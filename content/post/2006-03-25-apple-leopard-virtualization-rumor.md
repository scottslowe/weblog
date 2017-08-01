---
author: slowe
categories: Musing
comments: false
date: 2006-03-25T15:57:06Z
slug: apple-leopard-virtualization-rumor
tags:
- Apple
- Macintosh
- OSS
- Virtualization
- VMware
title: Apple Leopard Virtualization Rumor
url: /2006/03/25/apple-leopard-virtualization-rumor/
wordpress_id: 210
---

There's a rumor floating around that [Mac OS X](http://www.apple.com/macosx/) 10.5 (code-named "Leopard") will include a virtualization engine similar to that provided by [VMware](http://www.vmware.com/). This will allow x86-based Macs running Leopard to also run Microsoft Windows and Linux on the same hardware, providing a deadly triple-play combo.

According to [this article](http://www.macosxrumors.com/articles/2006/03/23/leopard-to-include-vmware-like-virtualisation-software/) (thanks to [virtualization.info](http://virtualization.info/) for the pointer!), sources are indicating that [Apple](http://www.apple.com/) will include virtualization support in Leopard. These rumors are supported by a patent application last year which danced around the idea of running multiple operating systems but did not specifically mention virtualization.

Now, it would be tremendously cool (and not to mention very helpful) to be able to run Windows or Linux on an x86-based Macintosh. But will this virtualization be "full virtualization," allowing the use of other operating systems simultaneously (such as provided by VMware), or "paravirtualization," the ability to partition the hardware so that it supports multiple instances of the same OS (such as that provided by [Virtuozzo](http://www.swsoft.com/en/products/virtuozzo/)/[OpenVZ](http://www.openvz.org/) or [Xen](http://www.cl.cam.ac.uk/Research/SRG/netos/xen/))? I'm personally [hoping for the former][1], and the growth of open source projects such as [Q](http://www.kberg.ch/q/index.php) are a ray of light in that direction. Hopefully, Apple's support for virtualization (if such really exists) will bolster those types of efforts, not hamper them.

[1]: {{< relref "2006-02-13-apple-and-virtualization.md" >}}
