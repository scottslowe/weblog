---
author: slowe
categories: Tutorial
comments: true
date: 2015-02-10T14:25:00Z
aliases: /2015/02/11/using-docker-with-vagrant/
tags:
- Docker
- Vagrant
- Linux
- CLI
- Fusion
title: Using Docker with Vagrant
url: /2015/02/10/using-docker-with-vagrant/
---

As part of my ongoing effort to create tools to assist others in learning some of the new technologies out there, I spent a bit of time today working through the use of [Docker][link-1] with [Vagrant][link-2]. Neither of these technologies should be new to my readers; I've already provided quick introductory posts to both (see [here][xref-1] and [here][xref-2]). However, using these two together may provide a real benefit for users who are new to either technology, so I'd like to take a bit and show you how to use Docker with Vagrant.

## Background

Vagrant first started shipping with a Docker provider as part of the core product in version 1.6 (recall that Vagrant uses the concept of _providers_ to support multiple backend virtualization solutions). Therefore, if you've installed any recent version of Vagrant, you already have the Docker provider as part of your Vagrant installation.

However, while you may have the Docker provider as part of Vagrant, you still need Docker itself (just like if you have the VMware provider for Vagrant, you still need the appropriate VMware product---[VMware Fusion][link-5] on the Mac or [VMware Workstation][link-6] on Windows/Linux) in order to provide the functionality Vagrant will consume. This presents a bit of a unique challenge because not all platforms support Docker. Unless you're running Vagrant directly on Linux, you're going to need an installation of Linux to support running the Docker daemon. The Docker community solved this problem using [boot2docker][link-3], which is essentially a super-lightweight Linux instance running the Docker daemon, and provided some simple installers to install [VirtualBox][link-4] and this boot2docker VM onto systems running OS X and Windows so that users could more easily use/experiment with Docker.

"OK," you might reply. "But what does that have to do with using Docker with Vagrant?"

Vagrant's creator, Mitchell Hashimoto, took the boot2docker VM and created a Vagrant box (recall that Vagrant uses the concept of _boxes_, which are essentially VM templates created for each back-end provider) with support for the VirtualBox provider as well as the VMware provider. Vagrant's Docker provider, by default, uses this boot2docker box as the target for its Docker functionality when you are running Vagrant on anything other than Linux.

I took the time to explain all of this because as you move into the next section where I show you how to set up some `Vagrantfiles` to work with Docker under Vagrant, you'll understand _why_ things work the way they do.

## Setting Up Docker With Vagrant

Based on this background information, you should understand now that using Docker with Vagrant involves 4 different components in addition to Vagrant itself:

1. The Docker provider for Vagrant
2. Some sort of Linux instance on which to run the Docker daemon
3. A virtualization platform, such as VirtualBox, VMware Fusion, or VMware Workstation
4. The appropriate Vagrant provider for your virtualization platform

The Docker provider is installed when you install Vagrant (any release since 1.6), and I'm going to assume here that you've already installed the appropriate virtualization platform and provider (I'm using VMware Fusion 6.0.5 on OS X 10.9.5 with the Vagrant VMware plugin). All that's left to discuss then, is the Linux instance for the Docker daemon and putting together the appropriate `Vagrantfiles` to make this all work as expected.

### Specifying the Linux Instance for Docker

I mentioned earlier that Mitchell Hashimoto, the creator of Vagrant, took the boot2docker VM and created a Vagrant box from it. You can add that Vagrant box to your system by simply running `vagrant box add mitchellh/boot2docker`. Vagrant will prompt for the provider, and then download the appropriate version of the box for the selected provider. Then, unless you tell it otherwise, Vagrant will spin up an instance of this boot2docker box anytime it needs to perform Docker provider operations. If you haven't already added the Vagrant box (using the command above) the first time you use the Docker provider, it will download the box automatically.

But what if you _don't want_ to use boot2docker? What if you'd rather use Ubuntu or CentOS, so that what you do in Vagrant more closely matches what you might do in the data center?

No problem---you can change this behavior by adding a line in the `Vagrantfile` that spins up your Docker containers. The specific line would look something like this (I'll provide more concrete examples later in the post):

	docker.vagrant_vagrantfile = "path/to/host/VM/Vagrantfile"

That's right: all you have to do is create a `Vagrantfile` that spins up a VM running the Linux distribution of your choice, and then reference that `Vagrantfile` in the one that creates your Docker containers. Of course, since Vagrant expects the filename to be `Vagrantfile`, that means the file defining the host VM _must_ be in a different directory than the file defining the Docker containers. Hence, you need to specify the path to the host VM `Vagrantfile`. (There might be a way to combine the files, but I haven't figured out how.)

Although Vagrant expects Docker to be running inside this host VM, it does provide a way to simplify that by allowing you to provision (install) Docker into the VM when you run `vagrant up` (much in the same way Vagrant can provision other things into a VM when it is first instantiated). This is done with a simple command in the host VM `Vagrantfile`:

	config.vm.provision "docker"

Pretty straightforward, right? Let's take a look at a full example of a `Vagrantfile` that could be used to define a host VM for the Vagrant Docker provider:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

# Create and configure the VM(s)
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Assign a friendly name to this host VM
  config.vm.hostname = "docker-host"

  # Skip checking for an updated Vagrant box
  config.vm.box_check_update = false

  # Always use Vagrant's default insecure key
  config.ssh.insert_key = false

  # Spin up a "host box" for use with the Docker provider
  # and then provision it with Docker
  config.vm.box = "slowe/ubuntu-trusty-x64"
  config.vm.provision "docker"

  # Disable synced folders (prevents an NFS error on "vagrant up")
  config.vm.synced_folder ".", "/vagrant", disabled: true
end
```

Most of this `Vagrantfile` is pretty simple---define a VM, specify the box, disable checking for new versions of the box, etc. A couple of things I want to note:

* First, note the `config.vm.synced_folders` command---during my testing I found that if you did _not_ disable synced folders, Vagrant would halt with an NFS error. The only workaround that I found was to disable synced folders both at the host VM _and_ at the Docker container level.
* Second, note the aforementioned `config.vm.provision` command. This expects that the VM created by Vagrant will have Internet access; if it doesn't, it will fail. Please plan accordingly.

You'll note that this box uses my Ubuntu 14.04 base box for the vmware_desktop provider (works with both Fusion and Workstation), but you could use just about any appropriate Linux box here. (I'm just trying to make it as easy as possible for folks to try this out themselves, so feel free to use my Ubuntu 14.04 base box.)

I like to store this `Vagrantfile` in a subdirectory of the main project directory. So, if I was storing my Vagrant project in, say, `/Users/slowe/Projects/vagrant-docker`, then I might put the `Vagrantfile` that defined the host VM in `/Users/slowe/Projects/vagrant-docker/host` or similar. Obviously, this is completely up to you; you just need to know the path and specify it in the file that defines your containers.

### Specifying your Containers

And speaking of defining your containers...that's the next topic to discuss. Here's a sample `Vagrantfile` that specifies the host VM and builds a single Nginx container:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

# Create and configure the Docker container(s)
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Disable synced folders for the Docker container
  # (prevents an NFS error on "vagrant up")
  config.vm.synced_folder ".", "/vagrant", disabled: true

  # Configure the Docker provider for Vagrant
  config.vm.provider "docker" do |docker|

    # Define the location of the Vagrantfile for the host VM
    # Comment out this line to use default host VM that is
    # based on boot2docker
    docker.vagrant_vagrantfile = "host/Vagrantfile"

    # Specify the Docker image to use
    docker.image = "nginx"

    # Specify port mappings
    # If omitted, no ports are mapped!
    docker.ports = ['80:80', '443:443']

    # Specify a friendly name for the Docker container
    docker.name = 'nginx-container'
  end
end
```

As with the `Vagrantfile` that specifies the host VM, this is reasonably straightforward once you've worked with Vagrant a little while. This example `Vagrantfile` disables synced folders, specifies the path to the `Vagrantfile` defining the host VM (in this case, that file resides in a subdirectory named `host`), provides the name of the Docker image to use, the ports to map, and a user-friendly name. Note that if you omit the `docker.ports` statement no ports will be mapped.

The techniques I've already shared with you regarding the use of YAML and an external data file could certainly be applied here to create a multi-container `Vagrantfile` (see [this post][xref-3] for more details).

## Bringing Up the Environment

As with most uses of Vagrant, all you need to do is run `vagrant up` from the main directory where the `Vagrantfile` that defines your containers is located. The main `Vagrantfile` includes a reference to the `Vagrantfile` defining your host VM, so Vagrant will automatically turn up the VM (if needed), install Docker (if this host VM is being provisioned for the first time), and create the Docker containers. Once it's done, you're ready to roll!

When you go to turn down the environment, it's a bit trickier. The `vagrant halt` and `vagrant destroy` commands will only affect the Docker containers; you'll have to use the `vagrant global-status` command to get a reference for the host VM so that you can halt or destroy it separately.

## Additional Notes and Resources

I performed my testing using Vagrant 1.7.2, running with the Vagrant VMware plugin against VMware Fusion 6.0.5 on OS X 10.9.5. If you use different versions or different platforms, the specifics might be slightly different (but should not be dramatically different).

I found [this post][link-7] by Nick Weaver to be very helpful as well, providing some additional details and examples that fleshed out the Vagrant documentation.

Finally, I posted resources to help you follow along with this blog post in [my "learning-tools" GitHub repository][link-8]; feel free to clone or download the repository to help with your own studies.

[link-1]: http://www.docker.com
[link-2]: http://www.vagrantup.com
[link-3]: https://github.com/boot2docker/boot2docker
[link-4]: https://www.virtualbox.org/
[link-5]: http://www.vmware.com/products/fusion/
[link-6]: http://www.vmware.com/products/workstation/
[link-7]: http://nickapedia.com/2014/06/12/docker-vagrant-vmware-fusion-bug/
[link-8]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2014-03-11-a-quick-introduction-to-docker.md" >}}
[xref-2]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-3]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
