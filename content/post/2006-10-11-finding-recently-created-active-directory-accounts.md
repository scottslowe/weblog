---
author: slowe
categories: Education
comments: true
date: 2006-10-11T15:28:12Z
slug: finding-recently-created-active-directory-accounts
tags:
- ActiveDirectory
- Microsoft
title: Finding Recently Created Active Directory Accounts
url: /2006/10/11/finding-recently-created-active-directory-accounts/
wordpress_id: 345
---

The syntax for finding recently created Active Directory accounts using either `dsquery` or AdFind is listed below. In this context, we're defining "newly created accounts" as all accounts created after a specific date. The date is encoded into the commands, as explained below, so you can tailor this to your specific environment.

Using AdFind, the syntax would look like this:

```text
adfind -b dc=example,dc=net -f "(&(objectCategory=Person)
(objectClass=User)(whenCreated>=20061001000000.0Z))"
```

This would return all user accounts created on or after October 1, 2006. Of course, you could add the "-csv" option to output the results in CSV format, and redirect the output to a text file (using the "&gt;" redirection symbol) for further processing as needed.

Using `dsquery`, the syntax would look like this:

```text
dsquery * dc=example,dc=net -filter "(&(objectCategory=Person)
(objectClass=User)(whenCreated>=20061001000000.0Z))"
```

The syntax is very similar, and the actual LDAP query is identical between the two applications.

The key to making it work is the syntax of the whenCreated portion of the LDAP query, which breaks down like this:

    YYYY MM DD HH mm ss.s Z
    2006 10 01 00 00 00.0 Z

Specify a value of zeroes where the value is not important; in this case, we don't really care what time on or after October 1, 2006 the account was created so we leave the hour, minute, and seconds fields empty. The capital Z at the end is mandatory.

[This article at Microsoft](http://www.microsoft.com/technet/prodtechnol/windows2000serv/reskit/distrib/dsbc_nar_zgyg.mspx?mfr=true) discusses some of the search criteria, but the breakdown of the syntax for the whenCreated value wasn't correct; I found working values from [this Usenet discussion on microsoft.public.win2000.active_directory](http://groups.google.com/group/microsoft.public.win2000.active_directory/browse_thread/thread/87e0839aa447deeb/664ec0a4bff341a7%23664ec0a4bff341a7).
