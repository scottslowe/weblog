---
author: slowe
categories: Information
comments: true
date: 2020-06-23T01:10:00-07:00
tags:
- Fedora
- Libvirt
- Linux
- Vagrant
- Virtualization
title: Fixes for Some Vagrant Issues on Fedora
url: /2020/06/23/fixes-for-some-vagrant-issues-on-fedora/
---

Yesterday I needed to perform some testing of an updated version of some software that I use. (I was conducting the testing because this upgrade contained some breaking changes, and needed to understand how to mitigate the breaking changes.) So, I broke out [Vagrant][link-4] (with [the Libvirt provider][link-5]) on my [Fedora][link-6] laptop---and promptly ran into a couple issues. Fortunately, these issues were relatively easy to work around, but since the workarounds were non-intuitive I wanted to share them here for the benefit of others.<!--more-->

If you're unfamiliar with Vagrant, have a look at [my quick introduction to Vagrant][xref-1]. The "TL;DR" is the Vagrant can offer users a consistent workflow to creating and destroying VMs across a fairly wide number of platforms, including both local providers (like VirtualBox or VMware Fusion/VMware Workstation) and cloud provider (such as AWS and Azure). I've written a fair amount on Vagrant, so feel free to browse [all the "Vagrant"-tagged posts][link-7] on the site for more information.

Likewise, if you're unfamiliar with the Libvirt provider, check out this post from 2017 on [using Vagrant with Libvirt on Fedora 27][xref-2].

In my testing yesterday, I ran into two networking-related issues. The first of them was an error that the correct Libvirt network could not be found, even though `virsh net-list` and `virsh net-list --all` showed the "missing" network was present. The error is described in [this GitHub issue][link-3]; the GitHub issue also contains a fix at the very end of the discussion. If you are using Vagrant 2.2 on Fedora (which I was), then some changes had been made to the default configuration of Vagrant. [This Fedora wiki page][link-2] outlines the changes; it turns out these changes _did_ (in my case, at least) lead to the "couldn't find network" behavior. The fix, as outlined on the wiki, was to add `libvirt.qemu_use_session = false` to my `Vagrantfile`, and the problem went away. I hadn't seen this issue in my earlier use of Vagrant with Libvirt because these changes hadn't occurred until the most recent release of Vagrant (the 2.2.x series).

The second issue was also networking-related; the guest domain (VM) would boot up, but hang while waiting for an IP address. My first thought was this was related to `firewalld`, but I quickly verified via `firewall-cmd` that the Libvirt provider was placing the bridge interfaces into the correct zone to allow the necessary traffic. The ultimate fix is described in [this Red Hat Bugzilla bug][link-1]; I had to specify `host-passthrough` as the CPU mode setting for the Libvirt provider. I have _no_ idea why this works, but it does. I also find it strange that the bug dates back to Fedora 23, yet this is the first time I've run into this behavior. Regardless, it solved the problem, enabling me to move forward with the testing.

(The testing is still underway, by the way. I haven't figured out a workaround for the breaking changes introduced by the new software version.)

As always, feel free to contact me if you have any questions, comments, or suggestions for improvement. It's probably easiest to find [me on Twitter][link-8] and engage with me there. Thanks!

[link-1]: https://bugzilla.redhat.com/show_bug.cgi?id=1283989
[link-2]: https://fedoraproject.org/wiki/Changes/Vagrant_2.2_with_QEMU_Session
[link-3]: https://github.com/vagrant-libvirt/vagrant-libvirt/issues/626
[link-4]: https://www.vagrantup.com/
[link-5]: https://github.com/vagrant-libvirt/vagrant-libvirt
[link-6]: https://getfedora.org/
[link-7]: /tags/vagrant/
[link-8]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-2]: {{< relref "2017-12-06-using-vagrant-with-libvirt-on-fedora.md" >}}
