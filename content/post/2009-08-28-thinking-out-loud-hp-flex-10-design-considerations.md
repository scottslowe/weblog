---
author: slowe
categories: Musing
comments: true
date: 2009-08-28T10:41:02Z
slug: thinking-out-loud-hp-flex-10-design-considerations
tags:
- HP
- Networking
- ToL
- Virtualization
- VMware
title: 'Thinking Out Loud: HP Flex-10 Design Considerations'
url: /2009/08/28/thinking-out-loud-hp-flex-10-design-considerations/
wordpress_id: 1571
---

Along with a number of other projects recently, I've also been spending time working with HP Virtual Connect Flex-10. You may have seen these (relatively) recent Flex-10 articles:

[Using VMware ESX Virtual Switch Tagging with HP Virtual Connect][1]  

[Using Multiple VLANs with HP Virtual Connect Flex-10][2]  

[Follow-Up About Multiple VLANs, Virtual Connect, and Flex-10][3]

As I began to work up some documentation for internal use at my employer, I asked myself this question: what are the design considerations for how an architect should configure Flex-10?

Think about it for a moment. In a "traditional" VMware environment, architects will place port groups onto vSwitches (or dvPort groups onto dvSwitches) based on criteria like physical network segregation, number of uplinks, VLAN support, etc. In a Flex-10 environment, those design criteria begin to change:

* The number of uplinks doesn't matter anymore, because bandwidth is controlled in the Flex-10 configuration. You want 1.5Gbps for VMotion? Fine, no problem. You want 500Mbps for the Service Console? Fine, no problem. You want 8Gbps for IP-based storage traffic? Fine, no problem. As long as it all adds up to 10Gbps, architects can subdivide the bandwidth however they desire. So the number of uplinks, from a bandwidth perspective, is no longer applicable.

* Physical network segregation is a non-issue, because all the FlexNICs share the same LOM and will (as far as I know) all share the same uplinks. (In other words, I don't think that LOM1:a can use one uplink while LOM1:b uses a different uplink.) You'll physically distinct NICs in order to handle physically segregated networks. Of course, physically segregated networks will present a bit of challenge for blade environments anyway, but that's beside the point.

* VLAN support is a bit different, too, because of the fact that you can't map overlapping VLANs to FlexNICs on the same LOM. In addition, because of the way VLANs work within a Virtual Connect environment, I don't see VLANs being an applicable design consideration anyway; there's too much flexibility in how VLANs are presented to servers for that to drive how networking should be set up.

So what _are_ the design considerations for Flex-10 in VMware environments, then? What would drive an architect to specify multiple FlexNICs per LOM instead of just lumping everything together in a single 10Gbps pipe? Is bandwidth the only real consideration? I'd love to hear what others think. Let me hear your thoughts in the comments---thanks!

[1]: {{< relref "2009-07-06-using-vmware-esx-virtual-switch-tagging-with-hp-virtual-connect.md" >}}
[2]: {{< relref "2009-07-09-using-multiple-vlans-with-hp-virtual-connect-flex-10.md" >}}
[3]: {{< relref "2009-07-09-follow-up-about-multiple-vlans-virtual-connect-and-flex-10.md" >}}
