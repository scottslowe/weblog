---
author: slowe
categories: Information
comments: true
date: 2007-09-06T15:28:44Z
slug: solaris-10-update-4-on-esx-server-2
tags:
- ESX
- Solaris
- UNIX
- Virtualization
- VMware
title: Solaris 10 Update 4 on ESX Server
url: /2007/09/06/solaris-10-update-4-on-esx-server-2/
wordpress_id: 526
---

I've been looking forward to [Solaris 10 8/07 (Update 4)](http://www.sun.com/software/solaris/) for a while now, eager to see what new technologies and features have been baked into the stable Solaris 10 code base. I won't go into all the new bells and whistles, partly because I'm just not knowledgeable enough about them to them justice, and partly because it's been [done better elsewhere](http://www.c0t0d0s0.eu/archives/3481-Whats-new-in-Solaris-10-0807-aka-Update-4.html).

I didn't really expect any problems when I went to install Update 4 on [ESX Server](http://www.vmware.com/products/vi/esx/) 3.0.2, and Solaris didn't disappoint. The new update installed quickly and flawlessly, and VMware Tools installed without a hitch. After installation, I modified the VM configuration to use the Intel e1000 driver, mostly because I know that the e1000 driver is supported for VLAN interfaces, something I have been and will continue to be experimenting with over the next day or so.

&lt;aside&gt;By the way, if you're having problems getting changes to the VMX file to stick (i.e., if [VirtualCenter](http://www.vmware.com/products/vi/vc/) keeps overwriting your changes), then remove the VM from inventory, edit the file, then browse the datastore and add the VM back to inventory. Your changes to the VMX file will now stick.&lt;/aside&gt;

Now that I have Update 4 running on ESX Server, the next step is to try using Solaris Containers (zones) on the virtualized instance, as well as testing the new iSCSI target functionality in Solaris to provide iSCSI storage for the ESX hosts. I'll post more information here (it may be slightly delayed because I'll be in San Francisco next week at VMworld) as soon as I've had the opportunity to conduct those tests and have some results.
