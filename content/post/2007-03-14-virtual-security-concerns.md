---
author: slowe
categories: Musing
comments: true
date: 2007-03-14T14:18:42Z
slug: virtual-security-concerns
tags:
- ESX
- Networking
- Security
- Virtualization
- VMware
title: Virtual Security Concerns
url: /2007/03/14/virtual-security-concerns/
wordpress_id: 428
---

About a year ago, I [wrote briefly][1] about Reflex VSA, a security appliance designed to operate in the virtual environment to provide additional security functionality to the virtual networking environment. Within the last few days, another security vendor, [BlueLane](http://www.bluelane.com/), has joined the effort to provide additional security by releasing their [VirtualShield](http://www.bluelane.com/products/virtualshield/) product.

Generally speaking, anything that adds security to the infrastructure---virtual or physical---is usually a good thing, so I'm excited to see more vendors creating security solutions that are aware of virtualization solutions. What I'm not so keen to see, though, is the trend among security vendors (and some analysts) that the addition of server virtualization completely changes the security picture.

I disagree with that statement. Does the addition of server virtualization technologies, such as [VMware ESX Server](http://www.vmware.com/products/vi/esx/), introduce some new security challenges? Sure. The addition of _any_ new technology creates new security challenges. Consider the explosion of wireless networks and VPNs and the security challenges created by the widespread adoption of these technologies. Server virtualization is no different. But does server virtualization completely change the security landscape of your network? Personally, I don't think so.

That's not a view that is particularly shared, especially among the security vendors themselves, who stand to benefit from increased paranoia about the security implications of server virtualization. This [Dark Reading article](http://www.darkreading.com/document.asp?doc_id=117908) implies that the increased complexity that is inherent in most (if not all) server virtualization implementations breeds additional security concerns. Similarly, [this related article](http://www.darkreading.com/document.asp?doc_id=119187&WT.svl=news1_1) states:

>The Gartner report says virtual machines may be convenient, but they also bring with them "embedded vulnerabilities and require special consideration for patching and updates." Gartner recommends building security into VM implementations, and watching out for the common security "holes" in VM environments...

"Special consideration for patching and updates"? Huh? How is patching a virtual instance of [Windows Server 2003](http://www.microsoft.com/windowsserver/default.mspx) any different from patching a physical instance? Administrators will still need to maintain virtual instances just like they maintain physical instances---both will need to be patched, reviewed for insecure configuration, scanned for malicious software, etc., generally using the exact same processes in both cases.

I will agree that the limited view into the virtual network switches (and inter-VM/intra-host traffic) _is_ a security concern, but this isn't anything that a quick installation of [Snort](http://www.snort.org/) (or any other intrusion detection/prevention system) can't fix. Likewise, the addition of a new quasi-OS (in the form of the host software, such as ESX Server) does introduce some additional security concerns. It just doesn't change the security landscape in some sort of basic, fundamental way. At least, I don't think so.

Feel free to disagree with me in the comments---just be sure to state your reasons why.

[1]: {{< relref "2006-03-21-virtual-security.md" >}}
