---
author: slowe
categories: Tutorial
comments: true
date: 2015-08-04T15:00:00Z
tags:
- Docker
- Vagrant
- CLI
- Virtualization
title: Using Vagrant and Docker Machine Together
url: /2015/08/04/using-vagrant-docker-machine-together/
---

In this post, I'm going to show you a quick tip I used today to combine the power of [Vagrant][link-1] with that of [Docker Machine][link-3] to quickly and easily create [Docker][link-2]-enabled virtual machines (VMs) on your laptop. This could be useful in a variety of scenarios; I leave it as an exercise for the reader to determine the best way to leverage this functionality in his or her own environment.

In my case, I needed to be able to easily create/destroy/recreate a couple of Docker-enabled VMs for a project on which I'm working. The problem I faced was that the tools I would normally use for such a task---Vagrant and Docker Machine---each had problems when used on their own:

* Vagrant has a Docker provisioner, but I could only get it to install the latest released version of Docker. In my case, I needed to run a test version (specifically, the RC2 build of Docker 1.8.0).
* Docker Machine has various back-end drivers that can create VMs into which Docker is provisioned, but the [VMware Fusion][link-4] driver for Machine _only_ works with Boot2Docker. In my case, I needed to run Ubuntu 14.04 in the VMs.

As it turns out, you can combine these two tools to gain some of the flexibility I needed in my specific circumstances. This allows you to use Vagrant (and the associated provider; VMware Fusion in my case) to handle the VM provisioning, and then use Docker Machine to provision Docker into the Vagrant-managed VMs. Here's how it works.

1. First, create a Vagrant environment that defines the VM(s) you want created and managed by Vagrant. I won't cover that here, but you can review [my list of Vagrant-tagged articles][xref-1] for more information on how you might accomplish this step.
2. Use `vagrant up` to instantiate the VM(s) you defined in your Vagrant environment.
3. Once Vagrant has created and started the VMs, verify that you have SSH connectivity using `vagrant ssh` (or `vagrant ssh <VM-name>` if you have more than one VM in the Vagrant environment).
4. Provision Docker into the new VM(s) using this command (note that I've line-wrapped the command here with backslashes):

        docker-machine create -d generic \
        --generic-ssh-user vagrant \
        --generic-ssh-key ~/.vagrant.d/insecure_private_key \
        --generic-ip-address <IP address of VM> \
        --engine-install-url "https://test.docker.com" \
        <vm-name>

    There are a couple of notable things about this command:

    * This uses Docker Machine's generic driver to import an existing VM (the VM created/configured/managed by Vagrant).
    * This assumes that you're allowing Vagrant to continue to use the public insecure SSH key (i.e., that `config.ssh.insert_key` is set to False in your `Vagrantfile`). Change this path accordingly if that isn't the case.
    * The `--engine-install-url` is what allows you to install a test/pre-release version of Docker. (Want to install an experimental build? Just change the URL provided with this parameter.)
5. To connect your local Docker client to this newly-provisioned Docker Engine, run `eval "$(docker-machine env vm-name)"` and you're all set.

From here, you can use all the standard Docker commands to pull images, launch containers, etc., using the local Docker client. If you need to access a shell within the VM, you can use _either_ `vagrant ssh <vm-name>` or `docker-machine ssh <vm-name>`. Both will work just fine.

When you're finished with the environment, or if you need to rebuild the environment for whatever reason, it's simple:

1. Run `docker-machine rm <vm-name>` to remove the VM from Docker Machine's list of available Docker Engines.
2. Run `vagrant destroy <vm-name>` to destroy the VM so that it can be re-provisioned with a clean image.

Because you're using Vagrant to provision the VMs, you have all the flexibility that Vagrant offers---choosing the OS to go into the VM, running provisioners against the new VM, etc. Because you're provisioning Docker with Docker Machine, you have the ability to specify release, test, or experimental versions of Docker should be provisioned into the VM.

I hope someone else finds this useful.

**UPDATE**: I published [this update][xref-2] to add some information about using Vagrant and Docker Machine together with Vagrant providers that leverage a forwarded port on the loopback address for connectivity.



[link-1]: http://www.vagrantup.com/
[link-2]: https://www.docker.com/
[link-3]: https://docs.docker.com/machine/
[link-4]: http://www.vmware.com/products/fusion/
[xref-1]: /tags/vagrant/
[xref-2]: {{< relref "2018-01-24-update-on-using-docker-machine-with-vagrant.md" >}}
