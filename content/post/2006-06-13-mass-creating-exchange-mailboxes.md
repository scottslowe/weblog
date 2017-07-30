---
author: slowe
categories: Tutorial
comments: false
date: 2006-06-13T18:28:30Z
slug: mass-creating-exchange-mailboxes
tags:
- Microsoft
- ActiveDirectory
- Exchange
title: Mass-Creating Exchange Mailboxes
url: /2006/06/13/mass-creating-exchange-mailboxes/
wordpress_id: 269
---

While performing some testing and research at the office today, I found myself in need of a way to mass-create some Exchange mailboxes. A very quick Google search revealed just the tool I needed to perform the task: [ExchMbx](http://www.joeware.net/win/free/tools/exchmbx.htm), a freeware utility by the same author of [AdFind](http://www.joeware.net/win/free/tools/adfind.htm) and [AdMod](http://www.joeware.net/win/free/tools/admod.htm).

The real power of ExchMbx is demonstrated when combined with `dsquery` (a Microsoft-supplied command-line tool for pulling lists of objects from Active Directory) or AdFind. In these examples I'm using AdFind because tools such as Dsquery don't work on Windows 2000 (at least, not in my experience).

So, let's say you wanted to mailbox-enable all the users in a particular OU. With AdFind, you could enumerate all the users in an OU like this:

    adfind -dsq -b "OU=Users,OU=Department,DC=example,DC=net"
    -f "(objectclass=user)"

(Be sure to type commands like this on a single line, not broken across lines for appearance's sake as shown here.)

This will produce a quoted DN listed similar to the output of `dsquery` (hence the "-dsq" switch). Then, this output can be fed to ExchMbx:

    adfind -dsq -b "OU=Users,OU=Location,DC=example,DC=net"
    -f "(objectclass=user)" | exchmbx -cr "SERVER1:First Storage 
    Group:Mailbox Store (SERVER1)"

Here, the DN output from AdFind is piped to ExchMbx to create a mailbox on the database named "Mailbox Store (SERVER1)" in the First Storage Group on SERVER1.

Or, you could move all the user objects for the HR personnel to a new Exchange server or database:

    adfind -dsq -b "OU=Users,OU=Location,DC=example,DC=net" 
    -f "(&(objectclass=user)(department=HR))" | 
    exchmbx -move "SERVER1:First Storage Group:Second Database"

To find which accounts don't have an Exchange mailbox (perhaps you only created Exchange mailboxes for a subset of your users), this command will help you out:

    adfind -dsq -b "ou=RTP,dc=legacyad,dc=net" 
    -f "(&(objectclass=user)(!(homeMDB=*)))"

You could then pipe this to ExchMbx again to create mailboxes, repeating the process until the AdFind command did not find any more accounts out there without mailboxes.

Of course, there's a lot more to ExchMbx than just creating and moving mailboxes; you can also mail-enable objects, hide or unhide objects from address lists, and set mailbox quotas. All in all, a very handy tool!
