---
author: slowe
categories: News
comments: true
date: 2009-06-05T14:21:05Z
slug: adaptec-support-in-vsphere-revoked
tags:
- ESX
- ESXi
- Hardware
- Storage
- Virtualization
- VMware
- vSphere
title: Adaptec Support in vSphere Revoked?
url: /2009/06/05/adaptec-support-in-vsphere-revoked/
wordpress_id: 1388
---

It would _appear_ (I have not yet been able to reliably verify this information) that VMware has removed support for the Adaptec `aacraid` driver from VMware vSphere. The driver was apparently listed on the Hardware Compatibility List (HCL) as supported at release, but shortly after release support was pulled. Apparently the `aacraid` driver is in such bad shape with vSphere that you can't even install vSphere on systems using the `aacraid` driver. Even more mysterious is the fact that there is no documentation about this issue one way or the other, so it's quite difficult to really verify where support stands. As of the last time I checked (a couple of days ago), the `aacraid` driver was still not listed on the HCL.

This primarily impacts white-box owners; users of systems from major vendors like HP, IBM, and Dell will most likely not be affected.

If anyone has any additional information on this matter, please speak up in the comments.
