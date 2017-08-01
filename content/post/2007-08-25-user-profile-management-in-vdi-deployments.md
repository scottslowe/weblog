---
author: slowe
categories: Explanation
comments: true
date: 2007-08-25T20:06:37Z
slug: user-profile-management-in-vdi-deployments
tags:
- Microsoft
- VDI
- Virtualization
- VMware
- Windows
title: User Profile Management in VDI Deployments
url: /2007/08/25/user-profile-management-in-vdi-deployments/
wordpress_id: 512
---

In a "traditional" (as if that word can really be applied to a new technology use-case like this) VDI deployments, we'd use roaming profiles to have users' settings follow them from hosted desktop to hosted desktop. In this article [using the Flex Profile Kit (FPK) in a VDI deployment][1], I described an implementation in which roaming profiles couldn't be used, and so we had to resort to the use of the FPK (which, incidentally, works well). There is, however, one problem: leftover profiles.

Usually, the FPK is used in conjunction with a mandatory profile and folder redirection, so there isn't any "stuff" leftover when a user logs in to a desktop. The FPK pulls down the customized portions of the user's settings, applies them to a mandatory profile, and off we go. This is an advantage over roaming profiles, where a local copy of the profile remains even after the user logs off. (Note that there are Group Policy settings to help control this behavior, if I recall correctly.)

Mandatory profiles, though, go hand-in-hand with roaming profiles; there doesn't seem to be any way to use one without the other (but not necessarily conversely). As a result, we settled for a compromise in the form of a few lines in the logout script that deletes certain files and settings that we don't want to persist between sessions.

Even so, we've found that profiles are still accumulating across machines, and occupying more space on the hosted desktops than we'd really prefer. So we had to come up with a way to get rid of these leftover profile remnants. That's where `Delprof.exe` came in.

Great utility, yes, but almost useless in the logout script because it won't delete the current user's profile (as it's still loaded at that point). We needed a different way of handling it, so I came up with this little batch file-wannabe that is scheduled to run from the [VirtualCenter](http://www.vmware.com/products/vi/vc/) server on a nightly basis:
    
    dsquery computer "ou=Hosted Desktops,dc=example,dc=com" \
    -o rdn > vdi-list.txt
    sed -f stripquotes.sed vdi-list.txt > vdi-list2.txt
    for /f "tokens=1" %1 in (vdi-list2.txt) do \
    delprof.exe /q /i /c:\%1 /d:1

(The lines above were wrapped for readability, with a backslash to indicate the continuation of a line.) It's pretty easy to see what this does, but let's break it down. First, it uses `dsquery computer` to list all the hosted desktops in the specified OU. Then, because `dsquery` returns the computer names inside double quotes and `Delprof` doesn't like computer names inside double quotes, we'll use `sed` to strip the quotes from the text file. The "stripquotes.sed" file is a simple regular expression to strip out double quotes. Finally, we use `for /f` (not seen here on this blog for quite a while now!) to feed the entries of the resulting text file to `Delprof.exe` to delete all profiles that have been inactive for more than 1 day.

Of course, we could adjust the "/d:1" parameter to keep profiles around longer; that would help balance the disk space used and the time it takes to set up a new profile when a user logs in. Depending upon the usage profile in your VDI deployment, this may need to be longer.

[1]: {{< relref "2007-03-27-using-fpk-in-vdi-deployments.md" >}}
