---
author: slowe
categories: Explanation
comments: true
date: 2016-01-14T00:00:00Z
tags:
- Vagrant
- CLI
- Virtualization
- YAML
title: An Improved Way to use YAML with Vagrant
url: /2016/01/14/improved-way-yaml-vagrant/
---

In this post, I'd like to share with you an improved way to use YAML with [Vagrant][link-1]. I first discussed the use of YAML with Vagrant in [a post on simplifying multi-machine Vagrant environments][xref-1], where I simply factored out variable data into an external YAML file. The original approach I described had (at least) one significant drawback, though, which this new approach adddresses.

(By the way, this "improved" way is probably just a matter of better coding. I'm not an expert with Ruby, so Ruby experts may look at this and find it to be quite obvious.)

Here's the _original_ snippet of a `Vagrantfile` that I shared in that first Vagrant/YAML post:

``` ruby
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

The "magic," so to speak, comes from the `servers = YAML.load_file('servers.yml')` line, where Vagrant loads the information from the external YAML file named "servers.yml" into an array. The `servers.each` loop later iterates through the entries in the array, creating and configuring a VM for each record in the YAML file.

This works fabulously well, with one exception: _you can't use `vagrant` commands outside of the directory where the `Vagrantfile` and the YAML file are stored._

What do I mean by this? You may be aware that once a Vagrant environment has been created (you've run `vagrant up` on the environment and haven't yet run `vagrant destroy`), then you can run this command _from any directory_ to get a list of created Vagrant environments:

    vagrant global-status

The output of that command will look something like this (along with some other text after the listing):

    id       name    provider      state       directory
    ----------------------------------------------------------------------
    8fa3dfc  trusty  vmware_fusion not running /home/slowe/vagrant/trusty

Using the value in the `id` column (`8fa3dfc`, in this example), you could run various Vagrant commands, like `vagrant up 8fa3dfc` or `vagrant halt 8fa3dfc`, even when you aren't in the directory where the Vagrant environment was defined. This is a pretty handy feature, but it _doesn't work_ if you use the Vagrant + YAML approach listed above. It errors out, indicating that it can't find the referenced YAML file (`servers.yml`, for example).

The fix for this is to modify the line where Vagrant loads the data from the YAML file. This is the original line, which produces the inability to use global Vagrant commands:

``` ruby
servers = YAML.load_file('servers.yaml')
```

The fixed/corrected/improved version looks like this:

``` ruby
servers = YAML.load_file(File.join(File.dirname(__FILE__), 'servers.yml'))
```

The difference here is that the second command creates the full path to the referenced YAML file (by determining the directory name of the `Vagrantfile`, referred to by the special `__FILE__` variable, and then appending the name of the YAML file itself).

With this approach, you're now able to use global Vagrant commands from any directory without an error. I've recently modified all the Vagrant environments in [my GitHub "learning-tools" repository][link-2] to use this new approach, so that should make those environments a bit easier to use.


[link-1]: http://www.vagrantup.com/
[link-2]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
