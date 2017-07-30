---
author: slowe
categories: Tutorial
comments: false
date: 2005-12-08T22:16:58Z
slug: mass-password-changes-in-active-directory
tags:
- Microsoft
- ActiveDirectory
- Windows
title: Mass Password Changes in Active Directory
url: /2005/12/08/mass-password-changes-in-active-directory/
wordpress_id: 137
---

Earlier this year, I had a need in a project to set the password for a large number of Active Directory accounts simultaneously. Here's the solution I came up with for this particular need.

To use this technique, you'll need `ldifde` (included with [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx)), `grep` (included with [Mac OS X](http://www.apple.com/macosx/) and most Linux distributions; Win32 versions available on the Internet), a text editor with search and replace functionality (advanced geeks are free to use `sed`), and `dsmod` (from the _Windows Server 2003 Resource Kit_).

First, export the list of user accounts out of Active Directory using `ldifde`. The command will look something like this:

    ldifde -d "CN=Users,DC=company,DC=com" -r "(objectclass=user)" -f c:\export-1.ldif

This creates a file called `export-1.ldif`. Using `grep`, filter this file down to only the full distinguished names of the users:

    cat export-1.ldif | grep 'dn: ' > export-2.ldif

Note that you'll need to use `type` instead of `cat` on a Win32 system. Also, on a Win32 system you'll need to use double quotes instead of single quotes in the grep command. This creates a file called `export-2.ldif`.

Load this file into the text editor and make the following changes:

* Remove all occurrences of "dn: " (there is a space after the colon)
* Add a double quotation mark before CN= at the start of each line
* Add a double quotation mark after =com at the end of each line

Save this modified file as `export-3.ldif`.

Finally, pipe this file through to the `dsmod` program to set passwords for all the users in the file:

    type export-3.ldif | dsmod user -pwd newpass1 -mustchpwd yes

Full help for the `dsmod` command line syntax is available using `dsmod /?` or `dsmod user /?`.

You can add " > _filename_" to the end of the above command to log the output of the `dsmod` command to a file. You can then use `grep` to parse this file to ensure that the command was successful for all users.
