---
author: slowe
categories: Tutorial
comments: true
date: 2015-05-21T07:20:00Z
tags:
- Linux
- Ubuntu
title: Using an Apt Proxy
url: /2015/05/21/using-apt-proxy/
---

In this post I'll show you how to use [apt-cacher-ng][link-1] as an Apt proxy for Ubuntu systems on your network. I'm sure there are a lot of other resources that also provide this information, but I'm including it here for the sake of completeness and making it as easy as possible for others. Using an Apt proxy will help reduce the traffic coming from your network to external repositories, but it simpler and easier than running your own internal repository or mirror.

This isn't the first time I've discussed apt-cacher-ng; almost two years ago I showed you [how to use Puppet to configure Ubuntu to use apt-cacher-ng][link-2]. This post focuses on the manual configuration of an Apt proxy.

On the server side, setting up an Apt proxy is as simple as one command:

    apt-get install apt-cacher-ng

I'm sure there are some optimizations or advanced configurations, but this is enough to get the Apt proxy up and running.

On the client side, there are a couple of ways to configure the system. You could use a tool like Puppet (as described [here][link-2]), or manually configure the system. If you choose manual configuration, you can place the configuration in either `/etc/apt/apt.conf` or in a standalone file in `/etc/apt/apt.conf.d`. Either way, the contents to add look like this:

    Acquire::http { Proxy "http://apt-proxy-name.domain.com:3142"; };

(Naturally, you'll want to substitute the correct server name/FQDN instead of `apt-proxy-name.domain.com` shown above.)



[link-1]: https://www.unix-ag.uni-kl.de/~bloch/acng/
[link-2]: {{< relref "2013-10-10-using-puppet-to-configure-ubuntu-to-use-apt-cacher-ng.md" >}}