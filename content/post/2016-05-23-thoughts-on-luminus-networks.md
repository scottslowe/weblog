---
author: slowe
categories: Musing
comments: true
date: 2016-05-23T00:00:00Z
tags:
- Networking
title: Thoughts on Luminus Networks
url: /2016/05/23/thoughts-on-luminus-networks/
---

Late last week, Cyrus Durgin from [Luminus Networks][link-1] published [an article on SDx Central titled "The (R)evolution of Network Operations."][link-2] You may notice that my name is mentioned at the bottom of the article as someone who provided feedback. In this post, I'd like to share some thoughts---high-level and conceptual in nature---on network operations and Luminus Networks.

I was first introduced to Luminus Networks when I met its CEO, Kelly Wanser, at the Open Networking User Group (ONUG) meeting in New York City last November. We met again in the Denver area in late December, and Kelly gave me a preview of what Luminus was building. I must confess that I was immediately intrigued by what Kelly was describing. One key thing really jumped out at me: _we need to treat the network as a system, not as a bunch of individual elements._

When it comes to network monitoring/management/operations, so many of the tools are focused on the individual elements that comprise a network: provisioning a switch, pushing configuration changes to a router or group of routers, polling counters from interfaces on switches, etc. While there's nothing wrong with any of these things, it seems to me that there's still a need for _something_ to take a more holistic view of the network.

Think about all the various innovations/changes that are happening in the networking space:

* Devices providing streaming telemetry, instead of relying on polling (which doesn't scale)
* Whitebox/britebox hardware with Linux-based NOSes
* The addition of application programming interfaces (APIs) to existing NOSes
* Using overlay networks to decouple logical network topologies from the physical network topology
* Shifting to spine-leaf topologies instead of the classic 3-tier network architectures
* The increased use of configuration management/orchestration tools like Ansible, Chef, Puppet, or Salt with network devices

All of these things are necessary and exciting, but none of them address the need to view and treat the network holistically. Having a spine-leaf network fabric full of devices providing streaming telemetry and running a NOS that provides RESTful APIs doesn't do you any good if you don't have a solution that can take all that data, analyze and correlate it, and then use the results to proactively _operate_ (instead of passively _manage_) the network.

From what I've been able to gather from both Kelly and Cyrus---I haven't yet seen a demo of the actual solution yet---this is what Luminus is building: a solution that can take advantage of any of the innovations/changes listed above, if they are present, but a solution that treats the network as a system, and applies distributed systems approaches to its own architecture as well as to the operation of networks.

I'm looking forward to seeing more of what Luminus is doing and how their solution shapes up.

_(Disclaimer: I've talked extensively with Kelly and Cyrus from Luminus, and I provided feedback on the article that was published on SDx Central. I have not received any form of compensation, nor do I expect any. This post reflects my own thoughts on what Luminus Networks is doing.)_



[link-1]: http://www.luminusnetworks.com/
[link-2]: https://www.sdxcentral.com/articles/contributed/networking-revolution-network-operations-cyrus-durgin/2016/05/
