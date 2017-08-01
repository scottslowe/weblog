---
author: slowe
categories: Explanation
comments: true
date: 2009-07-09T11:56:26Z
slug: follow-up-about-multiple-vlans-virtual-connect-and-flex-10
tags:
- Hardware
- HP
- Networking
- Virtualization
title: Follow-Up About Multiple VLANs, Virtual Connect, and Flex-10
url: /2009/07/09/follow-up-about-multiple-vlans-virtual-connect-and-flex-10/
wordpress_id: 1459
---

I wanted to post a quick follow-up on the previous two articles that I published regarding using VLANs, Virtual Switch Tagging, HP Virtual Connect, and Flex-10:

[Using VMware ESX Virtual Switch Tagging with HP Virtual Connect][1]

[Using Multiple VLANs with HP Virtual Connect Flex-10][2]

One thing that I wanted to really clarify was that you _can_ present multiple VLANs to FlexNICs on the same LOM, but you can't present the _same set of VLANs_ to FlexNICs on the same LOM. I'm not sure that I made that clear enough in my other post. So, if you have VLANs 100, 101, 102, 103, and 200, you can present any number or combination of those to all the FlexNICs on a single LOM---but it must be in a non-overlapping configuration, so that the same VLAN isn't presented to multiple FlexNICs on the same LOM. I plan on posting some sample configurations, with graphics, that should clarify things even more.

The other thing I wanted to clarify was _why_ I posted an article about presenting multiple VLANs to FlexNICs on the same LOM. Usually, this topic comes up in the form of a question (and I've had several readers e-mail me about this) like this: "You state that you can't present the same set of VLANs to multiple FlexNICs on the same LOM. Why in the world would you want to do that?"

That's a good question! In a Flex-10 environment, you normally _wouldn't_ want to do that, and not just for the reason that it doesn't work (although having a configuration that works is usually quite beneficial). Consider the value proposition behind Flex-10: it provides four logical NICs (the FlexNICs), each of whose bandwidth can be adjusted as needed, up to 10Gbps, based on the traffic requirements. Now consider the reason why administrators normally use multiple NICs: more bandwidth. Considering that you can allocate bandwidth easily with a FlexNIC, there's no need to use multiple FlexNICs on the same LOM to handle the same VLANs. A single FlexNIC can handle multiple VLANs just fine because you can allocate more bandwidth to that FlexNIC easily. And since presenting multiple VLANs to FlexNICs on different LOMs works just fine, you can use one FlexNIC from each LOM to gain redundancy. All the reasons for wanting to use multiple NICs are addressed by using only two FlexNICs, one from each LOM, and presenting as many VLANs as you like to those two FlexNICs.

However, having said all that, it is the default configuration in many VMware environments to present VLAN trunks to all ports on all ESX/ESXi hosts (i.e., to present the same set of VLANs to all ports). In a Flex-10 environment, you'll have to break out of that line of thinking. Hence, why I posted the information that I posted, so that VMware administrators would realize they can't follow the same configuration guidelines in the Flex-10 environment as they would follow in a more "traditional" networking environment. Just as virtualization with VMware requires server admins to approach things differently, the functionality offered by Virtual Connect also requires server admins to think a bit differently about how network connectivity is presented to VMware ESX/ESXi hosts.

Have more questions, or need additional clarification? Speak up in the comments. Thanks for reading!

[1]: {{< relref "2009-07-06-using-vmware-esx-virtual-switch-tagging-with-hp-virtual-connect.md" >}}
[2]: {{< relref "2009-07-09-using-multiple-vlans-with-hp-virtual-connect-flex-10.md" >}}
