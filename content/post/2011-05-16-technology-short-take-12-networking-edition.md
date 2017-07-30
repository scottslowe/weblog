---
author: slowe
categories: Information
comments: true
date: 2011-05-16T09:00:00Z
slug: technology-short-take-12-networking-edition
tags:
- Cisco
- FCoE
- Networking
- UCS
- vCloud
- OTV
- OpenFlow
title: 'Technology Short Take #12: Networking Edition'
url: /2011/05/16/technology-short-take-12-networking-edition/
wordpress_id: 2299
---

Now that I've published the Storage Edition of Technology Short Take #12, it's time for the Networking Edition. Enjoy, and I hope you find something useful!

* Ron Fuller's ongoing deep dive series on OTV (Overlay Transport Virtualization) has been great for me. I knew about the basics of OTV, but Ron's articles really gave me a better understanding of the technology. Check out the first three articles here: [part 1](http://ccie5851.blogspot.com/2011/02/otv-deep-dive-part-one.html), [part 2](http://ccie5851.blogspot.com/2011/02/otv-deep-dive-part-two.html), and [part 3](http://ccie5851.blogspot.com/2011/03/otv-deep-dive-part-3.html).

* Similarly, Joe Onisick's two-part (so far) series on inter-fabric traffic on Cisco UCS is very helpful and informative as well. There are definitely some design considerations that come about from deploying VMware vSphere on Cisco UCS. Have a look at Joe's articles on his site ([Part 1](http://www.definethecloud.net/inter-fabric-traffic-in-ucs) and [Part 2](http://www.definethecloud.net/inter-fabric-traffic-in-ucspart-ii)).

* Kurt Bales' [article on innovation vs. standardization](http://www.network-janitor.net/2011/02/proprietary-cometh-before-the-standard/) is a great read. The key, in my mind, is innovating (releasing "non-standard" stuff) while also working with the broader community to help encourage standardization around that innovation.

* Here's another great multi-part series, this time from Brian Feeny on NX-OS ([part 1 here](http://www.feeny.org/?p=794), and [part 2 here](http://www.feeny.org/?p=827)). Brian exposes some pretty interesting stuff in the NX-OS kickstart and system image.

* I've discussed LISP a little bit here and there, but Greg Ferro reminds us [that LISP isn't a "done deal."](http://etherealmind.com/path-lisp-certain-rfc-6115/)

* J Metz wrote a good article on the interaction (or lack thereof, depending on how you look at it) between [FCoE and TRILL](http://blogs.cisco.com/datacenter/understanding-fcoe-and-trill-the-easy-way/).

* For a non-networking geek like me, some great resources to become more familiar with TRILL might include [this comparison of 802.1aq and TRILL](http://blog.ioshints.info/2010/08/trill-and-8021aq-are-like-apples-and.html), this [explanation from RFC 5556](http://packetattack.org/2010/10/06/traveling-east-west-might-get-a-little-easier-highlights-from-the-trill-rfc5556/), this [discussion of TRILL-STP integration](http://blog.ioshints.info/2011/03/trillfabric-path-stp-integration.html), or this [explanation using north-south/east-west terminology](http://etherealmind.com/layer-2-multipath-east-west-bandwidth-switch-designs/). Brad Hedlund's [TRILL write-up](http://bradhedlund.com/2010/05/07/setting-the-stage-for-trill/) from a year ago is also helpful, in my opinion. All of these are great resources, in my mind.

* And as if understanding TRILL, or the differences between TRILL and FabricPath weren't enough (see [this discussion](http://www.networkworld.com/community/blog/full-tilt-boogie-networking-ciscos-fabricpat) by Ron Fuller on the topic), then we have 802.1aq Shortest Path Bridging (SPB) thrown in for good measure, too. If it's hard for networking experts to keep up with all these developments, think about the non-networking folks like me!

* Ivan Pepelnjak's [examination of vCDNI-based private networks via Wireshark traces](http://blog.ioshints.info/2011/04/vcloud-director-networking.html) exposes some notable scalability limitations. It makes me wonder, as Ivan does, why VMware chose to use this method versus something more widely used and well-proven, [like MPLS?](http://blog.ioshints.info/2011/04/vcloud-architects-ever-heard-of-mpls.html) And isn't there an existing standard for MAC-in-MAC encapsulation? Why didn't VMware use that existing standard? Perhaps it goes back to innovation vs. standardization again?

* If you're interested in more details on vCDNI networks, check out [this post](http://www.borgcube.com/blogs/2011/03/vcd-network-isolation-vcdni/) by Kamau Wanguhu.

* Omar Sultan of Cisco has a quick post on OpenFlow and Cisco's participation [here](http://blogs.cisco.com/datacenter/openflow-pulling-networking-into-the-application-stack/).

* Jake Howering of Cisco (nice guy, met him a few times) has [a write-up](http://blogs.cisco.com/datacenter/dci-use-case-capacity-expansion/) on an interesting combination of technologies: ACE (load balancing) plus OTV (data center interconnect), with a small dash of VMware vCenter API integration.

I think that's going to do it for this Networking Edition of Technology Short Take #12. I'd love to hear your thoughts, suggestions, or corrections about anything I've mentioned here, so feel free to join the discussion in the comments. Thanks for reading!
