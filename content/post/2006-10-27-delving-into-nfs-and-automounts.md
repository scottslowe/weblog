---
author: slowe
categories: Information
comments: true
date: 2006-10-27T23:06:57Z
slug: delving-into-nfs-and-automounts
tags:
- ActiveDirectory
- CentOS
- Linux
- Macintosh
- NFS
- Solaris
- UNIX
title: Delving into NFS and Automounts
url: /2006/10/27/delving-into-nfs-and-automounts/
wordpress_id: 350
---

The main goal in undertaking this effort is to create a structure in which hosts running Linux (typically [CentOS](http://www.centos.org/)) and [Solaris 10](http://www.sun.com/software/solaris/) share common home directories. These common home directories will be NFS-hosted shares that are automounted when a user logs in. By combining this with CIFS-hosted shares (for Windows-based clients), we can provide common home directories for users regardless of the OS to which they are logging in.

The plan was to use [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx) as the NFS server. A server running CentOS 4.3 and a server running Solaris 10, both already configured for Active Directory integration, would be used as the clients. In addition, I was going to test connectivity from a [Mac OS X](http://www.apple.com/macosx/) client as well.

Unfortunately, I just can't seem to make it work. I have the Server for NFS component installed on a newly-built file server, and I have all the Unix attributes all stored in Active Directory (UID, UID number, login shell, Unix home directory, etc.). But I can't seem to get my head wrapped around the need for "User Name Mapping," which is designed to match Windows accounts with Unix accounts. In this situation, the Windows accounts _are_ the Unix accounts! I installed and configured User Name Mapping on one of the DCs, and configured the NFS server to use that server, but things still don't seem to work.

Any Unix/NFS gurus out there care to help me understand this?
