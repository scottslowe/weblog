---
author: slowe
categories: Explanation
comments: true
date: 2008-03-05T23:13:37Z
slug: vmotion-and-vlan-security
tags:
- ESX
- Networking
- Security
- Virtualization
- VLAN
- VMotion
- VMware
title: VMotion and VLAN Security
url: /2008/03/05/vmotion-and-vlan-security/
wordpress_id: 651
---

Xensploit, as it's called, is the [recently demonstrated exploit](http://www.eecs.umich.edu/techreports/cse/2007/CSE-TR-539-07.pdf) that allows virtual machines (VMs) that are "in flight" during a live migration (XenMotion in [Citrix XenServer](http://citrix.com/English/ps2/products/product.asp?contentID=683148), VMotion in [VMware ESX Server](http://www.vmware.com/products/vi/esx/)) to be manipulated. If you haven't yet read the PDF that describes Xensploit, I highly encourage that you take a look at it. It's very enlightening as to exactly what _can_ be done to an in-flight VM.

Naturally, the best way to protect against this particular problem is to guard the integrity of the live migration network. For simplicity's sake, I'll refer to this as the VMotion network from this point on, but keep in mind that it is equally applicable to any network connections on any virtualization platform that uses live migration.

The most surefire way to protect the VMotion network is to place it on its own dedicated, physically separate network, using separate physical NICs plugged into separate physical switches that do not possess any connections to production networks. This will ensure that unauthorized access to the VMotion network is prevented, but comes with disadvantages as well: this configuration requires more physical NICs and more physical switches than other configurations.

In implementations with limited numbers of physical NICs, however, this isn't really an option. In these cases, the use of Layer 2 VLANs and  multiple port groups on a single ESX Server vSwitch to allow VMotion traffic to share the same physical NICs and the same physical switches as other traffic is a very common solution. In fact, it's a solution that I've recommended many, many times. But does this configuration provide enough protection for the VMotion network?

The real question is, does a simple Layer 2 VLAN offer enough protection? That question, in turn, spawns other questions: what kinds of attacks are there against Layer 2 VLANs? Is it possible for traffic to hop across VLAN boundaries?

Armed with those questions, I set out to do some research. You can see some links in my del.icio.us bookmarks that pertain to the research I did. Basically, I found that Layer 2 VLAN attacks boil down to two basic types:

1. The first type involves a malicious host pretending to be a switch and forming a 802.1Q VLAN trunk with the real switch, which then passes traffic from all VLANs across to the malicious host.

2. The second type involves double-tagged 802.1Q frames and the native VLAN, whereby traffic can, under specific circumstances, hop from one VLAN to another without any Layer 3 routing involved.

(Network and security gurus out there feel free to elaborate or correct me on this information.)

The best way to address attack vector #1 is to explicitly disable automatic trunk negotiation on all ports that don't need to be trunks. From my research and my (relatively) limited knowledge of Cisco IOS, this command should do it:

	switchport mode access

This explicitly forces the switchport into a state where it will not negotiate an 802.1Q VLAN trunk with another device, hence killing attack vector #1 dead in its tracks. Again, this should only be done on the ports that are _not_ connected to the ESX Servers; otherwise, you're shooting yourself in the foot. Keep in mind that uplinks to other switches, ports going out to IP phones, etc., may also need to be configured as VLAN trunks. Really, the issue is about controlling the creation of unauthorized VLAN trunks in order to control VLAN leakage. One of the CCIEs at the office mentioned a `switchport noneg` command, but I'm not familiar with that one; anyone have more details?

Addressing attack vector #2 is also relatively straightforward. Since the VLAN hopping exploit takes advantage of the nature of the native VLAN (which I've discussed before [here][1]), setting the native VLAN on the trunk ports connecting to the ESX Servers to a VLAN that is not used anywhere else in the network will prevent VLAN hopping. For example:

	switchport trunk native vlan 4094

From my [NIC teaming and VLAN trunking article][2] (one of the most popular articles on the site, by the way), you'll see that I recommended at that time the creation of a VLAN that is used only as the native VLAN for 802.1Q trunks. At that time, I didn't fully understand why; now, after additional research, I understand why I needed to do that and I also recognize that the suggested configuration provides a layer of protection against VLAN hopping attacks.

To summarize:

* To protect against malicious hosts forming unauthorized 802.1Q trunks, disable automatic trunk negotiation and explicitly/manually create trunks. The key here is to ensure that VLANs don't inadvertently "leak" beyond where they should (also see note below about specifying the allowed VLANs).

* To protect against VLAN hopping, create a VLAN that is used only as the native VLAN on the 802.1Q trunks connecting to the ESX Servers. This VLAN _must not_ be used anywhere else in the LAN. Set this VLAN as the native VLAN on the trunks into the ESX Servers.

In addition, my networking mentors also recommended the "switchport trunk allowed vlan" command to specify which VLANs are allowed to cross 802.1Q VLAN trunks. This will help ensure that VMotion traffic is limited to only those switches that absolutely must carry it; again, we're seeking to control VLAN leakage.

With these configurations in place, using a Layer 2 VLAN to carry VMotion traffic on the same physical NICs and same physical switches as other types of traffic is fairly well-protected against malicious interference. While it is not as secure as a physical separate, dedicated network, it is secure enough for most organizations and the reduction in infrastructure needs generally outweighs the risks.

More information and discussion about Xensploit---and protecting against Xensplot---at the following links:

[Keeping Your VMotion Traffic Secure](http://blogs.vmware.com/security/2008/02/keeping-your-vm.html)  

[Two vulnerabilities found in VMware virtualization products](http://www.scmagazineus.com/Two-vulnerabilities-found-in-VMware-virtualization-products/article/107207/)  

['Live' VMs at Risk While in Transit](http://www.darkreading.com/document.asp?doc_id=146647&f_src=darkreading_gnews)  

[News Flash: If You Don't Follow Suggested Security Hardening Guidelines, Bad Things Can Happen...](http://rationalsecurity.typepad.com/blog/2008/02/news-flash-if-y.html)

**UPDATE:** I've updated the wording above to more properly reflect the goal behind the use of the `switchport mode access` command, and when it should be used. Colin, thanks for the feedback and clarification!

[1]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[2]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
