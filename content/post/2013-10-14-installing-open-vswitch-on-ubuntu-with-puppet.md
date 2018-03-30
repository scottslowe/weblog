---
author: slowe
categories: Explanation
comments: true
date: 2013-10-14T09:00:00Z
slug: installing-open-vswitch-on-ubuntu-with-puppet
tags:
- Automation
- Linux
- Networking
- OVS
- Puppet
- Ubuntu
title: Installing Open vSwitch on Ubuntu with Puppet
url: /2013/10/14/installing-open-vswitch-on-ubuntu-with-puppet/
wordpress_id: 3311
---

In this post, I'll share with you some [Puppet](http://www.puppetlabs.com/) code that you can include in your manifests to install [Open vSwitch (OVS)](http://openvswitch.org/) packages on Ubuntu. This post, along with a number of others (like [using Puppet for Ubuntu Cloud Archive support][1] or [using Puppet to configure an Apt proxy][2]) stems from my work on building a new home lab in which I'll be doing some OpenStack and NSX testing.

This code makes a couple of assumptions:

1. It assumes that you've established an internal Apt repository (I created one using `reprepro`). In the code below, you'll see that I've used [the Puppet Labs Apt module](http://forge.puppetlabs.com/puppetlabs/apt) to define my internal Apt repository.

2. It assumes that you have Debian packages for OVS in that internal Apt repository. Depending on which version of OVS you need (I needed a newer version than was available in the public repositories), you might be able to get away with just using the public repositories.

OK, with the assumptions out of the way, let's have a look at the code. If you're interested in downloading this code (for your own use or modification), please click [here](https://gist.github.com/scottslowe/6938382).

``` puppet
if $::operatingsystem == 'Ubuntu' {

  # Install prerequisite packages
  package {'make':
    ensure            => 'installed',
  }

  package {'dkms':
    ensure            => 'installed',
  }

  package {'libc6-dev':
    ensure            => 'installed',
  }

  # Install Open vSwitch common files from internal repository
  package {'openvswitch-common':
    ensure            => 'installed',
    name              => 'openvswitch-common',
    require           => Apt::Source['internal'],
  }

  # Install Open vSwitch switch from internal repository
  package {'openvswitch-switch':
    ensure            => 'installed',
    name              => 'openvswitch-switch',
    require           => Apt::Source['internal'],
  }

  # Install Open vSwitch kernel module form internal repository
  package {'openvswitch-datapath-dkms':
    ensure            => 'installed',
    name              => 'openvswitch-datapath-dkms',
    require           => [ Apt::Source['internal'],
                          Package['make'], Package['dkms'],
                          Package['libc6-dev'] ],
  }
}
```

The code is fairly straightforward; the key is making sure that the appropriate packages are installed _before_ you attempt to install the OVS DKMS module. This is reflected in the `require` statement for the `openvswitch-datapath-dkms` package.

I've only tested this on Ubuntu 12.04 LTS, so use at your own risk on other distributions and other versions.

As always, I encourage you to participate in the discussion by adding your questions, thoughts, suggestions, and/or clarifications in the comments below. All courteous comments are welcome.

[1]: {{< relref "2013-10-04-using-puppet-for-ubuntu-cloud-archive-support.md" >}}
[2]: {{< relref "2013-10-10-using-puppet-to-configure-ubuntu-to-use-apt-cacher-ng.md" >}}
