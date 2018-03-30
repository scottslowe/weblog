---
author: slowe
categories: Information
comments: true
date: 2015-02-17T13:40:00Z
tags:
- OVS
- Vagrant
- Linux
- CLI
title: Vagrant Box for Learning Open vSwitch
url: /2015/02/17/vagrant-box-ovs/
---

As you may have picked up from some of my recent posts, I'm focused on building content _and_ tools that will help others learn new projects, products, and technologies that I think will be very relevant in the future. One such project is [Open vSwitch (OVS)][link-1], which I've written about quite a bit (you can see all OVS-related posts [here][link-3]). To help others work with and learn about Open vSwitch, I've published a new [Vagrant][link-2] base box.

(In the event you're not familiar with Vagrant, take a look at [this quick introduction][xref-1].)

The new Vagrant box I've created is running Ubuntu 14.04.1 and has Open vSwitch 2.3.1---the latest available release---pre-installed. To install this Vagrant box for use in your Vagrant environments, simply run this command:

	vagrant box add slowe/ubuntu-1404-x64-ovs

Vagrant will download and install the box. (Note that this box is formatted for the "vmware_desktop" provider, which means you'll need VMware Fusion or VMware Workstation as well as the Vagrant plugin for VMware.) Once the box is installed on your system, then you can begin using it in a `Vagrantfile` by just referencing the box name. As with the other Vagrant boxes I've published, VMware Tools is installed already, so you can use Vagrant's shared folders feature to easily move files into or out of VMs spun up by Vagrant.

I'll be leveraging this box in the near future as I build (and share) more Vagrant environments. All these Vagrant environments will be shared via [my GitHub "learning-tools" repository][link-4].

Here's hoping this is useful!


[link-1]: http://openvswitch.org/
[link-2]: http://www.vagrantup.com/
[link-3]: /tags/ovs/
[link-4]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
