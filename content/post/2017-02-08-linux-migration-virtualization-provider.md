---
author: slowe
categories: Information
comments: true
date: 2017-02-08T00:00:00Z
tags:
- Linux
- CLI
- Virtualization
- Fedora
- Vagrant
- KVM
- Libvirt
title: 'The Linux Migration: Virtualization Provider'
url: /2017/02/08/linux-migration-virtualization-provider/
---

As part of my migration to Linux as my primary laptop OS, I needed to revisit my choice of virtualization provider. Long-time readers probably know that I was an early adopter of [VMware Fusion][link-2], starting way back in 2006 with the very first "friends and family" release (before it was even publicly available). Obviously I can't use Fusion on Linux, but do I use [VMware Workstation for Linux][link-3]? [VirtualBox][link-1]? Or something else? That's what I set out to determine, and in this post I'll share what I selected and the reasoning behind my selection.

So what were the options to consider? While there may be some other solutions, these are the three I primarily assessed:

* VMware Workstation for Linux 12.5.2
* VirtualBox 5.1.14
* "Native" Linux KVM, supplemented by Libvirt and a GUI like [GNOME Boxes][link-4] (installed by default in [Fedora][link-5] 25)

Since I have been using [Vagrant][link-6] quite a bit over the last few years, whatever solution I selected needed to work reasonably well with Vagrant.

I'm pretty familiar with KVM and Libvirt, so I started there. Given that KVM and Libvirt are "native" to Linux, it felt like it would be a clean solution. While GNOME Boxes is a decent (if simplistic) GUI, the Vagrant support---via [the vagrant-libvirt plugin][link-7]---was unstable. It's still really early days yet for Libvirt support in Vagrant. So, while KVM/Libvirt had the potential to offer really good performance, the usability just wasn't where I needed it to be.

Next I moved on to VMware Workstation. I had high hopes for Workstation, even though I knew that the Vagrant support wasn't (yet) where I really wanted it to be (Workstation and Fusion share the same Vagrant plugin, so the Fusion limitations I described [here][xref-1] would also apply to Workstation). Unfortunately, VMware Workstation 12.5.2 and Fedora 25 (especially with the 4.9 Linux kernel series) don't play well together. Certain prerequisite packages must be installed before the installer will even run, [custom patches must be applied][link-8] to the source code of the Workstation kernel modules in order for the installation to work, and lack of GTK3 or Wayland support meant some GUI glitches on Fedora. (You may find a couple of other threads---[here][link-9] and [here][link-10]---on the VMware Communities site to be helpful as well.) These issues, coupled with the aforementioned Vagrant integration issues, meant that Workstation wasn't the right solution, either.

&lt;aside&gt;I've spoken to the Workstation team at VMware and they've assured me they're working on some important updates for the next major release of Workstation. I can't go into details, but rest assured I will be re-evaluating Workstation with the next major release.&lt;/aside&gt;

That left only VirtualBox. As I described in [this post][xref-2], installing VirtualBox 5.1 on Fedora 25 isn't necessarily as straightforward as you might expect. Further, I found that I had to disable Secure Boot functionality on my Dell Latitude E7370 in order for the VirtualBox kernel modules to load; otherwise, they will complain about a "required key not available". (There is currently no known workaround for this issue.) Once installed and working, though, everything seems to work as expected. I already knew the Vagrant support was good (which was one reason why I'd migrated to VirtualBox on OS X for most of my needs), and given that VirtualBox is cross-platform this helps satisfy my requirement to use applications and solutions that aren't tied to a single platform only.

Based on this assessment, I've settled on using VirtualBox as my virtualization provider for my Fedora 25 laptop.

I know a lot of folks have been curious about how I'm addressing corporate collaboration and communication from Linux, and I promise I will try to address that soon. I'm still doing some additional testing, and I'd like to have that completed before I share anything. Until then, feel free to [hit me up on Twitter][link-11] if you have any questions or feedback.



[link-1]: https://www.virtualbox.org/
[link-2]: http://www.vmware.com/products/fusion.html
[link-3]: http://www.vmware.com/products/workstation-for-linux.html
[link-4]: https://en.wikipedia.org/wiki/GNOME_Boxes
[link-5]: https://getfedora.org/
[link-6]: https://www.vagrantup.com/
[link-7]: https://github.com/vagrant-libvirt/vagrant-libvirt
[link-8]: https://communities.vmware.com/message/2644848#2644848
[link-9]: https://communities.vmware.com/thread/552175?start=0&tstart=0
[link-10]: https://communities.vmware.com/thread/552699?start=0&tstart=0
[link-11]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
[xref-2]: {{< relref "2017-02-07-installing-virtualbox-51-fedora-25.md" >}}
