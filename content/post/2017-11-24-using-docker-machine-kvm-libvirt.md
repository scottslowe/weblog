---
author: slowe
categories: Tutorial
comments: true
date: 2017-11-24T23:00:00Z
tags:
- Libvirt
- KVM
- Docker
- Linux
- CLI
- Fedora
title: Using Docker Machine with KVM and Libvirt
url: /2017/11/24/using-docker-machine-kvm-libvirt/
---

Docker Machine is, in my opinion, a useful and underrated tool. I've written before about using Docker Machine with various services/providers; for example, see [this article on using Docker Machine with AWS][xref-1], or [this article on using Docker Machine with OpenStack][xref-2]. Docker Machine also supports local hypervisors, such as [VMware Fusion][link-3] or [VirtualBox][link-4]. In this post, I'll show you how to use Docker Machine with KVM and Libvirt on a Linux host (I'm using [Fedora][link-2] 27 as an example).<!--more-->

Docker Machine ships with a bunch of different providers, but the KVM/Libvirt provider must be obtained separately (you can find it [here on GitHub][link-1]). Download a binary release (make sure it is named `docker-machine-driver-kvm`), mark it as executable, and place it somewhere in your PATH. Fedora 27 comes with KVM and the Libvirt daemon installed by default (in order to support the Boxes GUI virtualization app), but I found it helpful to also install the client-side tools:

    sudo dnf install libvirt-client

This will make the `virsh` tool available, which is useful for viewing Libvirt-related resources. Once you have both the KVM/Libvirt driver and the Libvirt client tools installed, you can launch a VM:

    docker-machine create -d kvm --kvm-network "docker-machines" machine1

It appears that, by default, the KVM/Libvirt provider uses a network named "docker-machines", hence the need for the `--kvm-network` flag. (If you omit this flag, the provider will look for the "default" network, which may or may not exist on your system. It didn't exist on my Fedora laptop. This is where having `virsh` available is really handy.)

(Note that if your user account isn't a member of the "libvirt" group, you'll get prompted for authentication for most every `docker-machine` command, even just listing machines.)

There are a few other flags that might be useful as well when creating a Docker-equipped VM with Docker Machine and the KVM/Libvirt driver:

* Use the `--kvm-cpu-count` parameter to specify the number of virtual CPUs.
* Use the `--kvm-memory` parameter to control the amount of RAM allocated to the VM.
* Customize the name of the user used to log into the system (the default is "docker") by using the `--kvm-ssh-user` parameter.

You can view all the parameters by running `docker-machine create -d kvm --help`.

Once the VM is up and running, you can use all the standard Docker Machine commands to work with this VM, just like with any other provider:

* Use `docker-machine ssh <name>` to connect to the VM via SSH.
* Use `docker-machine env <name>` to show the information needed to connect to the Docker Engine on the VM. (Use `eval $(docker-machine env <name>)` to load that information into the shell environment.)
* Use `docker-machine stop <name>` to stop the VM, and `docker-machine rm <name>` to remove/delete the VM.

You might be wondering if there's any value in using Docker Machine to run a VM on Linux when you can just run Docker directly on the Linux host. I haven't fully tested the idea yet, but my initial thought is that it would enable users to run various different versions of the Docker Engine (using the `--engine-install-url` option, as I outlined [here][xref-3]). If you have other ideas where this sort of arrangement might have value, I'd love to hear them; [hit me up on Twitter][link-5].



[link-1]: https://github.com/dhiltgen/docker-machine-kvm
[link-2]: https://getfedora.org/
[link-3]: http://www.vmware.com/products/fusion.html
[link-4]: https://www.virtualbox.org/
[link-5]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2016-03-22-using-docker-machine-with-aws.md" >}}
[xref-2]: {{< relref "2015-09-24-using-docker-machine-with-openstack.md" >}}
[xref-3]: {{< relref "2015-08-04-using-vagrant-docker-machine-together.md" >}}
