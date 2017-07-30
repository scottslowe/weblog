---
author: slowe
categories: Explanation
comments: true
date: 2016-10-20T00:00:00Z
tags:
- Photon
- Linux
- Vagrant
- CLI
title: Vagrant-Photon OS Bug and Workaround
url: /2016/10/20/vagrant-photon-bug-workaround/
---

I recently came across a bug in using [VMware Photon OS][link-1] with [Vagrant][link-2], and so in this post I'm going to point out this bug and provide a workaround. The bug is, fortunately, pretty innocuous, and only affects Vagrant environments that configure additional network interfaces to Photon OS VMs. The workaround is equally easy, thankfully.

First, I'll point out that [the fix for this bug][link-3] has already been pushed to Vagrant, but it hasn't yet (as of this writing) made it into a release. Vagrant 1.8.6 was the latest release of this writing, and it still exhibits the bug.

There are a number of somewhat-interrelated issues:

1. First, the "vagrant-guests-photon" Vagrant plugin (latest version is 1.0.4) is _no longer needed._ This code has been replaced by code that is distributed as part of Vagrant itself. This wouldn't normally be an issue, except that...

2. The plugin relies on `awk`, which is no longer included in recent releases of the Photon OS Vagrant box. I can't tell you exactly when this started, but I can confirm the last couple of releases (1.2.0 and 1.2.1) are definitely affected.

3. Finally, the code which replaces the "vagrant-guests-photon" plugin (as mentioned above) has a typo that causes an error when additional network interfaces are configured.

The end result of this is that if you try to use Vagrant to spin up a VM based on a recent version of the Photon OS base box, then it will fail if you attempt to configure additional network interfaces (beyond the default NAT interface that Vagrant always configures). You'll either get:

* An `awk`-related error (if you have the plugin installed)
* An error related to a mistyped parameter to the `ifconfig` command (if you don't have the plugin installed)

So what's the fix?

1. Remove the plugin using `vagrant plugin uninstall vagrant-guests-photon`.

2. Edit 2 files that are part of Vagrant itself. The files are `<v-dir>/embedded/gems/gems/vagrant-<ver>/plugins/guests/photon/cap/configure_networks.rb` and `<v-dir>/embedded/gems/gems/vagrant-<ver>/test/unit/plugins/guests/photon/cap/configure_networks_test.rb`. You'll need to replace `<v-dir>` with the directory where Vagrant is installed, and `<ver>` with the version of Vagrant installed on your system. In these two files, replace the one instance of "netmast" with "netmask". (Yes, it's a simple typo.)

Once you've done these two steps, then you're good to go to use Vagrant to spin up Photon OS VMs with additional network interfaces.

I'll update this post as soon as the fix rolls into a Vagrant release and this workaround is no longer needed.

**UPDATE 2016-11-19**: This fix is now available in Vagrant 1.8.7.



[link-1]: https://vmware.github.io/photon/
[link-2]: https://www.vagrantup.com/
[link-3]: https://github.com/mitchellh/vagrant/issues/7808
