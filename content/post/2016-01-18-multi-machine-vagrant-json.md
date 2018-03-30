---
author: slowe
categories: Tutorial
comments: true
date: 2016-01-18T00:00:00Z
tags:
- Vagrant
- OSS
- CLI
- JSON
title: Multi-Machine Vagrant Environments with JSON
url: /2016/01/18/multi-machine-vagrant-json/
---

In this post I'd like to show you how to use a JSON-formatted data file to create and configure multi-machine Vagrant environments. This isn't a new idea, and certainly _not_ anything that I came up with or created. I'm simply presenting it here as an alternative option to the approach of using YAML with Vagrant for multi-machine environments (some people may prefer JSON over YAML).

If you're unfamiliar with Vagrant, I'd start with [my introduction to Vagrant][xref-1]. Then I'd recommend reviewing [my original article on using YAML with Vagrant][xref-2], followed by [the updated/improved method][xref-3] that addresses a shortcoming with the original approach. These earlier posts will provide some basics that I'll build on in this post.

To use a JSON-formatted data file as an external data source for Vagrant, the code in the `Vagrantfile` looks really similar to the code you'd use for YAML:

``` ruby
# -*- mode: ruby -*-
# # vi: set ft=ruby :
 
# Specify minimum Vagrant version and Vagrant API version
Vagrant.require_version '>= 1.6.0'
VAGRANTFILE_API_VERSION = '2'
 
# Require JSON module
require 'json'
 
# Read YAML file with box details
servers = JSON.parse(File.read(File.join(File.dirname(__FILE__), 'servers.json')))
 
# Create boxes
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
 
  # Iterate through entries in JSON file
  servers.each do |server|
    config.vm.define server['name'] do |srv|
      srv.vm.box = server['box']
      srv.vm.network 'private_network', ip: server['ip_addr']
      srv.vm.provider :vmware_fusion do |vmw|
        vmw.vmx['memsize'] = server['ram']
        vmw.vmx['numvcpus'] = server['vcpu']
      end # srv.vm.provider
    end # config.vm.define
  end # servers.each
end # Vagrant.configure
```

For the most part, this `Vagrantfile` is very similar to what I've already shown you when using an external YAML data file. The line starting with `servers = ...` is what pulls in the data from the external JSON file; the `JSON.parse` command (method? function?) is required to parse the JSON-formatted data into a hash that we reference later in the `servers.each` loop. The loop itself is otherwise exactly what I've already used in other examples of using an external data file.

And what does the external data file look like? Here's a JSON-formatted data file that would work with this `Vagrantfile`:

``` json
[
    {
        "name": "jessie",
        "box": "slowe/debian-81-x64",
        "ram": 512,
        "vcpu": 1,
        "ip_addr": "192.168.100.101"
    },

    {
        "name": "trusty",
        "box": "slowe/ubuntu-trusty-x64",
        "ram": 512,
        "vcpu": 1,
        "ip_addr": "192.168.100.102"
    }
]
```

Naturally, you could extend this external JSON-formatted data file to do other things. Here's a few examples:

* You could optionally enable (or disable) shared folders based on a value in the external data file.
* You could insert additional values, like enabled nested virtualization support (if using a VMware desktop hypervisor that supports such functionality) if a certain value is included in the external data file.

I've used both these approaches in many of my own personal Vagrant environments without any major issues (using a YAML file, but the concept applies equally here).

## Inspiration/Other Resources

This post was inspired primarily by James Thorne's post on [using JSON and loops to create multi-machine Vagrant environments][link-1]. The idea of using JSON instead of YAML is not mine, but his; I just wanted to present similar information (but with the JSON separated into a different file instead of embedded into the `Vagrantfile`).

If you're interested in experimenting with JSON and Vagrant yourself, have a look at the `vagrant-json` directory in [my GitHub "learning-tools" repository][link-2]. It has a `Vagrantfile` and an associated JSON data file.



[link-1]: http://thornelabs.net/2014/11/13/multi-machine-vagrantfile-with-shorter-cleaner-syntax-using-json-and-loops.html
[link-2]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2014-09-12-a-quick-introduction-to-vagrant.md" >}}
[xref-2]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
[xref-3]: {{< relref "2016-01-14-improved-way-yaml-vagrant.md" >}}
