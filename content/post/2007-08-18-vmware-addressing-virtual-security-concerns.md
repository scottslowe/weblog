---
author: slowe
categories: News
comments: true
date: 2007-08-18T15:40:49Z
slug: vmware-addressing-virtual-security-concerns
tags:
- Networking
- Security
- Virtualization
- VMware
title: VMware Addressing Virtual Security Concerns
url: /2007/08/18/vmware-addressing-virtual-security-concerns/
wordpress_id: 505
---

One of the recurring complaints with highly virtualized environments is security. I've written about [virtual security concerns][1] before, and the resulting discussion (see the comments to that article) brought up some additional viewpoints as well. My opinion is that many of the "concerns" about highly virtualized environments are only concerns if you aren't properly designing and implementing your virtualization environment correctly. (Of course, that statement is true for many, many technologies, not just virtualization. The same could be said for such technologies as firewalls, Active Directory, or handhelds/PDAs.)

There is one area, however, where I do agree with some of the complaints, and that is visibility into the virtualized networking environment. Without that, there is no way to detect (and/or mitigate) guest-to-guest attacks on the same host. I believe that VMware's rumored move to [allow Cisco switches on ESX Server][2] is one step to help mitigate this weakness. After all, if we've got native IOS running on a switch on ESX, that means (presumably) that we can mirror traffic to another virtual switch port for use by an IDS/IPS, most likely running in another VM.

With VMware's IPO now behind them, it looks like they are also taking some additional steps to help mitigate some of the security concerns that keep getting brought back to the surface again. As reported by [Tarry Singh](http://tarrysingh.blogspot.com/2007/08/vmware-acquires-security-firm-determina.html), [Alessandro Perilli](http://www.virtualization.info/2007/08/vmware-acquires-determina.html), [SearchSecurity.com](http://searchsecurity.techtarget.com/originalContent/0,289142,sid14_gci1268544,00.html), and [David Marshall](http://vmblog.com/archive/2007/08/18/vmware-quietly-acquires-determina-and-its-host-intrusion-prevention.aspx), VMware has acquired security firm [Determina](http://www.determina.com/), whose [LiveShield](http://www.determina.com/products/liveshield.asp) and [Memory Firewall](http://www.determina.com/products/memory_firewall.asp) products are designed to provide "inline patching" and code injection/buffer overflow protection, respectively.

As pointed out in almost all of the articles referenced above, it is anticipated that VMware will incorporate these technologies into their core products. For example, incorporating the Memory Firewall functionality into the hypervisor helps to ensure the security and integrity of the hypervisor itself, and extending this functionality automatically to guest OS instances thus protects them as well. Likewise, incorporating the LiveShield functionality allows VMware to protect "vulnerability protection" at the virtual networking layer by blocking the malicious attacks that take advantage of exploits which have not yet been patched by the OS vendors.

If VMware does indeed provide the ability for third-party vendors to provide virtual switches for ESX Server and incorporates the Determina technologies into vmKernel, I believe they will have addressed almost all of the security concerns that many people have with highly virtualized environments. Combine this with the [patching solution that will supposedly be included in ESX Server 3.1](http://www.virtualization.info/2007/08/vmware-partners-with-shavlik-for-new.html) (as reported on virtualization.info), and organizations have some compelling security reasons why they **should** virtualize instead of why they should not virtualize.

[1]: {{< relref "2007-03-14-virtual-security-concerns.md" >}}
[2]: {{< relref "2007-07-29-cisco-switches-on-vmware.md" >}}
