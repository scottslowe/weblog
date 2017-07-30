---
author: slowe
categories: Review
comments: false
date: 2006-02-04T22:07:32Z
slug: brief-thoughts-on-virtual-server-2005
tags:
- Microsoft
- Networking
- Virtualization
- VMware
title: Brief Thoughts on Virtual Server 2005
url: /2006/02/04/brief-thoughts-on-virtual-server-2005/
wordpress_id: 173
---

I just installed Virtual Server 2005 for a customer yesterday, and I thought I'd just briefly discuss my impressions of the product. Keep in mind that these are _subjective_, not objective, impressions, and---as always---your mileage may vary.

Here are my thoughts regarding Virtual Server 2005, in no particular order:

* First of all, I don't like web-based interfaces. I just don't. I'd rather have a traditional application any day of the week.

* If you absolutely _must_ use a web-based interface, at least make it browser- and platform-independent. The Virtual Server 2005 interface uses ActiveX controls, which means you must use Internet Explorer on Windows.

* I didn't like how Virtual Server 2005 didn't have the ability to "dynamically" capture media like a floppy drive or a CD-ROM. Instead, you had to manually capture the media, and that media was then unusable by the host system.

* Did I mention I don't like web-based interfaces?

* The whole scenario regarding in which security context the web-based interface should run (i.e., within the security context of the authenticating users, or within the security context of the local system account) seemed confusing and overly complicated to me. Perhaps I'm just too simplistic, but it seemed like a good idea to me to make the application run within the context of the authenticating users (the local system account is often considered too powerful), but I could never make it work. I had to uninstall the application and reinstall again (using the security context of the local system account) in order for it to work.

* Of course, there's the whole deal about what operating systems are supported by Virtual Server 2005 (i.e., only other versions of Windows).

* It uses a web-based interface.

* I find it interesting that the VMRC (Virtual Machine Remote Control) uses TCP port 5900. Can anyone say, "VNC"? (Anyone been able to confirm this with a network sniffer?)

* Last, but not least, the web-based interface.

OK, so perhaps I'm a bit biased toward [VMware](http://www.vmware.com/) (no, I don't work for them). I like to try to keep an open mind when it comes to technology issues. So, in that vein, can anyone out there enlighten me to as to what advantages Virtual Server 2005 has over VMware GSX Server (or even ESX Server)?
