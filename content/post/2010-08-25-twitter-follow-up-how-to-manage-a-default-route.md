---
author: slowe
categories: Musing
comments: true
date: 2010-08-25T14:34:25Z
slug: twitter-follow-up-how-to-manage-a-default-route
tags:
- Cisco
- Networking
title: 'Twitter Follow-Up: How to Manage a Default Route?'
url: /2010/08/25/twitter-follow-up-how-to-manage-a-default-route/
wordpress_id: 2051
---

I posted a [tweet](https://twitter.com/scott_lowe/status/22110728878) earlier today that asked this question:

>If I shouldn't use "ip default-network" because it's classful, then should I redistribute a static default route?

What prompted this question was some work I was doing earlier today in preparation for my CCNA exam. I had a five-site hub-and-spoke network in GNS3 running EIGRP, with lightweight OpenBSD VMs attached behind each router so that I could test end-to-end connectivity (i.e., ping a host behind one router from a host behind another router). This configuration is working fine.

Then I decided I'd take this setup and hide it behind a Vyatta VM performing NAT and see if I could connect it to the rest of my home network. The Vyatta stuff works fine, but now I'm faced with the prospect of configuring this self-contained little environment with a default route that points to the Vyatta. The Vyatta, in turn, points to the physical firewall protecting the home network from the nasty Internet. This configuration doesn't seem at all too far-fetched from a realistic deployment where an enterprise network would need a default route out to the Internet, presumably through a firewall performing network address translation.

So what's the best way to do it? I've read a couple of articles (older ones, since that's all that seems to be available) saying that the `ip default-network` shouldn't be used because it's classful. To be honest, I'm not sure I fully understand the behavior of that command anyway, but if I'm not supposed to use that then do I just set a static route and redistribute that into EIGRP for distribution to the rest of the routers?

Sorry, I'm still learning here...
