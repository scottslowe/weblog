---
author: slowe
categories: Education
comments: true
date: 2015-02-11T10:50:00Z
tags:
- Docker
- Vagrant
- CLI
- Fusion
- YAML
title: Multi-Container Docker with YAML and Vagrant
url: /2015/02/11/multi-container-docker-yaml-vagrant/
---

In this post, I'll provide an example of using YAML to create a multi-container Docker environment in Vagrant. I made a brief mention of this technique in my earlier post talking about [how to use Docker with Vagrant][xref-1], but wanted to provide an example. For me, I know that examples are often quite helpful when I'm learning something new. Since one of my primary goals here is to help enable others to learn these technologies, I figured an example would be helpful. So, to that end, here's an example that I hope will help others.

As is becoming my custom, you can find resources to help you replicate this environment on your own laptop/desktop/whatever via [my "learning-tools" GitHub repository][link-1].

Before I get into the details, I want to just very quickly recap some information from my earlier post on using Docker with Vagrant:

* Vagrant has a built-in Docker provider (present since version 1.6).
* Unless running on Linux, Vagrant will (by default) spin up an instance of a boot2docker VM on which to host the Docker containers. If you decide to modify this behavior (see the earlier post for full details), you'll end up with a second `Vagrantfile` that manages the host VM.
* Vagrant also has a Docker provisioner that is capable of installing Docker onto a host VM.
* As with all other Vagrant projects, there will be a main `Vagrantfile` that controls the specific Docker containers Vagrant will instantiate on the host VM.

All of this was covered in my earlier post, so refer back there for more details.

In this post, I'll re-use the same `Vagrantfile` for the host VM, which tells Vagrant to use an Ubuntu 14.04 base box instead of the default boot2docker box. For the sake of completeness, here's that host VM `Vagrantfile`:

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

This `Vagrantfile` is pretty simple, and I've added comments to help explain each command present. As in my earlier post, I've stored this file in a `host` subdirectory off the main project directory.

The real meat of this post, though, is the use of a separate YAML file to provide the details on the Docker containers that Vagrant should create. This is a technique I've used elsewhere (see [here][xref-2] for an example). In this particular case, I've used a file named `containers.yml`, the contents of which look like this:

```yaml
---
- name: nginx-01
  image: nginx
  ports: ['80:80', '443:443']
- name: redis-01
  image: redis
  ports: ['6379:6379']
```

As you can see, the YAML file specifies two containers, and for each container three properties are provided: a user-friendly name, the specific Docker image to use, and the network ports that should be exposed. If you wanted to create more containers, you'd simply edit this YAML file to define these properties for additional containers. Note that these containers are all getting instantiated on the same host VM, so be mindful of port conflicts when exposing ports.

The main `Vagrantfile`, which Vagrant will use to create Docker containers on the host VM, now looks like this:

```ruby
# -*- mode: ruby -*-
# vi: set ft=ruby :

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
ENV['VAGRANT_DEFAULT_PROVIDER'] = 'docker'

# Require 'yaml' module
require 'yaml'

# Read details of containers to be created from YAML file
# Be sure to edit 'containers.yml' to provide container details
containers = YAML.load_file('containers.yml')

# Create and configure the Docker container(s)
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Perform one-time configuration of Docker provider to specify
  # location of Vagrantfile for host VM; comment out this section
  # to use default boot2docker box
  config.vm.provider "docker" do |docker|
    docker.vagrant_vagrantfile = "host/Vagrantfile"
  end

  # Iterate through the entries in the YAML file
  containers.each do |container|
    config.vm.define container["name"] do |cntnr|

      # Disable synced folders for the Docker container
      # (prevents an NFS error on "vagrant up")
      cntnr.vm.synced_folder ".", "/vagrant", disabled: true

      # Configure the Docker provider for Vagrant
      cntnr.vm.provider "docker" do |docker|

        # Specify the Docker image to use, pull value from YAML file
        docker.image = container["image"]

        # Specify port mappings, pull value from YAML file
        # If omitted, no ports are mapped!
        docker.ports = container["ports"]

        # Specify a friendly name for the Docker container, pull from YAML file
        docker.name = container["name"]
      end
    end
  end
end
```

Again, I've tried to heavily comment the `Vagrantfile` so that it's easy for others to follow along and figure out what the various commands do. At a high-level, this `Vagrantfile` does the following:

* Reads in the specified YAML file using the Ruby YAML module; the contents of the YAML file are now found in an array named "containers".
* Configures the Docker provider to use the specified host VM. This only needs to be done once, so it's found outside the loop that creates the containers.
* Iterates through the array of containers, creating a Docker container for each one specified in the YAML file.

When you run `vagrant up` in the main project directory, Vagrant will---if needed---instantiate the host VM (using the user-specified box) and provision Docker to that host VM, then turn up the list of containers as specified in the YAML file. The host VM just runs on the default private network that Vagrant uses, so if your virtualization platform provides connectivity to VMs on that network ([VMware Fusion][link-2] does for sure, I think [VirtualBox][link-3] might too) then you can try connecting to the host VM's IP address on a specified port to demonstrate that the Docker containers are running.

Similarly, you could use `vagrant global-status` to get the ID for the host VM, then use `vagrant ssh <VM ID>` to connect to the host VM to run commands like `docker images` or `docker ps`.

## Additional Resources

As I mentioned earlier, sample `Vagrantfiles` and supporting documentation to replicate this setup in your own environment can be found in [my "learning-tools" GitHub repository][link-1]. Just look in the "vagrant-docker-yaml" subdirectory.

[link-1]: https://github.com/scottslowe/learning-tools
[link-2]: http://www.vmware.com/products/fusion/
[link-3]: http://www.virtualbox.org/
[xref-1]: {{< relref "2015-02-10-using-docker-with-vagrant.md" >}}
[xref-2]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
