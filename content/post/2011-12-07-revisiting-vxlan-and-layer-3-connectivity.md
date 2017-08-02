---
author: slowe
categories: Explanation
comments: true
date: 2011-12-07T23:30:37Z
slug: revisiting-vxlan-and-layer-3-connectivity
tags:
- Networking
- OTV
- Virtualization
- VXLAN
title: Revisiting VXLAN and Layer 3 Connectivity
url: /2011/12/07/revisiting-vxlan-and-layer-3-connectivity/
wordpress_id: 2496
---

In my [earlier post on VXLAN and Layer 3 connectivity][1], I had a fatal flaw in my thinking and in my diagrams that was corrected for me in the comments to that post. In this post, I want to revisit the idea of Layer 3 connectivity with VXLAN and include the corrected information (and new diagrams).

The "fatal flaw" was that I was working under the impression that we'd have to change network address translation (NAT) mappings on the vShield Edge (VSE) instance that was handling NAT for a particular VXLAN segment. As a result of this incorrect thinking, I stated that VXLAN broke Layer 3 connectivity. As it turns out, I was wrong.

Instead---and this makes perfect sense now that my flawed thinking was pointed out---the VSE instance continues to serve as the default Layer 3 gateway for the workload(s) inside the VXLAN segment.

Consider this diagram, which shows how a workload external to a VXLAN segment communicates with a workload inside a VXLAN segment:

![](/public/img/fixed-l3-premig.png)

Note that in this diagram, the Linux workload outside the VXLAN segment communicates via the VSE instance handling NAT for that particular VXLAN segment. The VSE instance (VSE 1) passes that communication to the internal workload, and the return traffic follows the same path. Layer 3 connectivity outside of the VXLAN segment is handled via traditional/normal Layer 2/3 methods.

Now consider this diagram, which shows the same communication, but after the Windows-based workload inside the VXLAN segment has now migrated to a different location:

![](/public/img/fixed-l3-postmig.png)

Note that even though the Windows-based workload inside the VXLAN segment now resides on a completely separate VTEP (ESXi 2, in this case), the traffic from the Linux-based workload outside the VXLAN segment continues to move through VSE 1. That's because VSE 1 is still the Layer 3 default gateway for the IP subnet inside the VXLAN segment. Therefore---and this is where I was wrong earlier---Layer 3 connectivity is **not** broken, but it does have to "horseshoe" across to the other data center and then back again, as illustrated above. This is the classic traffic pattern that we see with other overlay technologies, like OTV.

For me, while this addresses Layer 3 connectivity after a migration with VXLAN, it does bring up other questions:

* How does one provide redundancy at the VSE level? Is there VRRP support in VSE, or an equivalent function?

* Because Layer 3 connectivity is maintained, what now is the role of OTV? Is OTV relegated to handling Layer 2 extensions only for non-virtualized workloads?

* How do we now propose to handle the "horseshoe" routing issue? It would seem to me that the _only_ way to address this would be to port support for LISP (or an equivalent protocol) into VSE.

Feel free to post any questions, thoughts, or corrections in the comments below. Thanks!

[1]: {{< relref "2011-11-30-vxlan-and-layer-3-connectivity.md" >}}
