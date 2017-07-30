---
author: slowe
categories: Information
comments: false
date: 2005-11-29T16:57:00Z
slug: secure-remote-filesystem
tags:
- BSD
- Encryption
- Linux
- Security
- SSH
title: Secure Remote Filesystem
url: /2005/11/29/secure-remote-filesystem/
wordpress_id: 128
---

This is something that only a computer junkie could enjoy. In conjunction with the [FUSE](http://fuse.sourceforge.net/) project (now an official part of the Linux kernel as of version 2.6.14), an SSH-wrapped remote filesystem---called [sshfs](http://fuse.sourceforge.net/sshfs.html)---has been created.

This [recent article](http://www.linux.com/article.pl?sid=05/11/11/176206) describes sshfs in a bit more detail and provides some additional information.

So what does this mean? It means that for any remote system you can reach via SSH, you can mount that remote system's filesystem inside an SSH tunnel. I can think of numerous possibilities, not the least of which involves easily updating a web site hosted on a remote web server without having to FTP (or SFTP) the files back and forth.

Now, if only there was a [Mac OS X](http://www.apple.com/macosx/) version of sshfs...it's currently only available for Linux and [FreeBSD](http://www.freebsd.org/).
