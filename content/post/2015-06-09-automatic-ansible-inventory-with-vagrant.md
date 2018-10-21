---
author: slowe
categories: Explanation
comments: true
date: 2015-06-09T07:15:00Z
tags:
- Ansible
- Vagrant
- Linux
- CLI
title: Automatic Ansible Inventory with Vagrant
url: /2015/06/09/automatic-ansible-inventory-with-vagrant/
---

Yesterday, I posted about [using Vagrant to learn Ansible][xref-1], in which I showed you one way to combine these two tools to make it easier to learn [Ansible][link-1]. This is a combination I'm currently using as I continue to explore Ansible. Today, I'm going to expand on yesterday's post by showing you how to make [Vagrant][link-2] automatically build an Ansible inventory for a particular Vagrant environment.

As you may already know, the `Vagrantfile` that Vagrant uses to instantiate and configure the VMs in a particular Vagrant environment is just [Ruby][link-3]. As such, it can be extended in a lot of different ways to do a lot of different things. In my case, I've settled on a design pattern that involves a separate [YAML][link-4] file with all the VM-specific data, which is read by the `Vagrantfile` when the user runs `vagrant up`. The data in the YAML file determines how many VMs are instantiated, what box is used for each VM, and the resources that are allocated to each VM. This is a design pattern I've used repeatedly in [my GitHub "learning-tools" repository][link-5], and it seems to work pretty well (for me, at least).

Using this arrangement, since I already have all the VM-specific data in the external YAML file---this could include IP addresses to be assigned to additional network adapters---it's relatively straightforward to extend the `Vagrantfile` to dynamically build an Ansible inventory file when the user runs `vagrant up`.

Here's the particular snippet of Ruby code to add to the `Vagrantfile`:

```ruby
require "fileutils"
f = File.open("hosts","w")
servers.each do |servers|
  f.puts servers["ip_addr"]
end # servers.each
f.close
```

This snippet of code opens a file named `hosts` (which is the inventory file name specified in the `ansible.cfg` in this directory, as I described in yesterday's post). If the file already exists, it is overwritten---the preferred behavior, actually, since it allows us to write the contents fresh every time the user runs `vagrant up`. It then iterates through the entries from the external YAML file, and writes the IP address to the file, closing the file after the loop is complete.

I'm not a Ruby programmer, so I freely admit there may be a more efficient or effective way of accomplishing this task. That being said, the code above does work. (I leave it as an exercise for the reader to incorporate this into a YAML-equipped `Vagrantfile`. Numerous examples of such are found in my "learning-tools" repository.)

With this approach, your workflow looks like this:

* You edit the external YAML file to include a key (named "ip_addr" in the code snippet above) that includes the IP address Ansible should use for this particular Vagrant environment.
* You ensure there is an Ansible configuration file, as per yesterday's post, in the same directory where your `Vagrantfile` is located.
* You run `vagrant up`. Vagrant instantiates the VMs, configures them according to the external YAML file, and writes the IP addresses to the Ansible inventory file.
* When the Vagrant environment is turned up, you run whatever Ansible ad-hoc commands you need, or run playbooks using `ansible-playbook`. The Ansible configuration file will tailor Ansible to work seamlessly in a Vagrant environment, and the inventory file will tell Ansible to work only against the VMs in that particular Vagrant environment.
* When you're done, you run `vagrant destroy`.
* When you're ready to start again, you run `vagrant up` and start all over again with a fresh environment. The Ansible inventory file gets automatically re-created with the correct IP addresses.

Pretty useful, yes?

[link-1]: http://www.ansible.com/home
[link-2]: https://www.vagrantup.com
[link-3]: https://www.ruby-lang.org/en/
[link-4]: http://yaml.org/
[link-5]: https://github.com/scottslowe/learning-tools
[xref-1]: {{< relref "2015-06-08-using-vagrant-to-learn-ansible.md" >}}
