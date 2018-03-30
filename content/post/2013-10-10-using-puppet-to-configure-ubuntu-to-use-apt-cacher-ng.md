---
author: slowe
categories: Explanation
comments: true
date: 2013-10-10T15:14:26Z
slug: using-puppet-to-configure-ubuntu-to-use-apt-cacher-ng
tags:
- Automation
- Linux
- Puppet
- Ubuntu
title: Using Puppet to Configure Ubuntu to use Apt-Cacher-NG
url: /2013/10/10/using-puppet-to-configure-ubuntu-to-use-apt-cacher-ng/
wordpress_id: 3307
---

In this post, I'll share a brief snippet of Puppet code that allows you to automatically configure Ubuntu clients to use Apt-Cacher-NG. By leveraging Apt-Cacher-NG, running `apt-get` commands on your Ubuntu instances will generally be faster because the Apt-Cacher-NG server will cache information locally instead of requiring that every command go out to the source repositories. In my own lab I've seen a tremendous speed boost on installing updates and frequently-used packages on my Ubuntu instances. You can get more information on Apt-Cacher-NG on [the Apt-Cacher-NG website](https://www.unix-ag.uni-kl.de/~bloch/acng/).

(Note: The Puppet code in this post relies upon the same [Puppet Labs apt module](http://forge.puppetlabs.com/puppetlabs/apt) that I used in my post on [using Puppet to configure Ubuntu to use the Ubuntu Cloud Archive][1].)

This snippet of Puppet code will take care of configuring apt to use a local Apt-Cacher-NG instance:

``` puppet
class {'apt':
  proxy_host        =>  'apt-cacher-ng.example.com',
  proxy_port        =>  '3142',
}
```

(You can also see this code block as a GitHub Gist [here](https://gist.github.com/scottslowe/6924675).)

This is a really simple block of code, but I'm publishing it here just for the sake of completeness and in the remote event someone else will find it useful. Because this a distro-specific thing (only applies to Debian and Debian derivatives like Ubuntu), you might want to wrap this in a conditional (like `If $::osfamily == 'Debian'` or similar) to prevent errors in the event this manifest is (accidentally) applied to a non-Debian distribution.

Questions, corrections, and other feedback are welcome, so feel free to speak up in the comments below.

[1]: {{< relref "2013-10-04-using-puppet-for-ubuntu-cloud-archive-support.md" >}}
