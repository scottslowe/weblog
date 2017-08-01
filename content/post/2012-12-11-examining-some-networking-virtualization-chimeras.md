---
author: slowe
categories: Musing
comments: true
date: 2012-12-11T09:00:00Z
slug: examining-some-networking-virtualization-chimeras
tags:
- Cisco
- Interoperability
- LISP
- Networking
- OTV
- Virtualization
- VXLAN
title: Examining Some Networking-Virtualization Chimeras
url: /2012/12/11/examining-some-networking-virtualization-chimeras/
wordpress_id: 3020
---

I like to spend time examining the areas where different groups of technologies intersect. Personally, I find this activity fascinating, and perhaps that's the reason that I find myself pursing knowledge and experience in virtualization, networking, storage, and other areas simultaneously---it's an effort to spend more time "on the border" between various technologies.

One border, in particular, is very interesting to me: the border between virtualization and networking. Time spent thinking about the border between networking and virtualization is what has generated posts like [this one][1], [this one][2], or [this one][3]. Because I'm not a networking expert (yet), most of the stuff I generate is junk, but at least it keeps me entertained---and it occasionally prods the Really Smart Guys (RSGs) to post something far more intelligent than anything I can create.

Anyway, I've been thinking more about some of these networking-virtualization chimeras, and I thought it might be interesting to talk about them, if for no other reason than to encourage the RSGs to correct me and help everyone understand a little better.

&lt;aside&gt;A _chimera_, by the way, was a mythological fire-breathing creature that was part lion, part goat, and part serpent; more generically, the word refers to any sort of organism that has two groups of genetically distinct cells. In layman's terms, it's something that is a mix of two other things.&lt;/aside&gt;

Here are some of the networking-virtualization chimeras I've concocted:

* FabricPath/TRILL on the hypervisor: See [this blog post][1] for more details. It turns out, at least at first glance, that this particular combination doesn't seem to buy us much. The push for large L2 domains that seemed to fuel FabricPath and TRILL now seems to be abating in favor of network overlays and L3 routing.

* MPLS-in-IP on the hypervisor: I also wrote about this strange concoction [here][3]. At first, I thought I was being clever and sidestepping some issues by bringing MPLS support into the hypervisor, but in thinking more about this I realize I'm wrong. Sure, we could encapsulate VM-to-VM traffic into MPLS, then encapsulate MPLS in UDP, but how is that any better than just encapsulating VM-to-VM traffic in VXLAN? It isn't. (Not to mention that Ivan Pepelnjak [set the record straight](http://blog.ioshints.info/2012/07/could-mpls-over-ip-replace-vxlan-or.html).)

* LISP on the hypervisor: I thought this was a really good idea; by enabling LISP on the hypervisor and essentially making the hypervisor an ITR/ETR (see here for more LISP info), inter-DC vMotion becomes a snap. Want to use a completely routed access layer? No problem. Of course, that assumes all your WAN and data center equipment are LISP-capable and enabled/configured for LISP. I'm [not the only one](http://blog.ioshints.info/2011/06/inter-dc-ip-based-vmotion-with-lisp.html) who thought this idea was cool, either. I'm sure there are additional problems/considerations of which I'm not aware, though---networking gurus, want to chime in and educate me on what I'm missing?

* OTV on the hypervisor: This one isn't really very interesting, as it bears great similarity to VXLAN (both OTV and VXLAN, to my knowledge, use very similar frame formats and encapsulation schemes). Is there something else here I'm missing?

* VXLAN on physical switches: This one is interesting, even [necessary](http://blog.ioshints.info/2011/10/vxlan-termination-on-physical-devices.html) according to some experts. Enabling VXLAN VTEP (VXLAN Tunnel End Point) termination on physical switches might also address some of the odd traffic patterns that would result from the use of VXLAN (see [here][4] for a simple example). Arista Networks demonstrated this functionality at VMworld 2012 in San Francisco, so this particular networking-virtualization mashup is probably closer to reality than any of the others.

* OpenFlow on the hypervisor: [Open vSwitch](http://openvswitch.org) (OVS) already supports OpenFlow, so you might say that this mashup already exists. It's not unreasonable to think Nicira might port OVS to VMware vSphere, which would bring an OpenFlow-compatible virtual switch to a much larger installed base. The missing piece is, of course, an OpenFlow controller. While an interesting mental exercise, I'm keenly interested to know what sort of real-world problems this might help solve, and would love to hear from any OpenFlow experts out there what they think.

* Virtualizing physical switches: No, I'm not talking about running switch software on the hypervisor (think Nexus 1000V). Instead, I'm thinking more along the lines of [FlowVisor](https://github.com/OPENNETWORKINGLAB/flowvisor/wiki), which in effect virtualizes a switch's control plane so that multiple "slices" of a switch can be independently controlled by an external OpenFlow controller. If you're familiar with NetApp, think of their "vfiler" construct, or think of the Virtual Device Contexts (VDCs) in a Nexus 7000. However, I'm thinking of something more device-independent than Nexus 7000 VDCs. As more and more switches move to x86 hardware, this seems like it might be something that could really take off. Multi-tenancy support (each "virtual switch instance" being independently managed), traffic isolation, QoS, VLAN isolationlots of possibilities exist here.

Are there any other groupings that are worth exploring or discussing? Any other "you got your virtualization peanut butter in my networking chocolate" combinations that might help address some of the issues in data centers today? Feel free to speak up in the comments below. Courteous comments are invited and encouraged.

[1]: {{< relref "2011-08-08-thinking-out-loud-what-if-vsphere-supported-fabricpath.md" >}}
[2]: {{< relref "2011-08-15-thinking-out-loud-logical-link-multiplexing-sort-of.md" >}}
[3]: {{< relref "2012-06-26-thinking-out-loud-why-not-mpls-in-ip.md" >}}
[4]: {{< relref "2011-12-22-otv-and-vxlan-layer-3-connectivity-compared.md" >}}
