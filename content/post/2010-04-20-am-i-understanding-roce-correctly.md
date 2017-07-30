---
author: slowe
categories: Explanation
comments: true
date: 2010-04-20T22:54:13Z
slug: am-i-understanding-roce-correctly
tags:
- FCoE
- Networking
title: Am I Understanding RoCE Correctly?
url: /2010/04/20/am-i-understanding-roce-correctly/
wordpress_id: 1886
---

A couple of days ago I posted a tweet inquiring about RDMA (Remote Direct Memory Access) over Converged Ethernet, affectionately known as RoCE and even more affectionately pronounced "Rocky". At the time I was unclear exactly what RoCE was and what it was trying to accomplish.

Since then, I've done a bit more research and I _think_ that I have a better idea of RoCE now. In particular, [this EE Times article](http://www.eetimes.com/news/latest/showArticle.jhtml?articleID=224400629) provided some information that I found useful in putting the pieces together.

If I understand things correctly, RoCE does for InfiniBand what FCoE did for Fibre Channel---it replaces the physical transport mechanism with 10 Gigabit Ethernet. More specifically, Converged Ethernet, which is the particular flavor of 10Gb Ethernet that supports the IEEE Data Center Bridging (DCB) standards like Priority-Based Flow Control (802.1Qbb), Enhanced Transmission Selection (802.1Qaz), and Congestion Notification (802.1Qau). These DCB standards are intended to make Ethernet less "lossy" and "chatty" and more reliable, predictable, and lossless like Fibre Channel (or InfiniBand).

Fibre Channel over Ethernet (FCoE) took the physical transport layers of traditional Fibre Channel and replaced them with Ethernet, and the IEEE created the DCB efforts to make sure that the underlying Ethernet transport was reliable and lossless so that it could support FCoE. Because it still "looked" like Fibre Channel at the upper layers, there is a great deal of interoperability between Fibre Channel and FCoE.

In a similar fashion, RDMA over Converged Ethernet (RoCE) does the same sort of thing, but for the RDMA interfaces that are common to InfiniBand. It takes RDMA and puts it on Ethernet, again relying upon the IEEE DCB standards to make Ethernet reliable and lossless with predictable latencies. No more proprietary fabrics; with RoCE-capable adapters, you'll be able to reap the ultra-low latency benefits of InfiniBand over standards-based 10Gb Converged Ethernet.

At least, that's how I understand it. Anyone else have a better explanation?
