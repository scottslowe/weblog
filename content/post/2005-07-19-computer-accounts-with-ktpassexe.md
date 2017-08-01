---
author: slowe
categories: Information
comments: false
date: 2005-07-19T13:08:06Z
slug: computer-accounts-with-ktpassexe
tags:
- Kerberos
- Linux
- Microsoft
- Windows
- Interoperability
title: Computer Accounts With ktpass.exe
url: /2005/07/19/computer-accounts-with-ktpassexe/
wordpress_id: 54
---

As a quick follow-up to my previous posting, testing with Kerberos authentication from a Linux server with pam_krb5 was successful. Instead of using a user account in Active Directory to generate the keytab, I was able to use a computer account and just had to modify the `ktpass.exe` command line syntax slightly (see [my previous post][xref-1]).


[xref-1]: {{< relref "2005-07-18-minor-linux-ad-hiccup-fixed-hopefully.md" >}}
