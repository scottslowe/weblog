---
author: slowe
categories: News
comments: true
date: 2014-08-12T09:54:56Z
slug: cloud-hosted-cloud-management-via-platform9
tags:
- Docker
- KVM
- OpenStack
- vSphere
- Virtualization
title: Cloud-Hosted Cloud Management via Platform9
url: /2014/08/12/cloud-hosted-cloud-management-via-platform9/
wordpress_id: 3493
---

A new startup emerged from stealth today, a company called [Platform9](http://www.platform9.com/). Platform9 was launched by former VMware veterans with the goal of making it easy for companies to consume their existing infrastructure in an agile, cloud-like fashion. Platform9 seeks to accomplish this by offering a cloud management platform that is itself provided as a cloud-based service---hence the name of this post, "cloud-hosted cloud management."

It's an interesting approach, and it certainly helps eliminate some of the complexity that organizations face when implementing their own cloud management platform. For now, at least, that is especially true for OpenStack, which can be notoriously difficult for newcomers to the popular open source cloud management environment. By Platform9 offering an OpenStack API-compatible service, organizations that want a more "public cloud-like" experience can get it without all the added hassle.

The announcements for Platform9 talk about support for KVM, vSphere, and Docker, though the product will only GA with KVM support (support for vSphere and Docker are on the roadmap). Networking support is also limited; in the initial release, Platform9 will look for Linux bridges with matching names in order to stitch together networks. However, customers will get an easy, non-disruptive setup with a nice set of dashboards to help show how their capacity is being utilized and allocated.

It will be interesting to see how things progress for Platform9. The idea of providing cloud management via an SaaS model (makes me think of "cloud inception") is an interesting one that does sidestep many adoption hurdles, though questions of security, privacy, confidentiality, etc., may still hinder adoption in some environments.

Thoughts on Platform9? Feel free to speak up in the comments below. All courteous comments are welcome!
