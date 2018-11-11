---
author: slowe
categories: Education
comments: false
date: 2006-07-17T14:25:01Z
slug: remotely-changing-user-account-passwords
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Remotely Changing User Account Passwords
url: /2006/07/17/remotely-changing-user-account-passwords/
wordpress_id: 301
---

Rather than using WMIC to do this (which is most likely possible), we'll pull in a couple of third-party freeware tools. First, we'll use [AdFind](http://www.joeware.net/win/free/tools/adfind.htm), by [Joe Richards](http://www.joeware.net/). We certainly could have used `dsquery` to provide the functionality we need, but this utility offers a bit more flexibility in the output options than `dsquery`. Next, we'll team AdFind up with [PsPasswd](http://www.sysinternals.com/Utilities/PsPasswd.html), part of [PsTools suite](http://www.sysinternals.com/Utilities/PsTools.html) by Sysinternals.

PsPasswd is a very flexible tool, capable of working on local or remote computers, multiple remote computers with their names specified on the command line, all computers within a domain, or remote computers specified by the contents of an input file. It does not, however, support having the name of the remote computer piped into it from another command (at least, not that I could tell). That doesn't mean we can't get our input for PsPasswd from another command, though.

Rather than using the old `for /f` trick, instead we're going to redirect the output of one command into a text file, then read that text file into PsPasswd to change local user account passwords---all on a single command line. We'll use a pair of ampersands to instruct the second command to run only if the first command completes successfully.

Here's what the command will look like:

    adfind -b ou=Workstations,dc=example,dc=net 
    -f "(objectcategory=computer)" dNSHostName -list > 
    c:\temp\pclist.txt && pspasswd @c:\temp\pclist.txt 
    pcsupport HsaEAA2e! > c:\temp\audit.txt

In this example, we use AdFind to search the Workstations OU in the example.net AD domain for computers, and return each computer's DNS host name in simple list format. This output is placed into the file `c:\temp\pclist.txt`. _If this command is successful_, then the PsPasswd command runs, reading the contents of that file and outputting the results into `c:\temp\audit.txt` for later review and storage.

Of course, this could easily be placed into a batch file and called with the OU as a parameter:

```text
REM Accepts an Active Directory base DN as a parameter, local account as a parameter, and new password as a parameter, then changes the password on the specified account on all PCs in the specified base DN.
REM
@echo off
adfind -b %1 -f "(objectcategory=computer)" dNSHostName -list > %temp%\adfindtmp.txt && pspasswd @%temp%\adfindtmp.txt %2 %3
del %temp%\adfindtmp.txt
```

It would be a good idea to incorporate some error checking, to ensure that the right number and type of parameters were specified, but I'll leave that as an exercise to the readers.

There are a couple of problems with this approach. First, it is assumed that all the PCs returned by the AdFind query will have the account specified on the command line; there is no functionality (not in this approach, at least) to specify the name of the account for each PC. In a standardized environment this may not be a big deal. Second, there is no way to determine if a computer returned by the query is actually up and running, so the PsPasswd command will attempt to operate even against PCs that may not be running or may not be reachable. This will cause some errors and delays.

Even given these limitations, however, this is a very useful technique for managing local user accounts that are used for support purposes.
