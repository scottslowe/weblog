---
author: slowe
categories: Explanation
comments: true
date: 2014-10-22T09:00:00Z
slug: multi-machine-vagrant-with-yaml
tags:
- Automation
- Linux
- Virtualization
- Vagrant
- YAML
title: Multi-Machine Vagrant with YAML
url: /2014/10/22/multi-machine-vagrant-with-yaml/
wordpress_id: 3551
---

In this post, I'll describe a technique I found for simplifying the use of multi-machine Vagrantfiles by extracting configuration data into a separate YAML file. This technique is by no means something that I invented or created, so I can't take any credit whatsoever; this is an idea I first saw [here](http://liquidat.wordpress.com/2014/03/03/howto-vagrant-libvirt-multi-multi-machine-ansible-and-puppet/). I wanted to share it here in the hopes that it might prove useful to a larger audience.

If you aren't familiar with Vagrant and Vagrantfiles, you might start with [my quick introduction to Vagrant][1].

I found this technique after trying to find a way to simplify the creation of multiple machines using Vagrant. Specifically, I was trying to create multiple instances of [CoreOS](https://coreos.com) along with an Ubuntu instance for testing things like etcd, fleet, Docker, etc. The Vagrantfile was getting more and more complex, and making changes (to add another CoreOS node, for example) wasn't as straightforward as I would have liked.

So what's the fix? As with other DSLs (domain-specific languages) such as Puppet, the fix was found in separating the data from the code. In Puppet, that means parameterizing the module or class, and I needed to use a similar technique here with Vagrant. So, based on the example provided [here](http://liquidat.wordpress.com/2014/03/03/howto-vagrant-libvirt-multi-multi-machine-ansible-and-puppet/), I came up with this Vagrantfile:

```ruby
# -*- mode: ruby -*-
# # vi: set ft=ruby :
 
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"
 
# Require YAML module
require 'yaml'
 
# Read YAML file with box details
servers = YAML.load_file('servers.yaml')
 
# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  # Iterate through entries in YAML file
  servers.each do |servers|
    config.vm.define servers["name"] do |srv|
      srv.vm.box = servers["box"]
      srv.vm.network "private_network", ip: servers["ip"]
      srv.vm.provider :virtualbox do |vb|
        vb.name = servers["name"]
        vb.memory = servers["ram"]
      end
    end
  end
end
```

(click [here](https://gist.github.com/scottslowe/eae23022db8cc95508c6) to get this as a GitHub Gist you can download.)

You'll note that there is no specific data in this Vagrantfile; all the pertinent configuration data is found in an external YAML file, named `servers.yaml` (the name is obviously configurable within the Vagrantfile). The Vagrantfile simply retrieves the data from the YAML file and iterates over it.

Here's a sample `servers.yaml` you could use with this Vagrantfile:

```yaml
---
- name: coreos-01
  box: coreos-alpha
  ram: 512
  ip: 172.17.8.101
- name: coreos-02
  box: coreos-alpha
  ram: 512
  ip: 172.17.8.102
- name: coreos-03
  box: coreos-alpha
  ram: 512
  ip: 172.17.8.103
```

(Click [here](https://gist.github.com/scottslowe/4ca1e0c960bbd450ea0e) for a downloadable GitHub Gist.)

Using the sample `servers.yaml` file shown above, the Vagrantfile would create three VMs, using the CoreOS Alpha Vagrant box, each with 512MB of RAM and the corresponding IP address. To add more systems to the configuration, simply add lines to the YAML file and you're done. It's as simple as that.

Clearly, there's a lot more that could be done with this sort of approach, but I think this bare-bones example should give you an idea of how flexible something like this can be.

For an even more supercharged approach, check out [Oh My Vagrant!](https://ttboj.wordpress.com/2014/09/03/introducing-oh-my-vagrant/). It takes the approach I've described here to an entirely new level.

[1]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
