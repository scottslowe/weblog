---
author: slowe
categories: Explanation
comments: true
date: 2018-01-24T14:00:00Z
tags:
- Docker
- Vagrant
- Networking
- VirtualBox
- Fusion
- Virtualization
- CLI
title: An Update on Using Docker Machine with Vagrant
url: /2018/01/24/update-on-using-docker-machine-with-vagrant/
---

As part of a project on which I'm working, I've been spending some time working with [Docker Machine][link-1] and [Vagrant][link-2] over the last few days. You may recall that I first wrote about [using these two tools together][xref-1] back in August 2015. As a result of spending some additional time with these tools---which I chose because I felt like they streamlined some work around this project---I've uncovered some additional information that I wanted to share with readers.<!--more-->

As a brief recap to [the original article][xref-1], I showed how you could use Vagrant to quickly and easily spin up a VM, then use Docker Machine's `generic` driver to add it to Docker Machine, like this:

    docker-machine create -d generic \
    --generic-ssh-user vagrant \
    --generic-ssh-key ~/.vagrant.d/insecure_private_key \
    --generic-ip-address <IP address of VM> \
    <name of VM>

This approach works fine _if the Vagrant-created VM is reachable without port forwarding_. What do I mean? In the past, the VMware provider for Vagrant used functionality in [VMware Fusion][link-4] or [VMware Workstation][link-5] to provide an RFC 1918-addressed network that had external access via network address translation (NAT). In Fusion, for example, this was the default "Share with my Mac" network. Thus, when you created a VM using Vagrant and the VMware provider for Vagrant, the first network interface card (NIC) in the VM would be assigned to this default NAT network and get an RFC 1918-style private IP address (on my Mac Pro, this network uses the 192.168.70.0/24 network range, but I don't know if that's the default for all systems). As a result, when you would run `vagrant ssh-config` for such a VM, you'd get something like this:

``` text
Host coreos-01
  HostName 192.168.70.132
  User core
  Port 22
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/slowe/.vagrant.d/insecure_private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

With version 5 of the VMware provider for Vagrant (which is required in order to use the latest versions of Fusion and Workstation), this behavior _seems_ to have changed, although it's not clear if this is intentional or not (there's [an open GitHub issue][link-3] about this change). Sometimes Vagrant will report connection information as shown above, but other times it will report using a forwarded port on the loopback address:

``` text
Host coreos-01
  HostName 127.0.0.1
  User core
  Port 2222
  UserKnownHostsFile /dev/null
  StrictHostKeyChecking no
  PasswordAuthentication no
  IdentityFile /Users/slowe/.vagrant.d/insecure_private_key
  IdentitiesOnly yes
  LogLevel FATAL
```

This occurs, by the way, even when an actual RFC 1918-style private IP address has, in fact, been assigned to the VM and is functioning normally.

This behavior is a big change from the previous behavior, but it does bring it in line with how Vagrant behaves when used with VirtualBox (which uses a forwarded port on the loopback address for connectivity). The Libvirt provider uses an approach similar to previous versions of the VMware provider (an RFC 1918-style private address instead of a forwarded port on the loopback address).

All of this brings us to the original matter at hand: using Docker Machine and Vagrant together. If you're using a Vagrant provider that does _not_ use a forwarded port on the loopback address, then the command I provided in the original article (and earlier in this article) will work just fine.

If, on the other hand, you're using a provider that _does_ use a forwarded port on the loopback address, then you'll need to amend the command slightly:

    docker-machine create -d generic \
    --generic-ssh-user vagrant \
    --generic-ssh-key ~/.vagrant.d/insecure_private_key \
    --generic-ssh-port 2222 \
    --generic-ip-address 127.0.0.1 \
    <name of VM>

Naturally, you'll need to replace `2222` with the SSH port reported by `vagrant ssh-config`, since it may change when you're running multiple VMs under Vagrant. (You may also need to change the username specified via `--generic-ssh-user`, since some distributions---I'm looking at you, [CoreOS Container Linux][link-6]---use a different username.)

Additionally, you'll also need to forward port 2376 (the port that Docker uses to communicate across the network) in your `Vagrantfile` by adding a snippet like this:

    # Configure port forwarding to support remote access to Docker Engine
    config.vm.network "forwarded_port", guest: 2376, host: 2376

With the forwarded port specified in the `Vagrantfile` and the updated `docker-machine` command that includes the correct SSH port, then you'll be able to use Vagrant to provision/manage the VM(s) and use Docker Machine to provision/manage the Docker Engine(s).

If you have questions or corrections, feel free to drop me an email (my address isn't terribly hard to find) or [hit me up on Twitter][link-7].



[link-1]: https://docs.docker.com/machine/overview/
[link-2]: https://www.vagrantup.com/
[link-3]: https://github.com/hashicorp/vagrant/issues/9151
[link-4]: https://www.vmware.com/products/fusion.html
[link-5]: https://www.vmware.com/products/workstation-pro.html
[link-6]: https://coreos.com/os/docs/latest/
[link-7]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2015-08-04-using-vagrant-docker-machine-together.md" >}}
