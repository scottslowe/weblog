---
author: slowe
categories: Explanation
comments: true
date: 2010-01-13T16:38:15Z
slug: resetting-the-root-password-on-vmware-esx-40
tags:
- ESX
- Linux
- Virtualization
- VMware
- vSphere
title: Resetting the Root Password on VMware ESX 4.0
url: /2010/01/13/resetting-the-root-password-on-vmware-esx-40/
wordpress_id: 1796
---

Earlier today, I had to reset the root password on a lab server running VMware ESX 4.0 Update 1. For some reason, the password we assigned yesterday when we built the server from scratch wasn't working this morning. OK, no big deal, right? Just reboot the server into single user mode and away you go. I won't bother to repeat the steps for getting into single user mode; go to [this article](http://www.desktop-virtualization.com/2008/07/04/how-to-change-password-on-your-esx-server/) and it will give you what you need (the article is written for ESX 3.5 but it works fine for ESX 4.0).

Because this is a lab environment we just wanted to assign a simple password that anyone on the team could easily remember. (I'm sure the security purists out there are screaming right now.) Unfortunately, once I had the ESX host booted into single user mode, the `passwd` command insisted on making me use a complex password. There didn't seem to be any simple way around the restriction.

However, having spent a fair amount of time with PAM (Pluggable Authentication Modules) during my Linux-AD integration experiments, I figured there was a way around it by modifying the PAM configuration. Sure enough, the `/etc/pam.d/system-auth-generic` file contained a reference to `pam_passwdqc.so`, the library that is responsible for ensuring complex passwords. The fix, therefore, was to somehow remove `pam_passwdqc.so` from the PAM configuration so that I could assign a simple password.

The first thing I tried was simply commenting out the line for the module, but the `passwd` command then failed to work, reporting an error that the authentication token could not be obtained. Strike 1! Next, I leave the `pam_passwdqc.so` module commented out and try changing the next line to **required** instead of **sufficient**. Same error: strike 2!

Finally, I simply replaced the `pam_passwdqc.so` line with a reference to `pam_cracklib.so` (after making a backup copy of the original `/etc/pam.d/system-auth-generic` file, of course---it never hurts to be prepared). Success! I was able to assign a simple password to the lab server.

After putting the original `/etc/pam.d/system-auth-generic` back in place and rebooting the host, we were back in action! So, what was the lesson learned? You can't stop someone who's determined to get around security requirements! No, I'm just kidding...there is no lesson learned. I just thought someone might find this information useful or interesting. Enjoy!
