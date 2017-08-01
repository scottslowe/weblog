---
author: slowe
categories: Tutorial
comments: false
date: 2006-05-16T17:32:07Z
slug: mass-password-changes-in-ad-revisited
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Mass Password Changes in AD Revisited
url: /2006/05/16/mass-password-changes-in-ad-revisited/
wordpress_id: 250
---

Late last year, I published an entry that described a method for making [mass password changes in Active Directory][1]. After some additional work on the topic, I have found a _much_ better way of accomplishing the same thing.

In my previous example, I used `ldifde` to extract information from Active Directory. Unfortunately, that information was a bit too complete, and it included fields and attributes that we didn't need. As a result, we had to parse down the `ldifde` output and remove everything except the DN field.

I have since discovered the use of the `dsquery` command, which will output DNs for objects easily from a single command:

    dsquery user ou=Accounts,dc=testlab,dc=net

This command will produce a list of DNs for all users in the Accounts OU of the testlab.net AD domain. Appending a " > _filename_" to the command would redirect the output into a file that could be used later. One caveat to this command: be sure to use the "limit -XXX" parameter if there are more than 100 objects that you are trying to enumerate. Otherwise, you'll get only the first 100 objects.

Instead of redirecting the output of this command to a file we can pipe it to another command, such as `dsmod`:

    dsquery user ou=Accounts,dc=testlab,dc=net |
    dsmod user -pwd newpass1 -mustchpwd yes

This command will query all the users in the Accounts OU of the testlab.net AD domain, then set their passwords to "newpass1" and force a password change at next logon. (Although this command is shown on two lines above, it should all be entered as a single line.)

Note that these commands work equally well against Active Directory domains running both Windows 2000 and Windows Server 2003.

[1]: {{< relref "2005-12-08-mass-password-changes-in-active-directory.md" >}}
