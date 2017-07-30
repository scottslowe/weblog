---
author: slowe
categories: Information
comments: false
date: 2006-06-30T09:27:04Z
slug: mounting-iso-images-in-linux
tags:
- CLI
- Linux
- OSS
title: Mounting ISO Images in Linux
url: /2006/06/30/mounting-iso-images-in-linux/
wordpress_id: 285
---

Here's another incredibly simple task that one often needs to perform when using Linux: mounting an ISO image. The problem is, I so very rarely do this that I forget the exact switches to use. So, to avoid that problem in the future, I'm posting the information here for future reference. Even if no one else finds it useful, at least I'll know where to look next time I need to do this.

To mount an ISO file, use the following command:

    mount -t iso9660 -o loop /path/to/image.iso /mount/path

I know, a very simple command and one that Linux veterans around the world have probably used a million times over. Like I said, when it's not something that you do every day, it's easy to forget it. (Especially when your brain is busy trying to process other new information...)
