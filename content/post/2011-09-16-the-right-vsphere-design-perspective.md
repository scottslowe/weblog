---
author: slowe
categories: Musing
comments: true
date: 2011-09-16T11:09:35Z
slug: the-right-vsphere-design-perspective
tags:
- VDI
- Virtualization
- VMware
- vSphere
- Cisco
- UCS
- HP
- Hardware
title: The Right vSphere Design Perspective
url: /2011/09/16/the-right-vsphere-design-perspective/
wordpress_id: 2421
---

I was on a conference call the other day where the topic of the call was [VMware View 5.0](http://www.vmware.com/products/view/overview.html). If you attended VMworld---or even if you didn't, it's been kind of hard to miss---you know already that VMware made significant improvements to PCoIP in View 5. For one reason or another, the topic of [the Teradici PCoIP offload card](http://www.teradici.com/pcoip/pcoip-products/pcoip-server-offload-card.php) came up on the call, and other participants in the call were immediately asking about the availability of the card in mezzanine card format for blade server implementations. (Continue reading, and at the end I'll tell you the answer I received to that question.)

There are lots of blade server implementations on the market; I've had direct hands-on experience with two of the leading implementations, [Cisco UCS](http://www.cisco.com/en/US/netsol/ns944/index.html) and [HP BladeSystem C-Class](http://h18000.www1.hp.com/products/blades/bladesystem/index.html). Both are excellent products, and both products have strengths and weaknesses. However, the one (relative) weakness they both share is a lack of expansion options. The HP BladeSystem blades are a bit better here, as they have onboard NICs (10Gb as well as 1Gb) without using an expansion slot. Either way, though, let's face it---mezzanine card slots in a blade environment aren't exactly plentiful. Now you want to go and use one of these slots for an offload card? This is especially true with Cisco UCS, where the half-width B series blades **require** a mezzanine card in order to have network connectivity.

In my mind, this really underscores the need to view vSphere designs---including designs that incorporate "upper layer" products like View, vCloud Director, and/or Site Recovery Manager---with a broad lens. Even in a dedicated VDI environment, where the ESXi hosts are used _only_ to host virtual desktops, is the trade-off between network connectivity and offloaded PCoIP processing worth it? If adding a PCoIP offload card means you have to move from a half-width/half-height blade to a full-width/full-height blade (thus cutting your compute density in half), was it really worth it? Was it worth the extra rack space, extra power, extra network drops, and extra expense? This is why, as vSphere architects, we sometimes have to "take a step back" to look at things in a broader perspective. Otherwise, you could find yourself adding some piece of technology to your design and crippling the overall design.

&lt;aside&gt;This is not, by the way, a rant against the PCoIP offload card---I can definitely see some value in it for dedicated VDI environments. The offload card just happened to be the catalyst that triggered this post.&lt;/aside&gt;

The answer, by the way, to the availability of the PCoIP offload card in mezzanine card format is "It's up to the OEM." Not much of an answer, I know, but the only answer that's available right now.

If you have additional thoughts you'd like to share, I encourage you to add your thoughts in the comments below.
