---
author: slowe
categories: Musing
comments: true
date: 2007-04-25T09:57:50Z
slug: sshjail-in-centralized-environments
tags:
- Linux
- OSS
- Security
- SSH
- UNIX
title: SSHjail in Centralized Environments
url: /2007/04/25/sshjail-in-centralized-environments/
wordpress_id: 446
---

The idea of [chrooting](http://en.wikipedia.org/wiki/Chroot_jail) (or jailing) certain security-sensitive services is a well-known and pretty well-accepted method of protecting systems against further compromise in the event of a security breach. BIND is commonly run in a chroot jail, as can be Apache HTTPD or an FTP server. SSH is another common target for running in a chroot jail, and [SSHjail](http://paradigma.pt/~gngs/sshjail/) is a patch designed to simplify the process of running [OpenSSH](http://www.openssh.org/) in a chroot jail. (UNIX die-hards, please forgive me and correct me if I am mistakenly interchanging "chroot" and "jail".)

I was alerted to SSHjail via [this article](http://www.linux.com/article.pl?sid=07/04/11/211209) on [Linux.com](http://www.linux.com/), and it certainly appears that SSHjail greatly simplifies the process of running OpenSSH in a chroot jail. What interested me more than the configuration or use of SSHjail (which, as I mentions, looks pretty straightforward---kudos to the developer) was the question, "Could SSHjail be used in centralized authentication environments?"

Perhaps due to my work in [Linux/UNIX-Active Directory integration][1], but the idea of using SSHjail initially seemed to be at odds with an environment where users are being authenticated via Kerberos/LDAP against Active Directory. After all, the home directory would normally be specified on the user object's properties in AD, so how would that interact with the home directory configuration specified in the `/etc/sshjail.conf` file? Is SSHjail so transparent that it won't matter? For example, if I specify that `/home/slowe` is the UNIX home directory in AD, and SSHjail is configured to put me into a jail at `/chroot/ssh/`, do I need to then change the UNIX home directory in AD? The article seems to imply that it does, as it mentions editing local users to specify a new home directory location. How, then, do we handle disparate systems where SSH may be jailed on some and not on others?

&lt;aside&gt;Of course, this brings back up the question of how to handle different operating systems, such as Solaris and Linux, that (by default) place home directories in different locations on the file system or in different file systems.&lt;/aside&gt;

Any feedback or clarification from Linux/UNIX experts out there is welcome. It would be great to be able to include information on how to utilize SSHjail in conjunction with AD integration.

[1]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
