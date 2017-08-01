---
author: slowe
categories: Explanation
comments: true
date: 2016-03-21T00:00:00Z
tags:
- Ubuntu
- Linux
- Vagrant
- Networking
- CLI
title: Vagrant, Ubuntu "Wily Werewolf," and Networking
url: /2016/03/21/vagrant-ubuntu-wily-networking-problem/
---

In what has been a fairly classic ["yak shaving"][link-3] exercise, I've been working on getting [Ubuntu 15.10 "Wily Werewolf"][link-1] running with [Vagrant][link-2] so that I can perform some testing with some other technologies that need a Linux kernel version of at least 4.2 (which comes with Ubuntu 15.10 by default). Along the way, I ran smack into a problem with Ubuntu 15.10's networking configuration when used with Vagrant, and in this post I'm going to explain what's happening here and provide a workaround.

The issue (described [here on GitHub][link-4], among other places) involves a couple of changes in Ubuntu Linux (and upstream Debian GNU/Linux as well, although I haven't personally tested it). One of the changes is in regards to how network interfaces are named; instead of the "old" `eth0` or `eth1` naming convention, Ubuntu 15.10 now uses persistent interface names like `ens32` or `ens33`. Additionally, an update to the "ifupdown" package now returns an error where an error apparently wasn't returned before.

The end result is that when you try to create a Vagrant VM with multiple network interfaces, it fails. Using a single network interface is fine; the issue only rears its head when you want to use multiple network interfaces. I don't know about you, but _I_ use multiple network interfaces quite frequently, so this is a real problem for me. Fortunately, there's a workaround.

As you may know, I'm a fan of using parameterized data stored in YAML files with Vagrant (see [here][xref-1] and [here][xref-2] for more information). So most of my Vagrant environments have this snippet of code in the `Vagrantfile`:

``` ruby
if server['ip_addr'] != nil
  srv.vm.network 'private_network', ip: server['ip_addr']
end # if server['ip_addr']
```

This snippet of code basically says that if a value exists for `ip_addr` in the YAML file, use that value as the IP address to be assigned to an additional private network. (Note that it doesn't do any validity checking on the value, so clearly there's room to make this more robust.) If the value is nil (there is no value), then no additional network interface is defined. This makes it easy for me to include an extra private network on a per-machine basis simply by including an IP address in the YAML file.

Obviously, given the issue with Vagrant and Ubuntu 15.10, this won't work. The workaround, then, is to simply not use Vagrant's network interface functionality, like this:

``` ruby
if server['ip_addr'] != nil
  srv.vm.network 'private_network', auto_config: false
end # if server['ip_addr']
```

This modified snippet says that if a value exists for `ip_addr` in the YAML file, create an additional network interface but _do not configure the interface._ This works---Vagrant will create the VM with the additional interface, but won't try to configure the interface in any way, shape, form, or fashion.

Once the interface has been created by Vagrant, then you can configure the interface manually, using [Ansible][link-5], or via whatever tool makes the most sense to you. I'm still working through some methods for configuring the interface via either an ERB template or via Ansible; I've run into a few snags but hope to have those worked out soon. In the meantime, if you're using Ubuntu 15.10 with Vagrant, keep this potential issue in mind and plan to work around the problem until a (permanent) fix is available.



[link-1]: https://wiki.ubuntu.com/WilyWerewolf/ReleaseNotes
[link-2]: https://www.vagrantup.com/
[link-3]: http://projects.csail.mit.edu/gsb/old-archive/gsb-archive/gsb2000-02-11.html
[link-4]: https://github.com/mitchellh/vagrant/issues/6871
[link-5]: https://www.ansible.com/
[xref-1]: {{< relref "2014-10-22-multi-machine-vagrant-with-yaml.md" >}}
[xref-2]: {{< relref "2016-01-14-improved-way-yaml-vagrant.md" >}}
