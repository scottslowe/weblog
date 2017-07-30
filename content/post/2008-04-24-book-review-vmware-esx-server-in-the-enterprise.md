---
author: slowe
categories: Review
comments: true
date: 2008-04-24T20:52:50Z
slug: book-review-vmware-esx-server-in-the-enterprise
tags:
- ESX
- Virtualization
- VMware
- ESX
- Security
- Writing
title: 'Book Review: VMware ESX Server in the Enterprise'
url: /2008/04/24/book-review-vmware-esx-server-in-the-enterprise/
wordpress_id: 694
---

_VMware ESX Server in the Enterprise: Planning and Securing Virtualization Servers_, by Edward Haletky, is a book I've been working on reviewing for quite a while now. It's a fairly hefty tome, weighing in at just over 550 pages, and is chock full of technical details on both ESX Server 2.5.x and ESX 3.0.x. Throughout the book, the author faithfully covers information for both versions---where applicable, of course---and highlights differences and similarities.

Although I personally found the constant "back and forth" between ESX Server 2.5.x and ESX 3.0.x to be distracting, I can easily see where readers fresh out of an ESX Server 2.5.x migration---or still managing some servers running ESX Server 2.5.x---would find that aspect of the book useful. VMware administrators familiar with the older version but perhaps just now making the move to VI3 would also find the book useful as a "transition" manual.

I did run into a few technical inaccuracies, but these are minor in scope and do not materially affect the content. For example, on page 180 in the section titled "iSCSI/NFS Best Practices," the author makes the following statement:

>The iSCSI VMkernel device _must_ be part of at least one service console vSwitch, which implies that the service console must be on the same network as the iSCSI servers. This is required whether using CHAP authentication, or not, to pass credentials to the iSCSI server.

Technically, the iSCSI VMkernel device can be on _any_ vSwitch; the requirement is that there is Service Console connectivity to the iSCSI target. This connectivity could be direct (on the same subnet) or routed. While following the author's guidance and placing a Service Console port group on the same vSwitch and same IP network as the iSCSI VMkernel device will most certainly work, it's not _required._ It's a very minor inaccuracy, as I said earlier, and does not substantially or materially change the validity of the material.

#### Summary

Overall, I found the book to be good reference material. Haletky covers a broad range of topics, from installation to storage and networking to disaster recovery. Anyone needing reference material for such a wide range of topics could do far worse than choosing this book.
