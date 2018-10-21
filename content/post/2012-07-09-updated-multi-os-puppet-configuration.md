---
author: slowe
categories: Explanation
comments: true
date: 2012-07-09T09:00:00Z
slug: updated-multi-os-puppet-configuration
tags:
- Automation
- BSD
- Interoperability
- Linux
- OSS
- Puppet
- UNIX
title: Updated Multi-OS Puppet Configuration
url: /2012/07/09/updated-multi-os-puppet-configuration/
wordpress_id: 2676
---

I received some great feedback on my post about [using Puppet with multiple operating systems][1]. One of the suggestions was to do a better job of following the "official" Puppet style guide for syntax and file layout. With that in mind, I installed `puppet-lint` on my Puppet master server using `apt-get install rubygems` followed by `gem install puppet-lint`.

Using `puppet-lint`, I was able to correct all "errors" in the manifest, but was left with one warning regarding line length. This is, as far as I know, an uncorrectable warning, as the line that `puppet-lint` finds is the line that specifies the source of the OpenBSD packages. I can't shorten the URL to the packages because that's not under my control.

In any case, here's the corrected Puppet manifests. I checked these with both `puppet parser validate` and `puppet-lint`, but if anyone has anything I've missed feel free to point it out in the comments. Unless otherwise stated, all files are placed in the `modules/ntp/manifests` directory under the Puppet directory (which on my Puppet master server is `/etc/puppet`).

First up is the `init.pp` manifest.

```text
# NTP class definition

class ntp {
  include "ntp::$::operatingsystem"

  file { 'ntp.conf':
    ensure        => present,
    path          => '/etc/ntp.conf',
    owner         => 'root',
    group         => 'root',
    mode          => '0644',
    source        => 'puppet:///modules/ntp/ntp.conf',
    require       => Package['ntp'],
  }

  package { 'ntp':
    ensure        => installed,
  }

  service { 'ntp':
    ensure        => running,
    subscribe     => File['ntp.conf'],
    require       => File['ntp.conf'],
  }
}
```

Next we have the subclasses for the various operating systems. Here's `openbsd.pp`:

```text
# NTP subclass for OpenBSD
    
class ntp::openbsd inherits ntp {
  File['ntp.conf'] {
    path          => '/etc/ntpd.conf',
    group         => 'wheel',
    source        => "puppet:///modules/ntp/ntpd.conf.$::operatingsystem",
  }

  Package['ntp'] {
    source        => 'http://openbsd.mirrorcatalogs.com/pub/OpenBSD/5.1/packages/i386/',
  }

  Service['ntp'] {
    provider      => 'base',
    hasstatus     => false,
    start         => '/usr/sbin/ntpd',
  }
}
```

Here's the subclass for Ubuntu, stored in `ubuntu.pp`:

```text
# NTP subclass for Ubuntu Linux
    
class ntp::ubuntu inherits ntp {
  Service['ntp'] {
    provider      => 'init',
    path          => '/etc/init.d/',
  }
}
```

And finally, here's `centos.pp`, used for CentOS/Red Hat-based instances:

```text
# NTP subclass for CentOS Linux
    
class ntp::centos inherits ntp {
  Service['ntp'] {
    name          => 'ntpd',
  }
}
```

I know that there are NTP modules for Puppet that "take care" of all this sort of thing for you, but creating this was, for me, part of the learning process. I'm going to tackle SSH next, and---per the comments on my first Puppet post---I've also started reading up on Hiera to see how that might fit in here. The initial reading I've done leads me to believe that the combination of Puppet and Hiera could be quite powerful.

As always, courteous comments are welcome. Feel free to speak up below!

[1]: {{< relref "2012-07-05-using-puppet-with-multiple-operating-systems.md" >}}
