---
author: slowe
categories: Explanation
comments: true
date: 2010-03-05T10:01:51Z
slug: vmware-esx-emc-clariion-arrays-and-multiple-protocols
tags:
- EMC
- FibreChannel
- iSCSI
- Storage
- Virtualization
- VMware
title: VMware ESX, EMC CLARiiON Arrays, and Multiple Protocols
url: /2010/03/05/vmware-esx-emc-clariion-arrays-and-multiple-protocols/
wordpress_id: 1856
---

I was browsing through an EMC technical document titled "EMC CLARiiON Integration with VMware ESX Server" (download it [here](http://www.emc.com/collateral/hardware/white-papers/h1416-emc-clariion-intgtn-vmware-wp.pdf)) a little while ago and I came across a phrase in the document that caught my attention:

>"VMware ESX/ESXi support both Fibre Channel and iSCSI storage. However, VMware and EMC do not support connecting VMware ESX/ESXi servers to CLARiiON Fibre Channel and iSCSI devices on the same array simultaneously."

What? No Fibre Channel and iSCSI from the same array to a VMware ESX/ESXi host simultaneously? That piqued my curiosity, so I contacted a few people within EMC to question the veracity of that statement. It turns out that the answer is more complicated than it might seem at first glance.

For those of you who aren't interested in the deep technical details, here's the short explanation behind this behavior:

* VMware fully supports the use of both Fibre Channel and iSCSI from the same array to the same VMware ESX/ESXi host simultaneously.

* VMware does not support presenting the **_same LUN_** via both protocols concurrently to the same host. (I qualified this directly with VMware.)

* For a Celerra, you can use both Fibre Channel (via the CLARiiON side of the array) and iSCSI (via the Celerra side of the array) simultaneously. This is a fully supported configuration.

* A CLARiiON array can easily present the same LUN via both Fibre Channel and iSCSI, but then VMware wouldn't support it (see earlier bullet).

* With a CLARiiON array, it is possible to present some LUNs via Fibre Channel and some LUNs via iSCSI to the same VMware ESX/ESXi host (i.e., LUN A via Fibre Channel and LUN B via iSCSI), but EMC will only support it if you file an RPQ. Without an RPQ, it's an unsupported configuration. An RPQ, by the way, is a request to qualify a certain configuration for support.

I'm confident that some other array vendors out there will be very quick to jump on this post and harp on this limitation until the cows come home. I would just ask this question: is it really as big of a limitation as it seems? I'll come back to that question in a moment.

With the short explanation in mind, here are the more in-depth details. If you like the longer, more technical explanation, then read on!

From EMC's side, the root of the restriction about using both Fibre Channel and iSCSI devices on the same array simultaneously stems from the interaction of host registration and storage groups.

Host registration is a requirement in the CLARiiON world. In order to present storage to a host from a CLARiiON array, you must first register the host's initiators with the array in Navisphere. Once the host has been registered, then you can proceed with presenting storage to that host. In theory the CLARiiON could operate without registering hosts and initiators, but EMC chose to require registration. EMC made this choice in order to help simplify host management.

Requiring host registration is a bit different than some of other storage arrays on the market. It's not better or worse---just different. (Remember, [pros and cons come from every technology decision][1].)

If you're like me, you're probably wondering at this point how requiring host registration simplifies anything. Instead of having to manage multiple paths, multiple initiators, and individual hosts every time you want to present storage to a host, you only need to register the host---and all of its initiators---and then you can refer to that same object (the host) over and over again as needed. Yes, host registration does mean a bit more work up front, but the idea is that it will save some work down the road. I guess you can think of host registration kind of like defining aliases in your Fibre Channel zoning configuration: it's a bit more work up front, but it simplifies things later down the road. If you didn't create device aliases in your Fibre Channel switch, you'd end up having to re-enter Fibre Channel WWPNs multiple times. You create the aliases so that it's easier later. The same applies to host registration. Again, it's a matter of choices.

One might also say that registration is security measure, albeit a weak measure. Rather than allow just any Fibre Channel-attached or iSCSI-attached host to see storage, the array requires that it _know_ about the host (via host registration) in order to present storage to the host. This provides an additional layer of security to ensure that only authorized hosts are presented storage from the array.

Now you have a fairly decent idea of why host registration is necessary. So how does host registration occur? Host registration can occur either manually or automatically. Starting with version 4.0, both VMware ESX and VMware ESXi will automatically register with a CLARiiON array running any recent version of FLARE (ESX 3i version 3.5 also supports this form of push registration). FLARE release 28 and earlier will show these hosts as "Manually registered, unmanaged"; starting with FLARE 29, these hosts are listed as "Manually registered, managed". In either case, the registration occurs automatically. If the host is Fibre Channel-attached, then the Fibre Channel initiators will be included in the automatic registration. The same goes for iSCSI initiators. Normally, this is a good thing because it saves the administrator the extra steps of registering the host with the storage array. (Also, because VMware ESX/ESXi hosts register automatically, there is no need to install the Navisphere Agent.)

In this case, though, the automatic registration causes a problem. Why? This goes back to the second item I said I needed to discuss: storage groups. Specifically, storage groups have two characteristics that come into play here:

1. First, any given host---not just VMware ESX/ESXi hosts, but all types of hosts---can only be connected to a single storage group at any given time.

2. Second, while the CLARiiON can present Fibre Channel LUNs and iSCSI LUNs simultaneously (including presenting the same LUN via both protocols simultaneously), there is no way within a single storage group to specify which LUNs should be accessed via Fibre Channel and which LUNs should be accessed via iSCSI. This is necessary because VMware won't support accessing the same LUN via both protocols at the same time (see earlier VMware support statement).

Do you see how all the pieces come together? The only way to control which LUNs should be presented via which protocol is to use multiple storage groups---but a host can only be in a single storage group at a time. With only a single host object for any given VMware ESX/ESXi host, that host can only see either Fibre Channel LUNs (by being in a storage group containing Fibre Channel LUNs) **or** iSCSI LUNs (by being in a storage group containing iSCSI LUNs), but not both. Hence, the statement in the CLARiiON document I referenced in the very beginning of this blog post that outlines using _either_ Fibre Channel or iSCSI but not both. This behavior is required to enforce the single-protocol LUN access required by VMware.

As with all things, there is a workaround. Because it is a workaround, that's why the RPQ is necessary to get full support.

To work around this problem, you'll need to ignore the automatic host registration (or disable the automatic host registration) and instead create two manually registered "pseudo-hosts": one with the Fibre Channel initiators and one with the iSCSI initiators. These "pseudo-hosts" will need fake IP addresses (if they both use the same IP address, Navisphere will treat them as the same host, thus defeating the purpose of the workaround). Put the Fibre Channel initiators into the Fibre Channel storage group(s), and put the iSCSI initiators into the iSCSI storage group(s). Each "pseudo-host" will be able to see LUNs presented to that storage group and therefore would see both Fibre Channel and iSCSI LUNs at the same time. And, as required by VMware, any given LUN would be accessed only via Fibre Channel or iSCSI but not both. Remember that you need to file an RPQ in order to get support on this configuration.

For VMware ESX/ESXi 4.0 hosts (and ESX 3i version 3.5 hosts), you can disable automatic registration using the Disk.EnableNaviReg advanced configuration option. Setting this value to 0 disables the automatic registration with Navisphere. (Here are screenshots for [VMware ESX 3i][2] and [VMware ESX/ESXi 4][3].) If you disable the automatic registration, then you only need to manually register the Fibre Channel and iSCSI initiators as separate "pseudo-hosts" and you're ready to go.

Let me reiterate again that if you are presenting iSCSI LUNs via the Celerra and not the CLARiiON, **_none of this applies._** Presenting Fibre Channel LUNs via the CLARiiON and iSCSI LUNs via the Celerra to the same VMware ESX/ESXi host is fine. This workaround that I've described only applies when you want to present some LUNs via Fibre Channel and some LUNs via iSCSI from a CLARiiON to a single VMware ESX/ESXi host.

Earlier you'll recall that I asked this question: is this really a limitation? There are a couple of viewpoints:

* One viewpoint states there is no need for both Fibre Channel and iSCSI connectivity to the same array. Since you already have Fibre Channel connectivity to the array, what's the point in using iSCSI? Conversely, if you already have iSCSI connectivity to an array, why invest in establishing Fibre Channel connectivity? Since you can't use it for failover (that would violate the VMware support position), running another block protocol against the same array and same sets of disks doesn't add a great deal of value.

* A second viewpoint argues that the ability to provide a differentiation of service based on the different performance characteristics of Fibre Channel and iSCSI (and NFS, but we're focusing on block protocols for this discussion) is valuable, and thus the need to be able to easily present LUNs via either protocol from the same array to the same host is a worthwhile function. There are a number of potential use cases here---test/development environments, Tier 2 applications, varying SLAs, etc. This is especially true if you are using different disk pools (fast Fibre Channel drives or EFDs vs. slower SATA drives) on the same array.

I can see both sides of the coin. Personally, I tend to side more with the second viewpoint and would prefer to see the CLARiiON have the ability to easily present Fibre Channel and iSCSI to the same host, especially when multiple disk pools are involved. I think that CLARiiON engineering is now evaluating this possibility; as more information emerges, I'll be sure to keep you posted.

Courteous and professional comments, clarifications, or corrections are always welcome!

[1]: {{< relref "2009-10-23-cutting-yourself-on-the-double-edged-sword.md" >}}
[2]: /public/img/navireg-esx3.png
[3]: /public/img/navireg-esx4.png
