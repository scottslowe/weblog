---
author: slowe
categories: Rant
comments: true
date: 2016-09-28T00:00:00Z
tags:
- Fusion
- macOS
- Virtualization
- CLI
- VirtualBox
title: Why I'm Now Using VirtualBox with Vagrant
url: /2016/09/28/why-now-using-virtualbox-with-vagrant/
---

One of the things I often tell people is, "Use the right tool for the job." As technologists, we shouldn't get so locked onto any one technology or product that we can't see when other technologies or products might solve a particular problem more effectively. It's for this reason that I recently made [VirtualBox][link-2]---not [VMware Fusion][link-1]---my primary virtualization provider for [Vagrant][link-3] environments.

I know it seems odd for a VMware employee to use/prefer a non-VMware product over a competing VMware product. I've been a long-time Fusion user (since 2006 when I was part of the original "friends and family" early release). Since I started working with Vagrant about two years ago, I really tried to stick it out with VMware Fusion as my primary virtualization provider. I had a ton of experience with Fusion, and---honestly---it seemed like the right thing to do. After a couple of years, though, I've decided to switch to using VirtualBox as my primary provider for Vagrant.

Why? There's a few different reasons:

1. **Greater manageability:** VirtualBox comes with a really powerful CLI tool, `vboxmanage`, that lets me do just about _anything_ from the command line. In fact, the VirtualBox documentation refers to the CLI as an alternate UI, not just a supplemental tool. It's a core part of the product. Need to start a VM? No problem. Stop a VM? Got it covered. Modify the settings of a virtual NIC for a VM from the CLI? Yep, it can do that too. VMware Fusion has the `vmrun` tool, but it doesn't come anywhere close to matching the breadth and scope of `vboxmanage` (nor is the `vmrun` tool documented very well).

2. **Less friction:** It seemed like every time I turned around, I was running into yet another problem or limitation of some sort trying to make VMware Fusion work with Vagrant. For example, if you want to be able to attach a virtual NIC to a particular network without assigning an IP address, Vagrant+Fusion won't work. It can't be done (at least, not that I've found). With VirtualBox, it's easily accomplished.

3. **Bigger community:** The real power of tools like Vagrant is the community behind the tool. In this case, one metric for measuring such community is the availability of Vagrant boxes for your virtualization platform. With Fusion (or Workstation), finding good Vagrant boxes can be a challenge. Most of the "official" boxes---like those provided by Ubuntu or Debian---are available only for VirtualBox. It's _far_ easier to find VirtualBox-formatted Vagrant boxes than VMware-formatted boxes. And yes, while you can use a tool like [Packer][link-4] to build your own boxes, they tend to be larger than the "official" images. (I saved several gigabytes of disk space switching to VirtualBox-formatted "official" boxes.) Besides, do you really want to spend all your time managing Packer builds? Or do you want to get on with getting things done? (This kind of goes back to point #2 as well.)

This doesn't mean I've ditched VMware Fusion entirely, nor does it mean that VirtualBox is a better product than Fusion. This is where "using the right tool for the job" applies. If I need to run Windows, I'll do that with Fusion because the UI and the Windows support are outstanding. If I need to run a virtual instance of OS X (sorry, macOS), I'll do that with Fusion because Fusion's support for virtualized macOS instances is really solid. If I need to run a VM with nested virtualization support, I'll do that with Fusion because VirtualBox doesn't offer that functionality. However, for simple Linux instances where I don't need a GUI and I want to automate it with Vagrant, I'm going to use VirtualBox because it makes more sense. For me, it's simply the right tool for the job.

[link-1]: http://www.vmware.com/products/fusion.html
[link-2]: https://www.virtualbox.org/
[link-3]: https://www.vagrantup.com/
[link-4]: https://www.packer.io/
