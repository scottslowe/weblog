---
author: slowe
categories: Explanation
comments: false
date: 2006-05-01T20:22:40Z
slug: permissions-in-the-esx-server-mui
tags:
- ESX
- Linux
- Virtualization
- VMware
title: Permissions in the ESX Server MUI
url: /2006/05/01/permissions-in-the-esx-server-mui/
wordpress_id: 239
---

I'm reasonably familiar with Linux/Unix permissions, so this isn't really hard to understand for me, but I'm going to post this information anyway for those of you that may not be as familiar with Linux/Unix-based permissions and how they affect the [ESX Server](http://www.vmware.com/products/esx/) MUI.

The Linux/Unix permission structure is defined in the following way.  There are three permissions: read, write, and execute. These three permissions may be granted to three entities: user, group, or others. You'll see them listed in the directory listing as "rwxr-xr-x"; this is decoded in the following way:

* The first "rwx" is for the user (i.e., the owner of the file), and it specifies that the owner has read, write, and execute permissions.

* The second "r-x" is for the group assigned to the file, and this specifies that members of the group have read and execute permissions.

* The third "r-x" is for others (i.e, everyone who is not the owner and is not in the group), and again states that all others have read and execute permissions.

You'll also see the permissions written in a numeric format, especially when in combination with the chmod command. For example, the "rwxr-xr-x" permissions described earlier would have a numeric equivalent of 755. This number is calculated as r=4, w=2, x=1, so "rwx" equals 7, and "r-x" equals 5.

How do these permissions affect the VMware MUI? The permissions on the VMware configuration file (the file ending in .vmx) control what operations will be permitted in the MUI. For example, if a user does not have write permissions to the VMX file, then the commands in the MUI change from "Configure Hardware" to "View Hardware". In order to grant someone the permission to modify a virtual machine, their user account must have the appropriate permissions on the VMX file. This can be accomplished by changing the group on the VMX file (say, to something like "vmops" or "vmadmins") and then adding write permissions for the group.

This would be accomplished using the following commands:

    chown user:vmops vmachine.vmx
    chmod g+w vmachine.vmx

Of course, you would substitute the appropriate names for "user" and "vmops", as appropriate.

Hopefully, this information will be helpful to those of you that are new to the Linux/Unix platform (upon which the console operating system for VMware ESX Server is based).
