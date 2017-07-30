---
author: slowe
categories: Tutorial
comments: false
date: 2006-01-24T10:01:28Z
slug: bypassing-roots-password
tags:
- Debian
- Linux
- Security
title: Bypassing Root's Password
url: /2006/01/24/bypassing-roots-password/
wordpress_id: 164
---

I had a situation today where a customer forgot the root password to a [Debian](http://www.debian.org/) GNU/Linux 3.1 system in their office. That left it up to me to try to find a way to get into the system. Here's how I managed to gain access. 

(Note: As far as I am aware, _NONE_ of the information I'm going to list in this article will work across the network; you _MUST_ have physical access to the server. Therefore, I'm not too terribly worried about "making it easier for the hackers." If you don't have physical security, then no amount of electronic security is going to help you!)

Here's how it works:

1. With physical console access, reboot the server.

2. When the Grub menu comes up, press "e" to edit the menu selections.

3. Use the arrow keys to select the Kernel line, then press "e" again.

4. Add `single init=/bin/bash` to the end of the existing line.

5. Press "b" to boot the modified line.

6. The system will boot up into single-user mode. Unfortunately, the root filesystem will be mounted read-only, so you'll need to remount it using the `mount -o remount,rw /` command.

7. Use the `passwd` command to change the password for root to whatever you like.

8. Reboot the computer again and log in as root with the new password.

There are ways to protect against even this (a BIOS-based power-on password, or passwords in Grub to prevent casual editing of the boot configuration), and those steps may be necessary depending upon the other aspects of physical security. If this system is out where people can get to it, then I'd highly recommend taking these additional steps to secure the server.

Please note that I've only done this on Debian GNU/Linux 3.1, but I would be reasonably confident that the steps will work elsewhere as well.
