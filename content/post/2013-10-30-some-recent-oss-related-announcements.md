---
author: slowe
categories: News
comments: true
date: 2013-10-30T10:00:00Z
slug: some-recent-oss-related-announcements
tags:
- Linux
- OpenStack
- OSS
- Storage
title: Some Recent OSS-Related Announcements
url: /2013/10/30/some-recent-oss-related-announcements/
wordpress_id: 3317
---

Recently a couple of open source software (OSS)-related announcements have passed through my Inbox, so I thought I'd make brief mention of them here on the site.

## Mirantis OpenStack

Last week Mirantis [announced the general availability of Mirantis OpenStack](http://www.mirantis.com/company/press-center/company-news/mirantis-ships-first-zero-lock-in-openstack-distribution-designed-for-the-enterprise-market/), its own commercially-supported OpenStack distribution. Mirantis joins a number of other vendors also offering OpenStack distributions, though Mirantis claims to be different on the basis that its OpenStack distribution is not tied to a particular Linux distribution. Mirantis is also differentiating through support for some additional projects:

* [Fuel](https://wiki.openstack.org/wiki/Fuel) (Mirantis' own OpenStack deployment tool)

* [Savanna](https://wiki.openstack.org/wiki/Savanna) (for running Hadoop on OpenStack)

* [Murano](https://wiki.openstack.org/wiki/Murano) (a service for assisting in the deployment of Windows-based services on OpenStack)

It's fairly clear to me that at this stage in OpenStack's lifecycle, professional services are a big play in helping organizations stand up OpenStack (few organizations lack the deep expertise to really stand up sizable installations of OpenStack on their own). However, I'm not yet convinced that building and maintaining your own OpenStack distribution is going to be as useful and valuable for the smaller players, given the pending competition from the major open source players out there. Of course, I'm not an expert, so I could be wrong.

## Inktank Ceph Enterprise

[Ceph](http://ceph.com/), the open source distributed software system, is now coming in a fully-supported version aimed at enterprise markets. Inktank has announced Inktank Ceph Enterprise, a bundle of software and support aimed to increase adoption of Ceph among enterprise customers. Inktank Ceph Enterprise will include:

* Open source Ceph (version 0.67)

* New "Calamari" graphical manager that provides management tools and performance data with the intent of simplifying management and operation of Ceph clusters

* Support services provided by Inktank; this includes technical support, hot fixes, bug prioritization, and roadmap input

Given Ceph's integration with OpenStack, CloudStack, and open source hypervisors and hypervisor management tools (such as [libvirt](http://libvirt.org/)), it will be interesting to see how Inktank Ceph Enterprise takes off. Will the adoption of Inktank Ceph Enterprise be gated by enterprise adoption of these related open source technologies, or will it help drive their adoption? I wonder if it would make sense for Inktank to pursue some integration with VMware, given VMware's strong position in the enterprise market. One thing is for certain: it will be interesting to see how things play out.

As always, feel free to speak up in the comments to share your thoughts on these announcements (or any other related topic). All courteous comments are welcome.
