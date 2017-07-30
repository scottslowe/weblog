---
author: slowe
categories: Tutorial
comments: true
date: 2007-10-23T12:33:44Z
slug: using-scponly-on-esx-server
tags:
- ESX
- Security
- SFTP
- SSH
- Virtualization
- VMware
title: Using scponly on ESX Server
url: /2007/10/23/using-scponly-on-esx-server/
wordpress_id: 565
---

Quite a few organizations implementing VMware Virtual Infrastructure 3 (VI3) are not organizations with a great deal of non-Windows experience. Given that the Service Console is based upon Red Hat Enterprise Linux, this is often a completely new environment for them that introduces unfamiliar processes and applications.

Take the process of copying files, for example. To an administrator within an organization that is primarily Windows-based servers, the idea of not being able to "map a drive" or just use a Universal Naming Convention (UNC) path is unbelievable. For someone like myself, who has lived in a non-Windows world for a number of years now, the idea of using SCP/SFTP to copy files to a server seems pretty normal. Nevertheless, these new VMware administrators must now get accustomed to doing things a bit differently than they have in the past, and the use of an SCP/SFTP client is one of those things.

One big problem with the use of SCP/SFTP clients is granting SCP/SFTP access to a user typically also grants shell access. This is a problem not only for new users but also for experienced users. New users may get into the shell and change things that shouldn't be changed, simply because they don't know; experienced users may get into the shell and change things that shouldn't be changed, because they think they _do_ know. Either way, shell access is something to be avoided where possible, in my opinion.

The solution? Use an "alternative" shell called [scponly](http://sublimation.org/scponly/wiki/index.php/Main_Page):

>scponly is an alternative 'shell' (of sorts) for system administrators who would like to provide access to remote users to both read and write local files without providing any remote execution priviledges. Functionally, it is best described as a wrapper to the tried and true ssh suite of applications.

Here's how I got scponly working on a test server running ESX Server 3.0.1 in the lab. Please keep in mind that this method is most likely not supported by VMware, so proceed accordingly.

1. Download the [scponly](http://dag.wieers.com/rpm/packages/scponly/) and [rsync](http://dag.wieers.com/rpm/packages/rsync/) RPM packages from the DAG repository. (The links point to the package page, not the actual package itself, so these are not blind download links.) I used _scponly-4.6-3.el3.rf.i386.rpm_ and _rsync-2.6.8-1.el3.rf.i386.rpm_, the packages for Red Hat Enterprise Linux 3, since the Service Console is derived from RHEL 3.

2. As root, first install the rsync package with the rpm command. Something like this should work: `rpm -Uvh rsync-2.6.8-1.el3.rf.i386.rpm`

3. After installing rsync, install scponly using `rpm -Uvh scponly-4.6-3.el3.rf.i386.rpm`.

4. Edit the `/etc/shells` file to include the scponly shell, which in my testing was installed to `/usr/bin`. You can simply append `/usr/bin/scponly` to the end of `/etc/shells`.

At this point, you should be able to create a user and set the user's shell to scponly. This will allow the user to use an SCP/SFTP client to transfer files, but it will not allow interactive shell access.

To test this, I used three file transfer methods:

* Interarchy, a Mac OS X application, via the SFTP protocol
* Command-line SCP from my Mac
* WinSCP on Windows XP, via both SFTP and SCP

In all three cases, I was able to successfully authenticate and transfer files, but I was not able to get access to the shell via SSH. I would imagine that a determined individual could probably get by scponly, but in this instance we are only using it to provide a layer of protection for the shell. For that function, it works well.
