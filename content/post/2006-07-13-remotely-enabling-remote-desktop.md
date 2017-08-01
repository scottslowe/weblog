---
author: slowe
categories: Education
comments: true
date: 2006-07-13T16:10:07Z
slug: remotely-enabling-remote-desktop
tags:
- Microsoft
- Windows
title: Remotely Enabling Remote Desktop
url: /2006/07/13/remotely-enabling-remote-desktop/
wordpress_id: 296
---

Other than exploring a new WMIC alias here, you won't see any startling new tricks or techniques here. We'll be reusing tools that are already well-worn but still useful.

In [Windows Server 2003](http://www.microsoft.com/windowsserver2003/), Microsoft added the RDTOGGLE WMIC alias. In [Windows XP](http://www.microsoft.com/windowsxp/), you had to use the Win32_TerminalServiceSetting path. In either case, this alias or path allows you to toggle the value of the Remote Desktop setting. The syntax is a bit wierd, if you ask me, but this is how it looks (the lines are wrapped here for readability, but be sure to type it all on one line):

    wmic rdtoggle where AllowTSConnections="0" 
    call SetAllowTSConnections "1"

Of course, this being WMIC, we can easily add the "remote" functionality to this command with a simple switch:

    wmic /node:remotepc1 rdtoggle where AllowTSConnections="0" 
    call SetAllowTSConnections "1"

If you're running this command from a Windows Server 2003-based computer, you can use the `rdtoggle` alias even when executing the command remotely against a Windows XP-based system. In the event you need to run this on Windows XP, use this command instead:

    wmic /node:remotepc1 path Win32_TerminalServiceSetting 
    where AllowTSConnections="0" call SetAllowTSConnections "1"

Note that this syntax (using the path instead of the alias) works equally well on either Windows Server 2003 or Windows XP.

We can automate this process by combining this command with `for /f` to either a) perform this command on a predetermined list of computers whose names are stored in a file, like this technique for [adding entries to a WINS server][1]; or b) embed a command, such as a `dsquery` command, that will dynamically return a list of computers on which the command will be performed. We demonstrated this idea in [remotely setting the DNS suffix search order][2]. Refer back to these articles and other recent articles for more examples of using `for /f` to script command-line utilities that don't normally accept piped input.

Putting all this into practice, it now becomes possible to use `dsquery` to return a list of all the computers in an OU, and for each computer in the OU that does not have Remote Desktop enabled we can enable it. Pretty handy, eh?

**UPDATE:** It appears that a simple Registry change may also have the same result, although the jury is out on whether a reboot is required for the change to take effect. Preliminary testing with a Windows XP-based system that already had Remote Desktop enabled showed that changing the Registry value was immediate; however, I still need to test this on a system that has never had Remote Desktop enabled previously. (Update: See new information below!)

The Registry key to change is:

    HKLM\System\CurrentControlSet\Control\Terminal Server

A REG_DWORD value named fDenyTsConnection controls Remote Desktop; a value of 1 means that connections are denied (thus, Remote Desktop is disabled); a value of 0 means that connections are not denied (thus, Remote Desktop is enabled).

Credit to [Daniel Petri](http://www.petri.co.il/remotely_enable_remote_desktop_on_windows_server_2003.htm) for this information (there were lots of sites with this information, but his was first in the Google search).

**UPDATE 2:** I had the opportunity to test the Registry change on computers that did not have Remote Desktop enabled previously. Changing the referenced Registry key immediately enables Remote Desktop; no reboot is required.

[1]: {{< relref "2006-06-28-automating-static-entries-in-wins.md" >}}
[2]: {{< relref "2006-07-06-remotely-setting-the-dns-suffix-search-order.md" >}}
