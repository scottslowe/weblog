---
author: slowe
categories: Explanation
comments: true
date: 2013-10-04T10:12:22Z
slug: using-puppet-for-ubuntu-cloud-archive-support
tags:
- Automation
- Linux
- OpenStack
- Puppet
- Ubuntu
title: Using Puppet for Ubuntu Cloud Archive Support
url: /2013/10/04/using-puppet-for-ubuntu-cloud-archive-support/
wordpress_id: 3303
---

In this post, I'll share a snippet of [Puppet](http://www.puppetlabs.com/) code that I am using to automatically configure Ubuntu Server 12.04 systems to use the Ubuntu Cloud Archive (which allows access to packaged versions of OpenStack for use with LTS releases of Ubuntu Server, like 12.04).

As you may already know, I recently acquired two off-lease Dell PowerEdge C6100 systems. Each of these units has four trays; each tray is a dual-socket, quad-core server with 24GB of RAM. This gives me a total of eight servers, and the plan is to use them to build an internal OpenStack cloud running Ubuntu 12.04, KVM, Open vSwitch (OVS), and---ultimately---VMware NSX. It's a fairly ambitious goal, but if you don't stretch yourself you'll never grow.

In any case, along the way I'm trying to make the whole process as repeatable and automated as possible, and naturally that's where Puppet comes into play. I've been working through an automated Ubuntu Server install via PXE and an internal HTTP repository (I'll do a separate post for that), but as part of my testing I wanted to be sure that I could automatically configure the Ubuntu Server instances to use the Ubuntu Cloud Archive for access to OpenStack packages. While this isn't necessarily _hard_, I did want to share the Puppet code I'm using just in case it might help someone else.

First off, you'll want to get your hands on the [Puppet Labs apt module from the Forge](http://forge.puppetlabs.com/puppetlabs/apt). Once you've gotten that installed on your Puppet server (a simple `puppet module install puppetlabs/apt` on any recent version of Puppet should knock that out for you), then you can use this snippet of code in a manifest:

``` puppet
apt::source { 'ubuntu-cloud':
  location          =>  'http://ubuntu-cloud.archive.canonical.com/ubuntu',
  repos             =>  'main',
  release           =>  'precise-updates/grizzly',
  include_src       =>  false,
  required_packages =>  'ubuntu-cloud-keyring',
}
```

(You can download this code from [here](https://gist.github.com/scottslowe/6827241), if you'd like.)

Once you put this into the Puppet manifest and then refresh the system's configuration, you should see a file named `ubuntu-cloud.list` appear in the `/etc/apt/sources.list.d` directory on your Ubuntu system. (By the way, I usually wrap that code in a conditional like `if $::operatingsystem == 'Ubuntu'` or similar.) Once that file is there, simply run `apt-get update` and you should now be able to install packages from the Ubuntu Cloud Archive.

Have fun!
