---
author: slowe
categories: Information
comments: true
date: 2013-05-02T17:07:29Z
slug: potential-fix-for-kerberos-ssh-ldap-and-active-directory-issue
tags:
- Interoperability
- ActiveDirectory
- Kerberos
- LDAP
- Linux
- SSH
title: Potential Fix for Kerberos, SSH, LDAP, and Active Directory Issue
url: /2013/05/02/potential-fix-for-kerberos-ssh-ldap-and-active-directory-issue/
wordpress_id: 3169
---

I had a reader contact me with a question on using Kerberos and LDAP for authentication into Active Directory, based on [Active Directory integration work][1] I did many years ago. I was unable to help him, but he did find the solution to the problem, and I wanted to share it here in case it might help others.

The issue was that he was experiencing a problem using native Kerberos authentication against Active Directory with SSH. Specifically, when he tried open an SSH session to another system from a user account that had a Kerberos Ticket Granting Ticket (TGT), the remote system dropped the connection with a "connection closed" error message. (The expected behavior should have been to authenticate the user automatically using the TGT.) However, when he stopped the SSH daemon and then ran it manually as root, the Kerberos authentication worked.

It's been a number of years since I dealt with this sort of integration, so I wasn't really sure where to start, to be honest, and I relayed this to the reader.

Fortunately, the reader contacted me a few days later with the solution. As it turns out, the problem was with SELinux. Apparently, by copying the keytab file from a Windows KDC (an Active Directory domain controller), the keytab is considered "foreign" because it doesn't have the right security context. The fix, as my reader discovered, is to use the `restorecon` command to reset the security context on the Kerberos files, like this (the last command may not be necessary):

    restorecon /etc/krb5.conf
    restorecon /etc/krb5.keytab
    restorecon /root/.k5login

Once the security context had been reset, the Kerberos authentication via SSH worked as expected. Thanks Tomas!

[1]: {{< relref "2007-01-15-active-directory-integration-index.md" >}}
