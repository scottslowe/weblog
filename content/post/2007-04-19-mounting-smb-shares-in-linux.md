---
author: slowe
categories: Information
comments: true
date: 2007-04-19T11:12:51Z
slug: mounting-smb-shares-in-linux
tags:
- Interoperability
- Linux
- Networking
- Samba
title: Mounting SMB Shares in Linux
url: /2007/04/19/mounting-smb-shares-in-linux/
wordpress_id: 443
---

This is one of those commands that you need to know, but use it so very rarely that it's hard to memorize. It seems like I have to go back and look this up every time I need to use it. What is it? It's the command to mount an SMB share from the typical Linux host.

The command looks something like this:

    mount -t smbfs -o username=User,password=Pass
      //host.IP.addr.ess/sharename /local/mnt

Of course, this should be typed all on a single line.

I find myself most often using this command when I need to mount an SMB share from the Service Console of one of my ESX hosts. So, to keep myself from having to go out and perform yet another Google search next time I need this, I'll know to just look right here.
