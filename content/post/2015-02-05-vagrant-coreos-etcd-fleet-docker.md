---
author: slowe
categories: Tutorial
comments: true
date: 2015-02-05T11:50:00Z
tags:
- Linux
- Docker
- Virtualization
- Fusion
- CLI
- Vagrant
- CoreOS
- etcd
title: Using Vagrant with CoreOS, etcd, fleet, and Docker
url: /2015/02/05/vagrant-coreos-etcd-fleet-docker/
---

As a follow-up to [my recent #vBrownBag session][link-1] on "Docker and Friends," I wanted to provide a quick and relatively easy way for VMware administrators to experiment with some of the technologies I demonstrated. Since not everyone has their own OpenStack cloud running in their basement, [Vagrant][link-3] seemed like a reasonable solution. So, in this post, I'll show you how to use Vagrant to experiment with some of the technologies I demonstrated in the #vBrownBag session.

If you'd like to follow along on your own, I've uploaded the necessary files to [the GitHub repo I created][link-2] for the #vBrownBag session. Just have a look in the `coreos-vagrant` folder in that repository.

## What You'll Need

* Vagrant (I tested with Vagrant 1.7.2)
* VMware Fusion (I used Fusion 6.0.5 on OS X 10.9.5)
* the VMware plugin for Vagrant
* the CoreOS Vagrant box for the `vmware_fusion` provider (more on that in a moment)
* the necessary supporting files (more on that in a moment as well)

I'm not going to cover installing Fusion, Vagrant, or the VMware provider for Vagrant, as those steps are reasonably well-documented by the respective vendors. I will, though, talk about getting the CoreOS box and the supporting files.

### Getting the CoreOS Box

At the time of this writing, CoreOS supplied a Vagrant box for the `vmware_fusion` provider that you could install with a simple `vagrant box add`, like this:

```sh
vagrant box add http://stable.release.core-os.net/amd64-usr/current/coreos_production_vagrant_vmware_fusion.json
```

This command will download and install the Vagrant box you'll need for use with VMware Fusion. Once it's done, you'll see the box listed when you run `vagrant box list`. I chose the stable branch for my testing; you could just as easily run builds from the alpha or beta branches by replacing "stable" in the URL above with the branch you want to use.

### Getting the Supporting Files

There are two ways to get the supporting files you need:

1. The [GitHub repo][link-2] I created for the "Docker and Friends" #vBrownBag session
2. The [GitHub repo][link-4] created and maintained by CoreOS

You can choose to either clone these repos to your local system (using `git clone` and the GitHub HTTPS clone URL), or simply download a ZIP archive of the repository and unpack it locally. It's up to you which approach you want to use.

Either repository will do what you need; I chose to create my own supporting files because I wanted to leverage some lessons learned in [using YAML to create multi-machine Vagrant environments][xref-6]. For the rest of this post, I'll assume you're using the support files from my GitHub repo.

## Preparation

Before you can just run `vagrant up` to turn up a cluster of systems on your laptop, there are a couple things you need to do first.

### Provide VM Configuration Details

First, you'll need to edit the `servers.yml` to provide Vagrant with the details on the VMs you'd like it to create. Here's a snippet from the supplied `servers.yml` in the repository:

```yaml
---
- name: coreos-01
  box: coreos-stable
  ram: 512
  vcpu: 1
  priv_ip: 192.168.254.101
  pub_ip: 172.16.5.11
```

Let's walk through this real quick:

* On the `name` line, supply the friendly name you'd like given to the VM spawned by Vagrant. This name will also be used as the hostname for the guest OS inside the VM.
* On the `box` line, supply the name of the Vagrant box you downloaded earlier. You can verify the name by running `vagrant box list`; whatever's listed there for the CoreOS box you downloaded, specify it here.
* On the `ram` and `vcpu` lines, tell Vagrant how much RAM and how many vCPUs should be assigned to the VMs it will create.
* On the `priv_ip` and `pub_ip` lines, supply IP addresses (both private and public) to be assigned to the CoreOS VMs.

This last item deserves a bit more discussion/explanation. CoreOS uses a variant of cloud-init (called coreos-cloudinit) that will read a YAML-formatted file (more on that in a moment in the "Provide Customization Information" section) to initialize (or customize) the CoreOS configuration. When used with Vagrant, coreos-cloudinit will read the IP address assigned _in the Vagrantfile_ (not what is actually assigned to the interfaces) and put them in a file named `/etc/environment`. The values in this file are then used in a variety of other places, including in the etcd configuration. This has a couple of ramifications. First, you can't leave the IP address out of the Vagrantfile, or else `/etc/environment` will get populated with the loopback address and nothing will work. Second, you can't use DHCP on the networks (or at least the public network), or else there's no guarantee that the address assigned to the interface will match the address specified in the Vagrantfile. (Unless you'd like to jump through a few hoops with static MAC addresses and DHCP configurations.) Remember, it's the address supplied in the Vagrantfile that's used by coreos-cloudinit, _not_ the address actually assigned to the interface.

Repeat this block of information (making sure to include the "-" in front of "name" as per YAML syntax) for each VM you want Vagrant to spin up. The supplied `servers.yml` defines 3 VMs.

### Provide Customization Information

In addition to providing the VM configuration details, you'll also need to edit the `user-data` file that is used by coreos-cloudinit to customize CoreOS on startup. My GitHub repo has a sample `user-data` (just as it has a sample `servers.yml`), but you'll need to supply an etcd discovery URL. This is documented inside the `user-data` file.

To obtain a discovery URL, just visit [https://discovery.etcd.io/new][link-5]. That URL will return a longer URL, which is your unique discovery URL. That's the value you'll supply in the `user-data` file where indicated by the comments.

Note that each cluster needs its own unique discovery URL, so everytime you `vagrant destroy` you'll need to generate a new discovery URL.

Also, note here that the `user-data` file references a variable `$public_ipv4`. This variable references the static IP address assigned to the public network in the Vagrantfile. This is why the static IP address is so important---it's used here in the configuration of both etcd and fleet, and if it is not correct or missing then everything breaks.

Once you've supplied the etcd discovery URL, you're ready to start things up.

## Starting up the VMs

In the same directory where the Vagrantfile, `user-data`, and `servers.yml` files reside, simply run `vagrant up`. Vagrant will instantiate and configure the VMs given the information in `servers.yml`, customize them via coreos-cloudinit using the information in `user-data`, and then communicate out to the etcd discovery URL to form an etcd cluster. Once Vagrant has completed, then you can use `vagrant ssh <name>` to connect to one of the VMs, then use commands like `systemctl status etcd` (to check on the status of etcd) or `fleetctl list-machines` (to see the membership of the cluster). Once fleet is working as expected, then you can also use `fleetctl` to launch Docker containers across the etcd cluster. (This is all stuff I demonstrated during the #vBrownBag session.)

Having an automated way to quickly and easily turn up a cluster of CoreOS systems running etcd, fleet, and Docker should make it much easier for newcomers to these technologies to begin to learn them and see how they could be put to work in their environments.

## Additional Information

If you'd like some additional information on some of the technologies mentioned in this post, have a look at these articles:

[A Quick Introduction to Vagrant][xref-1]  
[CoreOS Continued: etcd][xref-2]  
[CoreOS Continued: Fleet and Docker][xref-3]  
[Deploying CoreOS on OpenStack Using Heat][xref-4]  
[A Heat Template for Docker Containers][xref-5]

[link-1]: http://professionalvmware.com/2015/02/vbrownbag-devops-follow-up-docker-and-friends-with-scott-lowe-scott_lowe/
[link-2]: https://github.com/scottslowe/2015-vbrownbag-docker
[link-3]: http://www.vagrantup.com/
[link-4]: https://github.com/coreos/coreos-vagrant
[link-5]: https://discovery.etcd.io/new
[xref-1]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-2]: {{< relref "2014-08-18-coreos-continued-etcd.md" >}}
[xref-3]: {{< relref "2014-08-20-coreos-continued-fleet-and-docker.md" >}}
[xref-4]: {{< relref "2014-08-13-deploying-coreos-on-openstack-using-heat.md" >}}
[xref-5]: {{< relref "2014-08-22-a-heat-template-for-docker-containers.md" >}}
[xref-6]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
