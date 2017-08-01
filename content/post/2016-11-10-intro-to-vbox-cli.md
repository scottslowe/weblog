---
author: slowe
categories: Education
comments: true
date: 2016-11-10T00:00:00Z
tags:
- VirtualBox
- Virtualization
- CLI
title: An Introduction to the VirtualBox CLI
url: /2016/11/10/intro-to-vbox-cli/
---

This post provides a basic introduction to the [VirtualBox][link-1] CLI (command-line interface) tool, `vboxmanage`. This post does not attempt to replace [the comprehensive documentation][link-2]; rather, its purpose is to help users who are new to `vboxmanage` (such as myself, having recently [adopted VirtualBox for my Vagrant environments][xref-1]) get somewhat up to speed as quickly and as painlessly as possible.

## Basic Commands

Let's start with some basic operations. Here are a few to get you started:

* To list all the registered VMs, simply run `vboxmanage list vms`. Note that if you are using Vagrant with VirtualBox, this command will also show VirtualBox VMs that have been instantiated by Vagrant. Similarly, if you are using Docker Machine with VirtualBox, this command will show you VMs created by Docker Machine.

* To list all the running VMs, use `vboxmanage list runningvms`.

* To start a VM, run `vboxmanage startvm <name or UUID>`. You can optionally specify a `--type` parameter to control how the VM is started. Using `--type gui` will show it via the host GUI; using `--type headless` means you'll need to interact over the network (typically via SSH). To emulate Vagrant/Docker Machine-like behavior, you'd use `--type headless`.

* Once a VM is running, you'll switch to `vboxmanage controlvm <subcommand>` for most other operations. Valid `<subcommands>` related to VM state operations include pause, resume, reset, poweroff, and savestate. (There's a whole ton of additional subcommands, a few of which I'll discuss later in this post.)

* To unregister (remove) a stopped VM, the command `vboxmanage unregister <name or UUID>` will do it for you. Keep in mind this does _not_ delete the VM's files. To delete the files, add the `--delete` flag to the command.

## Viewing and Modifying the Configuration of a Stopped VM

To view the information about a VM, run `vboxmanage showvminfo <name or UUID>`. Because this command is "read only" (it doesn't attempt to modify the configuration of the VM in any way), you can use this on running, paused, or stopped VMs.

To change the configuration of a stopped VM, you'll use the `modifyvm` keyword to `vboxmanage`. Here are some quick examples of modifying the configuration of a stopped VM:

* To change the name of a VM, use `vboxmanage modifyvm <name or UUID> --name <new name>`. Note that you wouldn't want to do this for Vagrant- or Docker Machine-managed VirtualBox VMs, as you would likely break the relationship between the VM and Vagrant/Docker Machine.

* To change a VM's description, you'd use `vboxmanage modifyvm <name or UUID> --description <new description>`.

* To change the amount of RAM assigned to a VM, use `vboxmanage modifyvm <name or UUID> --memory <RAM in MB>`.

* To change the number of virtual CPUs assigned to a VM, use `vboxmanage modifyvm <name or UUID> --cpus <number>`.

* To put a VM's NIC into promiscuous mode (typically necessary if you are running a software switch in the VM), use `vboxmanage modifyvm <name or UUID> --nicpromisc<num> allow-all`, where `<num>` is the number of the NIC from VirtualBox's perspective ("nicpromisc1" would target the equivalent of "eth0", "nicpromisc2" would target the equivalent of "eth1", and so forth).

Note that these commands _won't_ work on a paused VM; the VM must actually be shutdown/stopped. If the VM isn't stopped, then you'll need to switch from the `modifyvm` command to the `controlvm` command, as described in the next section.

## Modifying the Configuration of a Running VM

Not all configuration options can be modified for a running VM, but for those that can you can use the `controlvm` subcommand. I've already discussed the `controlvm` subcommands that affect VM state (pause, resume, reset, poweroff, and savestate); here are a few more that you might find useful:

* To simulate a "disconnected" eth0 link in the VM, use `vboxmanage controlvm <name or UUID> setlinkstate1 off`. (As with the "nicpromis" command, the number on the end of "setlinkstate" refers to the number of the NIC you'd like to affect). Run `vboxmanage controlvm <name or UUID> setlinkstate1 on` to restore the connection. (This is more handy than it may seem at first glance.)

* You can change a NIC's promiscuous mode setting on the fly while the VM is running; just use `vboxmanage controlvm <name or UUID> nicpromisc<num> allow-all`. To make "eth1" in the VM named "vm-name" promiscuous, for example, you'd run `vboxmanage controlvm vm-name nicpromisc2 allow-all`. Change "allow-all" to "deny" to reverse the effect.

* To change the configuration of a NIC---meaning, to change the VirtualBox network to which it connects---use `vboxmanage controlvm nic<num> <network type>`. For example, to connect "eth1" to a hostonly network, you'd use `vboxmanage controlvm <name or UUID> nic2 hostonly`.

Obviously, this is only a subset of what you can do with `vboxmanage`; use `vboxmanage <command> help` to get more information on the options available for each command. What I've included here will help you at least get started. Have fun!



[link-1]: https://www.virtualbox.org/
[link-2]: https://www.virtualbox.org/manual/UserManual.html
[xref-1]: {{< relref "2016-09-28-why-now-using-virtualbox-with-vagrant.md" >}}
