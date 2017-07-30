---
author: slowe
categories: Information
comments: true
date: 2007-05-23T14:43:22Z
slug: windows-server-2008-beta-3-on-esx-server
tags:
- ESX
- Microsoft
- Virtualization
- VMware
- Windows
title: Windows Server 2008 Beta 3 on ESX Server
url: /2007/05/23/windows-server-2008-beta-3-on-esx-server/
wordpress_id: 460
---

I just finished installing [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx) Beta 3 on [ESX Server](http://www.vmware.com/products/vi/esx/) 3.0.1. Despite the fact that the amount of time I spend designing, implementing, and supporting Microsoft products continues to decrease, this is an important product release and one with which I need to be _very_ familiar. In addition, I'm particularly interested in how developments in the Windows Server product line will affect Active Directory (AD) and AD integration scenarios. Once I have had the opportunity to create an AD structure based on Windows Server 2008, I plan to write a new set of articles on Linux and Solaris integration.

When creating the VM, I used the following configuration options:

* Selected "Windows Vista" as the guest OS

* Provisioned the VM with 512MB of RAM and a 6GB hard drive (I suspect we will be very tight on disk space, but I have to be frugal with my shared SAN storage in the lab)

* Specified the LSI Logic SCSI driver

The VM booted up without any problems, but quickly dropped me to a screen where it indicated that it did not have the necessary driver for the CD/DVD-ROM drive. A quick web search turned up [this VMTN Forums thread](http://www.vmware.com/community/thread.jspa?threadID=70651), where this link was offered to a [floppy image with the necessary drivers](http://sti.epfl.ch/intranet/informatique/virtualisation/drivers-vista-rtm-esx.flp.zip). The drivers worked perfectly, and the rest of the installation proceeded without any further issues.

Now that I have this thing up and running, the next set of tasks will be to create a new Active Directory structure and begin testing various integration scenarios with the new version of Active Directory. I'll also begin exploring interoperability between Active Directory on earlier versions of Windows and migration scenarios to Windows Server 2008. While I can't promise anything, let me know if there is something specific you'd like me to explore or test and I'll see what I can do (and document it here, of course).
