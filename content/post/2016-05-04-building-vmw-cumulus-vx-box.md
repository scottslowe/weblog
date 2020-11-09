---
author: slowe
categories: Tutorial
comments: true
date: 2016-05-04T00:00:00Z
tags:
- Fusion
- Linux
- Virtualization
- VMware
- Cumulus
- Networking
title: Building a VMware-Formatted Cumulus VX Vagrant Box
url: /2016/05/04/building-vmw-cumulus-vx-box/
---

In this post, I'm going to walk you through the process I used to build a [Vagrant][link-3] box for [Cumulus VX][link-1] that will work with VMware desktop hypervisors (like [VMware Fusion][link-4] or [VMware Workstation][link-5]). Although Cumulus Networks offers several different versions of Cumulus VX to download, they do not (strangely enough) offer a Vagrant box that will work with VMware's desktop hypervisors.

If you're not familiar with Cumulus VX, it's a virtual appliance version of [Cumulus Linux][link-2]. This allows you to test Cumulus Linux without needing compatible network hardware. This is really handy for testing configuration management tools against Cumulus Linux, for testing complex topologies before you implement them in production, or just for getting a feel for how Cumulus Linux works.

Naturally, this sounds like a perfect fit to use with Vagrant, so if you're interested---as I am/was---in running Cumulus VX with Vagrant using a VMware desktop hypervisor, then the process described below should get you all fixed up.

First, you'll want to get a hold of the VMware version of Cumulus VX. Navigate over to [the Cumulus VX download page][link-6] (a free registration is required), and download the VMware version. This will download an OVA file. _Don't_ download the Vagrant box; this is formatted for VirtualBox and won't work (at least, it didn't work for me). 

OVA files are just TAR files, so after it has finished downloaded unpack the OVA file into separate files (the filename might be slightly different for you):

    tar xvzf CumulusVX-2.5.6-4048c0a8213324c0-vmware.ova

You'll end up with a VMware virtual disk (VMDK) file and an OVF manifest.

Next, using your VMware desktop hypervisor, import the OVF file. I tested this with VMware Fusion, but it should work very similarly with Workstation. This will create a "native" VMware VM.

Once the VM is imported, boot the VM. The Cumulus VX user guide provides the default username and password; after it has booted up, go ahead and log into the VM. You'll also want to make sure the VM has connectivity to the Internet.

Now it's time to configure the Cumulus VX instance to support Vagrant. The first step will be to copy the Vagrant default insecure SSH key into the Cumulus VX VM. Cumulus VX has `wget` installed by default, so you could use this command:

    wget https://raw.githubusercontent.com/mitchellh/vagrant/master/keys/vagrant.pub

Then install the Vagrant insecure SSH key you just downloaded into the VM's `authorized_keys` file:

    cat vagrant.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys

That's it for changes to the Cumulus VX instance; you can go ahead and shut down the VM.

In the same directory where the VM's files are located, create a file named `metadata.json` with these contents:

``` json
{"provider": "vmware_desktop"}
```

In the same directory as `metadata.json` and the rest of the VM's files, create another file named `Vagrantfile` with these contents:

``` ruby
# -*- mode: ruby -*-
# vi: set ft=ruby

Vagrant.configure("2") do |config|

  # Set the SSH username
  config.ssh.username = 'cumulus'

  # Use the default Vagrant insecure key
  config.ssh.insert_key = false

  # Disable the VMware Fusion GUI
  config.vm.provider 'vmware_fusion' do |v, override|
    v.gui = false
  end # config.vm.provider
end # Vagrant.configure
```

Finally, remove any `*.plist` files from the directory with the VM's files, and also trash any log files.

Now you're ready to package it all up. Vagrant boxes, like OVA files, are just TAR files, so create the Vagrant box with this command:

    tar cvzf cumulus-vx.box ./*

After that command completes, add the new Vagrant box so that Vagrant can start using it:

    vagrant box add cumulus-vx-2.5.6 cumulus-vx.box

While you're at the CLI, add the `vagrant-cumulus` plugin you'll need as well:

    vagrant plugin install vagrant-cumulus

You can now start creating Vagrant environments that include Cumulus VX instances.

One final note: it looks like the `vagrant-cumulus` Vagrant plugin may have a bug that prevents it from correctly setting the hostname of Cumulus VX instances, so you'll get an error when you run `vagrant up`. Despite the error, everything _seems_ to work well, so I think you're safe to ignore the error for now. I don't have a workaround for that yet, but when I find one I'll update this post with more information.

[link-1]: https://cumulusnetworks.com/cumulus-vx/
[link-2]: https://cumulusnetworks.com/cumulus-linux/overview/
[link-3]: https://www.vagrantup.com/
[link-4]: http://www.vmware.com/products/fusion/
[link-5]: http://www.vmware.com/products/workstation/
[link-6]: https://cumulusnetworks.com/cumulus-vx/download/
