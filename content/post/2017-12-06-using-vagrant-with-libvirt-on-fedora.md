---
author: slowe
categories: Tutorial
comments: true
date: 2017-12-06T16:00:00Z
tags:
- Libvirt
- Linux
- Virtualization
- Vagrant
- CLI
- Fedora
title: Using Vagrant with Libvirt on Fedora 27
url: /2017/12/06/using-vagrant-with-libvirt-on-fedora/
---

In this post, I'm going to show you how to use [Vagrant][link-2] with [Libvirt][link-3] via the [vagrant-libvirt provider][link-1] when running on [Fedora][link-7] 27. Both Vagrant and Libvirt are topics I've covered more than a few times here on this site, but this is the first time I've discussed combining the two projects.<!--more-->

If you're unfamiliar with Vagrant, I recommend you start first with [my quick introduction to Vagrant][xref-2], after which you can [browse all the "Vagrant"-tagged articles on my site][link-4] for a bit more information. If you're unfamiliar with Libvirt, you can browse [all my "Libvirt"-tagged articles][link-5]; I don't have an introductory post for Libvirt.

## Background

I first experimented with the Libvirt provider for Vagrant quite some time ago, but at that time I was using the Libvirt provider to communicate with a remote Libvirt daemon (the use case was using Vagrant to create and destroy KVM guest domains via Libvirt on a remote Linux host). I found this setup to be problematic and error-prone, and discarded it after only a short while.

Recently, I revisited using the Libvirt provider for Vagrant on my Fedora laptop (which I rebuilt with Fedora 27). As I mentioned in [this post][xref-1], installing [VirtualBox][link-8] on Fedora isn't exactly straightforward. Further, what I _didn't_ mention in that post is that the VirtualBox kernel modules aren't signed; this means you must turn off Secure Boot in order to run VirtualBox on Fedora. I was loathe to turn off Secure Boot, so I thought I'd try the Vagrant+Libvirt combination again---this time using Libvirt to talk to the _local_ Libvirt daemon (which is installed by default on Fedora in order to support the GNOME Boxes application, a GUI virtual machine tool). Hence, this blog post.

## Prerequisites

Obviously, you'll need Vagrant installed; I chose to install Vagrant from the Fedora repositories using `dnf install vagrant`. At the time of this writing, that installed version 1.9.8 of Vagrant. You'll also need the Libvirt plugin, which is  available via `dnf`:

    dnf install vagrant-libvirt vagrant-libvirt-doc

At the time of writing, this installed version 0.40.0 of the Libvirt plugin, which is the latest version. You could also install the plugin via `vagrant plugin install vagrant-libvirt`, though I didn't test this approach. (In theory, it should work fine.)

As with most other providers (the AWS and OpenStack providers being the exceptions), you'll also need one or more Vagrant boxes formatted for the Libvirt provider. I found a number of Libvirt-formatted boxes on [Vagrant Cloud][link-6], easily installable via `vagrant box add`. For the purposes of this post, I'll use the "fedora/27-cloud-base" Vagrant box with the Libvirt provider.

Finally, because Vagrant is orchestrating Libvirt on the back-end, I also found it helpful to have the Libvirt client tools (like `virsh`) installed. This lets you see what Vagrant is doing behind the scenes, which can be helpful at times. Just run `dnf install libvirt-client`.

## Using Libvirt with Vagrant

Once all the necessary prerequisites are satisfied, you're ready to start managing Libvirt guest domains (VMs) with Vagrant. For a really quick start:

1. `cd` into a directory of your choice
2. Run `vagrant init fedora/27-cloud-base` to create a sample `Vagrantfile`
3. Boot the VM with `vagrant up`

For more fine-grained control over the VM and its settings, you'll want to customize the `Vagrantfile` with some additional settings. Here's a sample `Vagrantfile` that shows a few (there are many!) of the ways you could customize the VM Vagrant creates:

```ruby
Vagrant.configure("2") do |config|
  # Define the Vagrant box to use
  config.vm.box = "fedora/27-cloud-base"

  # Disable automatic box update checking
  config.vm.box_check_update = false

  # Set the VM hostname
  config.vm.hostname = "fedora27"

  # Attach to an additional private network
  config.vm.network "private_network", ip: "192.168.100.101"

  # Modify some provider settings
  config.vm.provider "libvirt" do |lv|
    lv.memory = "1024"
  end # config.vm.provider
end # Vagrant.configure
```

For a more complete reference, see [the GitHub repository for the vagrant-libvirt provider][link-1]. Note, however, that I did run into a few oddities, particularly around networking. For example, I wasn't able to create a new private Libvirt network using the `libvirt__network_address` setting; it always reverted to the default network address. However, using the syntax shown above, I _was_ able to create a new private Libvirt network with the desired network address. I was also able to manually create a new Libvirt network (using `virsh net-create`) and then attach the VM to that network using the `libvirt__network_name` setting in the `Vagrantfile`. Some experimentation may be necessary to get precisely the results you're seeking.

Once you've instantiated the VM using `vagrant up`, then the standard Vagrant workflow applies:

* Use `vagrant ssh <name>` to log into the VM via SSH.
* Use `vagrant provision` to apply any provisioning instructions, such as running a shell script, copying files into the VM, or applying an Ansible playbook.
* Use `vagrant destroy` to terminate and delete the VM.

There is one potential "gotcha" of which to be aware: when you use `vagrant box remove` to remove a Vagrant box _and_ you've created at least one VM from that box, then there is an additional step required to fully remove the box. When you run `vagrant up` with a particular box for the very first time, the Libvirt provider uploads the box into a Libvirt storage pool (the pool named "default", by default). Running `vagrant box remove` only removes the files from the `~/.vagrant.d` directory, and does **not** remove any files from the Libvirt storage pool.

To remove the files from the Libvirt storage pool, run `virsh pool-edit default` to get the filesystem path where the storage pool is found (if no changes have been made, the "default" pool should be located at `/var/lib/libvirt/images`). Navigate to that directory and remove the appropriate files in order to complete the removal of a particular box (or a specific version of a box).

So far---though my testing has been fairly limited---I'm reasonably pleased with the Libvirt provider when running against a local Libvirt daemon. The performance is good, and I haven't had to "jump through hoops" to make the virtualization provider work (as I did with VirtualBox on Fedora).

If you have any questions or feedback, [hit me up on Twitter][link-9]. Thanks!



[link-1]: https://github.com/vagrant-libvirt/vagrant-libvirt
[link-2]: https://www.vagrantup.com/
[link-3]: http://libvirt.org/
[link-4]: /tags/vagrant/
[link-5]: /tags/libvirt/
[link-6]: https://app.vagrantup.com/boxes/search
[link-7]: https://getfedora.org/
[link-8]: https://www.virtualbox.org/
[link-9]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-02-07-installing-virtualbox-51-fedora-25.md" >}}
[xref-2]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
