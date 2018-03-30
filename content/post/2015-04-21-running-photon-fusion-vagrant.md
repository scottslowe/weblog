---
author: slowe
categories: Tutorial
comments: true
date: 2015-04-21T11:55:00Z
tags:
- Linux
- Vagrant
- CLI
- Fusion
- Virtualization
- Photon
title: Running Photon on Fusion via Vagrant
url: /2015/04/21/running-photon-fusion-vagrant/
---

Most of you have probably picked up on the news of VMware's new container-optimized Linux distribution, code-named "Photon". (More details [here][link-1].) In this post, I'm going to provide a _very_ quick walkthrough on running Photon on [VMware Fusion][link-2] via [Vagrant][link-3]. This walkthrough will leverage [a Vagrant box for Photon][link-7] that has already been created.

To make things easier, I've added a `photon` directory to [my GitHub "learning-tools" repository][link-4]. Feel free to pull those files down to make it easier to follow along.

I assume that you've already installed Vagrant, VMware Fusion, and the Vagrant plugin for VMware. If you haven't, you'll want to complete those tasks---and verify that everything is working as expected---before proceeding.

1. Install an additional Vagrant plugin that enables Vagrant to better detect and interact with Photon using this command:

		vagrant plugin install vagrant-guests-photon

	If you don't install this plugin, you'll likely get a non-fatal error about Vagrant being unable to perform the networking configuration. (Review [the GitHub repository for this plugin][link-5] if you want/need more details. Also, note that [a PR against Vagrant][link-6] to eliminate the need for this plugin was opened and merged; this fix should show up in a future release of Vagrant.)

2. Optionally, go ahead and pull down a Vagrant box for Photon using this command:

		vagrant box add vmware/photon

	If you decide not to pull down the box ahead of time, Vagrant will fetch the box the first time you run `vagrant up`. (I prefer to pull the box down ahead of time so that I have it in the event I'm offline when I run `vagrant up`, but your mileage may vary.)

3. Pull down the files from the `photon` directory in my GitHub "learning-tools" repository. You can clone the entire repository (using `git clone`), fork it and clone it, download a ZIP file of the entire repository, or just download the specific files you need.

4. Open a terminal window and change to the directory where the files from step 3 are found, then run `vagrant up`. If this is the first time you're running `vagrant up` after installing the plugin from step 1, you'll be prompted to enter your administrative credentials so that Vagrant can operate as expected.

5. Vagrant will clone the box, start up the VM, and configure it as specified in the `Vagrantfile`. Once the VM is up and running, just run `vagrant ssh` to log into the box as the "vagrant" user.

6. Note that both rkt (CoreOS container runtime) and Docker are pre-installed on Photon. You can run `rkt version` and `docker --version` to see the version numbers of each container runtime.

Because the `Vagrantfile` from my repository leverages an external YAML file for VM details, you can easily spin up more Photon instances---or change the RAM or vCPU configuration---by simply destroying the current environment (`vagrant destroy`), editing the YAML file, and re-creating the environment (`vagrant up`).

I have more Photon-related content planned, so stay tuned. In the meantime, enjoy working with Photon and learning your way around.



[link-1]: https://github.com/vmware/photon
[link-2]: http://www.vmware.com/products/fusion
[link-3]: http://www.vagrantup.com
[link-4]: https://github.com/scottslowe/learning-tools/
[link-5]: https://github.com/vmware/vagrant-guests-photon
[link-6]: https://github.com/mitchellh/vagrant/pull/5612
[link-7]: https://atlas.hashicorp.com/vmware/boxes/photon
