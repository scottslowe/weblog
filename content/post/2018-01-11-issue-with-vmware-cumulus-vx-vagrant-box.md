---
author: slowe
categories: Information
comments: true
date: 2018-01-11T22:00:00Z
tags:
- Cumulus
- Vagrant
- VMware
- Fusion
title: Issue with VMware-Formatted Cumulus VX Vagrant Box
url: /2018/01/11/issue-with-vmware-cumulus-vx-vagrant-box/
---

I recently had a need to revisit the use of Cumulus VX (the [Cumulus Networks][link-1] virtual appliance running Cumulus Linux) in a [Vagrant][link-2] environment, and I wanted to be sure to test what I was doing on multiple virtualization platforms. Via [Vagrant Cloud][link-3], Cumulus distributes [VirtualBox][link-4] and [Libvirt][link-5] versions of Cumulus VX, and there is a slightly older version that also provides a [VMware][link-6]-formatted box. Unfortunately, there's a simple error in the VMware-formatted box that prevents it from working. Here's the fix.<!--more-->

The latest version (as of this writing) of Cumulus VX was 3.5.0, and for this version both VirtualBox-formatted and Libvirt-formatted boxes are provided. For a VMware-formatted box, the latest version is 3.2.0, which you can install with this command:

    vagrant box add CumulusCommunity/cumulus-vx --box-version 3.2.0

When this Vagrant box is installed using the above command, what actually happens is _something_ like this (at a high level):

1. The `*.box` file for the specific box, platform, and version is downloaded. This `.box` file is nothing more than a TAR archive with specific files included (see [here][link-7] for more details).

2. The `*.box` file is expanded into the `~/.vagrant.d/boxes` directory on your system. A directory tree is built that helps Vagrant support multiple versions of the same box along with multiple formats of the same box (for example, having version 3.2.0 of a VMware-formatted box alongside version 3.5.0 of a VirtualBox-formatted box on the same system).

In this case, when you install version 3.2.0 of the VMware-formatted Cumulus VX Vagrant box, you'll end up with a set of files found in `~/.vagrant.d/boxes/CumulusCommunity-VAGRANTSLASH-cumulus-vx/3.2.0/vmware_desktop`. In this directory, you'll find all the files that would describe a VM to a product like VMware Fusion or VMware Workstation: the VMX file, one or more VMDK files, etc.

What you'll also find for this particular box is something you don't want: a lock file, in the form of a directory named `cumulus-linux-3.2.0.vmx.lck`. This lock file is normally created by a VMware desktop virtualization product to indicate that the VM is running and therefore the files are locked and can't be accessed. Unfortunately, the presence of this directory means that the Vagrant box _will not work._

If you try to run `vagrant up` on a Vagrant enviroment with this box, you'll get an error indicating the files are locked, and the `vagrant up` command will fail.

So how does one fix this?

Simple: just delete the `cumulus-linux-3.2.0.vmx.lck` directory and its contents.

Once you've deleted that file, then using `vagrant up` to instantiate a Vagrant environment based on this box will work as expected.

(Side note: If you are planning to use version 3.2.0 of the VMware-formatted Cumulus VX box, there's one additional oddity. When you use `vagrant box add` as outlined above to download and install the box, you'll be prompted with a set of options for which provider to use. Be sure to use option 4---the one labeled "vmware_desktop"---and not option 5, labeled "vmware_fusion". The latter reports an error after downloading the box and the command fails.)

Hopefully Cumulus Networks will release an updated version of the Cumulus VX Vagrant box for VMware products that addresses these issues.



[link-1]: https://cumulusnetworks.com
[link-2]: https://www.vagrantup.com/
[link-3]: https://app.vagrantup.com/boxes/search
[link-4]: https://www.virtualbox.org/
[link-5]: https://libvirt.org/
[link-6]: https://www.vmware.com/products/personal-desktop-virtualization.html
[link-7]: https://www.vagrantup.com/docs/boxes/format.html
