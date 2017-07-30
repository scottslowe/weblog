---
author: slowe
categories: Musing
comments: true
date: 2013-06-20T09:00:00Z
slug: thinking-out-loud-the-future-of-vlans
tags:
- Networking
- Virtualization
- VLAN
- SDN
- ToL
title: 'Thinking Out Loud: The Future of VLANs'
url: /2013/06/20/thinking-out-loud-the-future-of-vlans/
wordpress_id: 3219
---

It's interesting to me how much the idea of a VLAN has invaded the consciousness of data center IT professionals. Data center folks primarily tasked with managing compute workloads are nearly as familiar with VLANs as their colleagues primarily tasked with managing network connectivity. However, as networking undergoes a transformation at the hands of SDN, NFV, and network virtualization, what will happen to the VLAN?

The ubiquity of the VLAN is due, I think, to the fact that it serves as a reasonable "common ground" for both compute-focused and networking-focused professionals. Need a logical container for new workloads? We can use a VLAN for that. VMware is partially to blame for this---vSphere (and its predecessors) made it incredibly easy to use VLANs as a way of logically "partitioning" compute workloads on the same host. (To be fair, it was really the only tool available to accomplish the task at the time.)

Normally, finding a "common ground" is a good thinguntil that common ground starts to get pushed beyond where it was intended to be used. I think this is where VLANs are now---getting pushed beyond where they were intended to be used, and that strain is the source of some discord between the compute-centric teams and the networking-centric teams:

* The compute-centric teams need a logical container by which they can group workloads that might run potentially anywhere in the data center.

* The networking-centric teams, though, recognize the challenges inherent in taking a VLAN (i.e., a single broadcast domain) and stretching it across a bunch of different top-of-rack (ToR) switches so that it's available to any compute host. (The irony here is that we're using a tool designed to breakdown broadcast domains---VLANs---and building large broadcast domains with them.)

What's needed here is a separation, or layering, of functions. The compute-centric world needs some sort of logical identifier that can be used to group or identify traffic. However, this logical identifier needs to be separate and distinct from the identifier/tag/mark that the networking-centric folks need to use to build scalable networks with reasonably-sized broadcast domains. This is, in my view, one of the core functions of a network encapsulation protocol like STT, VXLAN, or NVGRE _when used in the context of a network virtualization solution._ Note that qualification---a network encapsulation protocol alone is like the suspension on a car: useful, but only in the context of the complete package. When a network encapsulation protocol is used in the context of the complete package (a network virtualization solution), the network encapsulation protocol can supply the logical identifier the compute-centric teams need (this would be VXLAN's 24-bit VNI, or STT's 64-bit Context ID) while simultaneously allowing the network-centric teams to use VLANs as they see fit for the best performance and resiliency of the network.

&lt;aside&gt;By the way, while using a network encapsulation protocol in the context of a network virtualization solution provides the decoupling that is so important to innovation, it's important to note that decoupling does not equal loss of visibility. But that's a topic for another post...&lt;/aside&gt;

The end result is that compute-centric teams can create logical groupings that are not dependent on the configuration of the underlying network. Need a new logical grouping for some new line-of-business application? No problem, create it and start turning up workloads. Meanwhile, the networking-centric teams are free to design the network for optimal performance, resiliency, and cost-effectiveness without having to take the compute-centric team's logical groups into consideration. Need to use a routed L3 architecture between pods/racks/ToR switches using VLANs? No problem---build it the way it needs to be built, and the network virtualization solution will handle creating the compute-centric logical grouping.

At least, that's my thought. All my Thinking Out Loud posts are just that---me thinking out loud, providing a springboard to more conversation. What do you think? Am I mistaken? Feel free to speak up in the comments below. Courteous comments (with vendor disclosures where applicable, please) are always welcome.
