---
author: slowe
categories: Information
comments: true
date: 2008-02-13T11:44:17Z
slug: recommended-esx-service-console-partitioning
tags:
- ESX
- Storage
- Virtualization
- VMware
title: Recommended ESX Service Console Partitioning
url: /2008/02/13/recommended-esx-service-console-partitioning/
wordpress_id: 631
---

Rich Brambley over at [vmetc.com](http://vmetc.com/) has recently published some information taken from the VMware Authorized Consultant guides regarding the [recommended partitioning scheme](http://vmetc.com/2008/02/12/best-practices-for-esx-host-partitions/) for ESX Server 3.x (not ESX Server 3i). It was good to see that this information closely mirrored the recommended partitioning configuration I'd developed myself.

In fact, I think that the only substantive change in the VAC guidelines from my own guidelines was the size of the swap partition. Anyone care to share their thoughts on the size of the swap partition? How many of you increase the size of the swap partition? How many leave it at the default?

Otherwise, the recommendations to create separate partitions---and the recommended sizes---for `/var`, `/tmp`, and `/home` fell right in line with what I'd already been using. My sizes were typically a bit larger for `/tmp` and `/home`, but otherwise were very comparable.

Thanks to Rich for getting that information disseminated.
