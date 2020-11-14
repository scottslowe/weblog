---
author: slowe
categories: Tutorial
comments: true
date: 2011-06-22T12:53:01Z
slug: rdp-printer-redirection-with-vmware-view-on-a-mac
tags:
- macOS
- VDI
- Virtualization
- VMware
title: RDP Printer Redirection with VMware View on a Mac
url: /2011/06/22/rdp-printer-redirection-with-vmware-view-on-a-mac/
wordpress_id: 2320
---

Last July I wrote an article about editing the `vmware-view.rdp` file inside the VMware View Open Client for Mac OS X in order [to customize the RDP settings][1]. It was one of those articles that I thought would probably appeal to only a very small group but would otherwise go mostly unnoticed.

As it turns out, one reader named Patrick Fergus picked up on that article and started experimenting with it to see if he could enable printer redirection using a similar technique. I'm going to share his findings here, but with one disclaimer: I haven't actually tested this process myself.

To make printer redirection work on a Mac using the VMware View client, two things need to happen:

1. First, you need to install the HP LaserJet 4350 PS driver (not the universal driver) on the Windows VMs being used by VMware View.

2. Second, you need to edit the `vmware-view.rdp` file to enable printer redirection.

I'll leave the first task as an exercise for the readers, but for the second task I'll provide a bit more detail. To enable printer redirection, edit the `vmware-view.rdp` file and add these lines:

```xml
<key>PrinterRedirection</key>  
<true/>  
<key>RedirectPrinter</key>  
<string>all</string>
```

Once you have the LaserJet 4350 PS printer driver installed on the Windows VM and have this text in the `vmware-view.rdp` file, printer redirection from the Mac VMware View client should work as expected.

Keep in mind the caveat that I pointed out in the original article: changing the `vmware-view.rdp` file will affect **all** connections using the VMware View client, not just one particular connection. It would be great to be able to enable/disable this sort of functionality on a per-connection/per-server basis.

Great work Patrick, and thanks for sharing the information with me. As always, comments are invited!

[1]: {{< relref "2010-07-21-changing-rdp-settings-in-vmware-view-open-client-for-mac.md" >}}
