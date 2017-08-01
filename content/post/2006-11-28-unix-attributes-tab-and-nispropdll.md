---
author: slowe
categories: Explanation
comments: true
date: 2006-11-28T09:51:09Z
slug: unix-attributes-tab-and-nispropdll
tags:
- ActiveDirectory
- Interoperability
- Linux
- Microsoft
- UNIX
title: Unix Attributes Tab and nisprop.dll
url: /2006/11/28/unix-attributes-tab-and-nispropdll/
wordpress_id: 372
---

A number of the readers of my article describing [integration between Linux and Active Directory on Windows Server 2003 R2][1] have inquired about the need to install Server for NIS on a domain controller. Even though we don't necessarily need NIS for this process (although we will need NIS if we are going to use NFS and automounts), installing the Server for NIS also makes available the "UNIX Attributes" tab in the Active Directory Users and Computers console. You'll need some sort of access to the attributes in Active Directory (unixHomeDirectory, gidNumber, uid, uidNumber, gecos, loginShell) in order to set them so that Linux and UNIX systems can utilize the information in those attributes, so installing Server for NIS in order to get the "UNIX Attributes" tab makes sense.

It's not the "UNIX Attributes" tab that's important; it's access to those attributes in Active Directory. You could just as well use ADSI Edit, LDP, or programmatically edit the attributes via VBScript or an LDIF import file. It doesn't matter. All that matters is that you have the ability to set and modify the values in the UNIX-related attributes.

One common workaround that has been mentioned is just registering the `nisprop.dll` file, using a command like this:

    regsvr32 c:\windows\idmu\common\nisprop.dll

Normally, this trick would work well. I used this trick, for example, to make Active Directory Users and Computers available to help desk personnel without having to install all the administrative tools (just copy down `dsadmin.dll` and register it). Not this time, though.

As Andy Loggia pointed out to me (first in the comments, and again later in a separate e-mail message), registering `nisprop.dll` requires Schema Admin privileges. At first, I didn't believe him, but he's absolutely right. When you register `nisprop.dll`, a change needs to be made in the Configuration naming context of Active Directory---and making that change requires Schema Admin privileges.

Specifically, registering `nisprop.dll` adds the CLSID of `nisprop.dll` to the AdminPropertyPages attribute of the user-display and group-display objects in this location in Active Directory:

	CN=409,CN=DisplaySpecifiers,CN=Configuration,DC=example,DC=net

(The "CN=409" would change if you are running a language other than English.) I verified this myself on my own instance of Active Directory in the lab and Andy is absolutely correct. Good work, Andy!

If you are working on Linux-AD integration in your shop, then just keep in mind that at some point during the process you'll probably need to have Schema Admin privileges. Certainly while you are extending the schema (if it's not already extended, which you can check using ADSI Edit), then when you install Server for NIS or register `nisprop.dll`. Alternately, if you don't want the "UNIX Attributes" tab in Active Directory Users and Computers, you can use tools such as LDP, ADSI Edit, LDIF import files, or scripts to populate and edit the values in the UNIX-related attributes. Populating these values is necessary for the process to work correctly, but the method by which the attributes are populated is up to you.

[1]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
