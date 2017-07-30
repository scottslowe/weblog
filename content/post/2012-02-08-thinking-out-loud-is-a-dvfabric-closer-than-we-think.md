---
author: slowe
categories: Musing
comments: true
date: 2012-02-08T16:42:41Z
slug: thinking-out-loud-is-a-dvfabric-closer-than-we-think
tags:
- FCoE
- FibreChannel
- Storage
- Virtualization
- Networking
- ToL
title: 'Thinking Out Loud: Is A dvFabric Closer Than We Think?'
url: /2012/02/08/thinking-out-loud-is-a-dvfabric-closer-than-we-think/
wordpress_id: 2533
---

This is a short post, but one that I hope will stir some discussion.

Earlier this evening, I read Maish's blog post titled [My Wish for dvFabric---a dvSwitch for Storage](http://technodrone.blogspot.com/2012/02/my-wish-for-dvfabric-dvswitch-for.html). In that blog post Maish describes a self-configuring storage fabric that would simplify how we provision storage in a vSphere environment. Here's how Maish describes the dvFabric:

>So how do I envision this dvFabric? Essentially the same as a dvSwitch. A logical entity to which I attach my network cards (for iSCSI/NFS) or my HBAs (for FcoE or FC). I define which uplink goes to which storage, what the multi-pathing policy is for this uplink, how many ports should be used, what is the failover policy for which NIC, which NFS volumes to mount, which LUNS to add  I gather you see what I am getting at.

It's a pretty interesting idea, and one with a great deal of merit. So here's the "Thinking Out Loud" part: is Target Driven Zoning (or Peer Zoning) the answer to a large part of Maish's dvFabric?

If you don't know what Target Driven Zoning (TDZ) or Peer Zoning are, I recommend you go have a look at Erik Smith's [introductory blog post on Target Driven Zoning](http://brasstacksblog.typepad.com/brass-tacks/2012/01/introducing-target-driven-zoning-tdz.html). Based on Erik's description of TDZ, it certainly seems like it could be used to help on the block side of the house with Maish's dvFabric idea.

So what do you think---am I way off here?
