---
author: slowe
categories: Musing
comments: true
date: 2013-08-30T11:13:25Z
slug: thinking-out-loud-diy-network-virtualization
tags:
- IPSec
- Linux
- Networking
- Virtualization
- VPN
- OVS
- OSS
- ToL
title: 'Thinking Out Loud: DIY Network Virtualization?'
url: /2013/08/30/thinking-out-loud-diy-network-virtualization/
wordpress_id: 3262
---

Following on the heels of this week's VMware NSX announcement at VMworld 2013, I had someone contact me about a statement this person had heard. The statement claimed NSX was nothing more than a collection of tools, and that it was possible to get the equivalent of NSX using completely free (as in speech and as in beer) open source tools---specifically, [IPTables](http://www.netfilter.org), [StrongSwan](https://www.strongswan.org), [OpenVPN](http://openvpn.net/index.php/open-source.html), and [Open vSwitch](http://openvswitch.org) (OVS). Basically, the statement was that it was possible to create do-it-yourself network virtualization.

That got me thinking: is it true? After considering it for a while, I'd say it's probably possible; I'm not enough of an expert with these specific tools to say it can't be done. I will say that it would likely be difficult, beyond the reach of most organizations and individuals, and would still suffer from a number of operational drawbacks. Here's why.

What are the core components of a network virtualization solution? In my view, there are at least three core components any network virtualization solution needs:

1. Logically centralized knowledge of the network topology; this component should provide programmatic access to this information

2. Programmable edge virtual switches in the hypervisor

3. An encapsulation protocol to isolate logical networks from the physical network

Let's compare this list with the DIY solution:

1. I don't see any component that is capable of building and/or maintaining knowledge of the network topology, and certainly no way to programmatically access this information. This has some pretty serious implications, which I'll describe below.

2. OVS fills the need for a programmable edge virtual switch quite nicely, considering that it was expressly designed for this purpose (and is itself leveraged by NSX).

3. You could potentially leverage either StrongSwan or OpenVPN as an encapsulation protocol. Both of these solutions use encryption, so you'd have to accept the computational overhead of using encryption within your data center for hypervisor-to-hypervisor connectivity, but OK---I suppose these count. Neither of these solutions provide any sort of way to distinguish or mark traffic inside the tunnel, which also has some implications we need to explore.

OK, so the DIY solution is missing a couple of key components. What implications does this have?

* Without any centralized knowledge of the network topology, there is nothing to handle programming OVS. Therefore, every single change must be manually performed. Provisioning a new VM? You must manually configure OVS, OpenVPN, StrongSwan, and possibly IPTables to handle connectivity to and from that VM. Moving a VM from one host to another? That requires manual reconfiguration. Live migration? That will require manual reconfiguration. Terminating a VM? That will require manual reconfiguration.

* Without programmatic access to the solution, it can't be integrated into any other sort of management framework. Want to use it with OpenStack? CloudStack? It's probably not going to work. Want to use it with a custom CMP you've built? It _might_ work, but only after extensive integration work (and a lot of scripts).

* It's my understanding that both StrongSwan and OpenVPN will leverage standard IP routing technologies to direct traffic appropriately through the tunnels. What happens when you have multiple logical networks with overlapping IP address space? How will StrongSwan and/or OpenVPN respond? Because neither StrongSwan nor OpenVPN have any way of identifying or marking traffic inside the tunnel (think of VXLAN's 24-bit VNID or STT's 64-bit Context ID), how do we distinguish one tenant's traffic from another tenant's traffic? Can we even support multi-tenancy? Do we have to fall back to using VLANs?

* Do you really want to incur the computational overhead of using encryption for all intra-DC traffic?

Of course, this list doesn't even begin to address other operational concerns: multiple hypervisor support, support for multiple operating systems (or even multiple Linux distributions), support for physical workloads, physical top-of-rack (ToR) switch integration, high availability for various components, and the supportability of the overall solution.

As you can see, there are some pretty significant operational concerns there. Manual reconfiguration for just about any VM-related task? That doesn't sound like a very good approach. Sure, it might be _technically_ feasible to build your own network virtualization solution, but what benefits does it bring to the business?

Granted, I'm not an expert with some of the open source tools mentioned, so I could be wrong. If you _are_ an expert with one of these tools, and I have misrepresented the functionality the tool is capable of providing, please speak up in the comments below. Likewise, if you feel I'm incorrect in any of my conclusions, I'd love to hear your feedback. Courteous comments are always welcome!
