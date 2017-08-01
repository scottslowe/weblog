---
author: slowe
categories: Information
comments: true
date: 2007-12-04T13:53:30Z
slug: centos-5-active-directory-integration-problem
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- Samba
title: CentOS 5 Active Directory Integration Problem
url: /2007/12/04/centos-5-active-directory-integration-problem/
wordpress_id: 587
---

Since I had [CentOS 5 up and running on ESX Server][1] in the test lab, I decided to try to validate my [latest Linux-AD integration instructions][2] on this installation. Unfortunately, the instructions do not seem to work well _at all_ with CentOS 5; here are some of the errors that I ran into:

## Existing Kerberos Tickets Not Recognized

When using `net ads join` to join the Active Directory domain, it didn't recognize any existing Kerberos tickets. I'd already run a `kinit <AD username>`, but `net ads` continued to either a) try to use the root account if I didn't specify the `-U <AD username>` parameter, and b) prompt for password even when I'd already obtained a Kerberos ticket for the specified username.

## Permission Denied

When initially trying to join the Active Directory domain, `net ads join` threw this error:  

	[2007/12/04 12:57:08, 0] libads/kerberos.c:create_local_private_krb5_conf_  
	for_domain(594) create_local_private_krb5_conf_for_domain:  
	failed to create directory /var/cache/samba/smb_krb5.  
	Error was Permission denied

This error persisted until I manually created the `/var/cache/samba/smb_krb5` directory myself. Why this directory wasn't created automatically during the Samba installation is beyond me. Once I created that directory, the error went away, but Samba still wouldn't create the keytab or add entries to the keytab.

## No Keytab Created

The `net ads keytab` command failed miserably; it would not create a keytab, nor would it add entries to a keytab. No error message is reported; it just doesn't work.

I inquired on the #samba IRC channel on irc.freenode.net, but the only person willing or able to respond didn't have any information to provide (in fact, he'd actually used [my Solaris-AD integration instructions][3] as a guide for some of his own work). Various Google searches also failed to provide any helpful information.

By the way, these tests were performed on a stock installation of CentOS 5, with all the latest packages installed using `yum update`. The Samba version was 3.0.25b-1.el5_1.2.

In the end, I've given up on trying to make Samba work in the AD integration process and will instead fallback to the use of `ktpass.exe` to create the keytab file. If you have any useful information to share, please let me know or post it in the comments. Thanks!

[1]: {{< relref "2007-09-05-centos-5-on-esx-server.md" >}}
[2]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[3]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
