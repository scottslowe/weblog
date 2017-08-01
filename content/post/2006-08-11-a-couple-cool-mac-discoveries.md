---
author: slowe
categories: Explanation
comments: true
date: 2006-08-11T23:10:27Z
slug: a-couple-cool-mac-discoveries
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- Macintosh
- Networking
- UNIX
title: A Couple Cool Mac Discoveries
url: /2006/08/11/a-couple-cool-mac-discoveries/
wordpress_id: 318
---

The first of these discoveries I found while working on the [Kerberos SSO article][1], and while verifying information for my [updated Linux-AD integration instructions][2] for Windows Server 2003 R2.

As you may already know, [Mac OS X](http://www.apple.com/macosx/) has built-in Kerberos support. Not only can you use the standard `kinit` command from the Terminal, but [Apple](http://www.apple.com/) also provided a graphical interface to obtain and manage Kerberos tickets. Buried in `/System/Library/CoreServices` is Kerberos.app, a graphical application for obtaining and managing Kerberos tickets. Now, if you use your Mac in a Windows network running Active Directory (which is a lot of you), using this application to obtain a Kerberos ticket from your AD domain controller then means that you can mount any Windows share in your domain _without any password prompts whatsoever_. Furthermore, using [Safari](http://www.apple.com/macosx/features/safari/) or [Shiira](http://hmdt-web.net/shiira/en) to access sites using Integrated Windows Authentication means you'll be transparently logged on to those web sites _without any password prompts whatsoever_.

Even better, there are hooks that allow you to configure the Mac login window to automatically go get you a Kerberos ticket from Active Directory, further automating this process and further making your life easier.

The second handy discovery came today while working with a [Solaris](http://www.sun.com/software/solaris/) 10 virtual machine (I'm working on some testing of Solaris-AD integration using Kerberos and LDAP...look for an article soon). I needed a way to grab a remote X Window session---not just a single X11 window, but the whole desktop. So, based on [this information](http://www.macosxhints.com/article.php?story=20041117115414383), I launched X11 on my PowerBook, opened an SSH session to the Solaris server and ran this command:

    /usr/openwin/bin/Xnest :1 -query localhost

Lo and behold, a XDMCP session popped up on my Mac's desktop, all neatly contained within its own window. Very handy! I found that you must first set the DISPLAY variable in Terminal in order for this to work (or you just use the xterm that opens when you launch X11). Using this trick, I was able to do what I needed to do on the Solaris server.

&lt;aside&gt;I suspect that, given the fact that Solaris' SSH daemon is Kerberized, that once I get Kerberos support on the Solaris server configured I will be able to use Kerberos.app to obtain a Kerberos ticket from Active Directory, then seamlessly authenticate to the Solaris server to tunnel an X Window desktop over SSH. Now _that's_ cool.&lt;/aside&gt;

I'm looking forward to using Xnest to do the same thing on Linux servers as well, but haven't yet had the time to test it. I'll post more information here as soon as I do.

[1]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
[2]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
