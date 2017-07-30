---
author: slowe
categories: Information
comments: true
date: 2012-08-03T10:10:08Z
slug: open-source-network-redesign
tags:
- Interoperability
- BSD
- Linux
- OSS
- UNIX
title: Open Source Network Redesign
url: /2012/08/03/open-source-network-redesign/
wordpress_id: 2715
---

As you might have noticed in recent blog posts, I'm spending a fair amount of time working with open source solutions like [Ubuntu Linux](http://www.ubuntu.com/), [OpenBSD](http://www.openbsd.org/), [Puppet](http://puppetlabs.com/puppet/puppet-open-source/), and similar. As part of the effort to make myself more familiar with these and other open source projects, I've decided to re-architect my home network using predominantly open source software.

Here are the open source software projects that I know for sure I'll end up using:

* Ubuntu Server 12.04 LTS

* OpenBSD (probably version 5.1)

* Squid and the Squidguard content filter

* BIND v9

* ISC DHCP server

* Open source Puppet

However, there are a few packages that I haven't quite settled for sure. I'd love to hear some feedback on these questions:

1. What do you recommend for low-volume web serving---Apache HTTP or Nginx? (Manageability via Puppet is a consideration, too.)

2. It looks as if I can use [Heartbeat](http://www.linux-ha.org/wiki/Heartbeat) to provide high availability/failover at the application level for the web and web proxy services (this would be active/passive only). Anyone have any experience with Heartbeat, or some good resources to share?

3. It would be great if I could actually do load balanced sessions for the web and web proxy services (active/active instead of active/passive). It appears as if [LVS](http://www.linuxvirtualserver.org/) will do this, but it also looks like I'll need separate VMs (everything will be virtualized) for LVS. Anyone have some resources for LVS?

4. Are there any other projects or tools I should be considering?

Thanks for any help or information you can provide!
