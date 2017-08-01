---
author: slowe
categories: Explanation
comments: true
date: 2008-06-24T13:00:57Z
slug: vlans-on-esx-with-nortel-switches
tags:
- ESX
- Networking
- Virtualization
- VLAN
- VMware
title: VLANs on ESX with Nortel Switches
url: /2008/06/24/vlans-on-esx-with-nortel-switches/
wordpress_id: 747
---

I ran into a recent issue with a customer who was having problems getting VLANs to work as expected with ESX. The basic scenario was that ESX would refuse to work properly with a VLAN that was marked as the native (or untagged) VLAN. This was causing no end of grief for this customer.

I've discussed VLANs extensively---first with [this blog post][1], then again [here][2], and again in [this SearchVMware.com tip][3]---so I was confident that I could help the customer resolve this issue. Granted, the customer was using Nortel switches, with which I am completely unfamiliar, but a switch is a switch, right?

Not quite. While the configuration seemed correct in all ways, it turns out there is a checkbox somewhere labeled "untag-default-vlan". If this box is not checked, then the default VLAN gets tagged. Since ESX wasn't configured with a VLAN tag, then it doesn't see the network traffic. Once that box gets checked, then the default (or native) VLAN doesn't get tagged and will be properly recognized by an ESX port group without a VLAN tag configured.

So, if you're using Nortel switches and having problems with VLANs, double-check this setting.

[1]: {{< relref "2006-12-04-esx-server-nic-teaming-and-vlan-trunking.md" >}}
[2]: {{< relref "2007-11-13-esx-server-and-the-native-vlan.md" >}}
[3]: {{< relref "2007-12-07-new-vlan-article-at-searchvmwarecom.md" >}}
