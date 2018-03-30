---
author: slowe
categories: Tutorial
comments: true
date: 2005-12-22T11:26:46Z
slug: complete-linux-ad-authentication-details
tags:
- Interoperability
- ActiveDirectory
- Kerberos
- LDAP
- Linux
- Microsoft
- OSS
- Windows
title: Complete Linux-AD Authentication Details
url: /2005/12/22/complete-linux-ad-authentication-details/
wordpress_id: 143
---

**UPDATE:** These instructions are for Windows 2000 Server and Windows Server 2003 pre-R2. For the R2 release, please see [these updated instructions][1].

There are several articles posted here that discuss, in general terms, how to authenticate Linux against Active Directory. For example, see this brief article on the [broad direction][2] of the integration, or this posting on [using computer accounts][3] for Linux servers (most how-to documents I've seen discuss the use of a user account for this purpose).

Over the last week or so, I've been having to rebuild one Linux server a number of times in an effort to fix a separate problem (see this article on [NTPd problems with CentOS 4.1][4] and the [recurrence of the problem][5] with CentOS 4.2). As a result, I've had to go through the process of configuring this Linux server (and preparing Active Directory) for authentication several times, and I found that I kept having to go back to my scattered and dispersed notes to remember what had to be done. In the hopes of correcting that, I am collecting all the pertinent information I have here in this article.

### Preparing Active Directory (One-Time)

There is a one-time preparation of Active Directory that is required. In order to authenticate Linux logins against Active Directory, Active Directory's schema must be extended to include Linux-specific attributes such as home directory, UID, GID, and default shell. There are a couple of different ways to do this; I chose to use Microsoft's Services for Unix (SFU) to extend the schema. There are two reasons I chose SFU: 1) it's free; and 2) the SFU schema is being included by default in Windows Server 2003 R2.

Once SFU has been installed and the schema has been extended, then the specific Active Directory accounts that are allowed to authenticate via Linux must be configured with a UID and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account.

You'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine.

After all the user accounts have been configured, then we are ready to perform the additional tasks within Active Directory and on the Linux server that will enable the authentication.

### Preparing Active Directory (Each Server)

For each server that will be authenticating against Active Directory, follow the steps below.

1. Create a computer account in Active Directory. When creating the computer account, be sure to specify that this account may be used by a pre-Windows 2000-based computer.

2. Use the following command at a command prompt to configure the new computer account:

``` text
ktpass -princ host/fqdn@REALM -mapuser DOMAIN\name$  
-crypto DES-CBC-MD5 -pass password -ptype KRB5_NT_PRINCIPAL  
-out filename
```

Of course, you'll need to substitute the appropriate values for "fqdn" (the fully-qualified domain name of the computer), "REALM" (the DNS name of your Active Directory domain in UPPERCASE), "DOMAIN" (the NetBIOS name of your Active Directory domain), "password" (the password that will be set for the new computer account), and "filename" (the keytab that will be generated and must be copied over to the Linux computer).

If you need to rebuild the Linux server for whatever reason, you'll need to delete the computer account you created and repeat this process.

### Preparing the Linux Server

Follow the steps below to configure the Linux server for authentication against Active Directory.

1. Make sure that the appropriate Kerberos libraries, OpenLDAP, pam\_krb5, and nss\_ldap are installed. If they are not installed, install them.

2. Be sure that time is being properly synchronized between Active Directory and the Linux server in question. Kerberos requires time synchronization.

3. Edit the `krb5.conf` file to look something like [this][gist-1], substituting your actual host names and domain names where appropriate.

4. Edit the `/etc/ldap.conf` file to look something like [this][gist-2], substituting the appropriate host names, domain names, account names, and distinguished names (DNs) where appropriate.

5. Securely copy the file created using the `ktpass.exe` utility above to the Linux server in question, placing it in the `/etc` directory as `krb5.keytab`. (SFTP or SCP are excellent candidates for this.)

6. Configure PAM (this varies according to Linux distributions) to use pam_krb5 for authentication. Many modern distributions use a stacking mechanism whereby one file can be modified and those changes will applied to all the various PAM-aware services. For example, in Red Hat-based distributions, the system-auth file is referenced by most other PAM-aware services.

7. Edit the `/etc/nsswitch.conf` file to include "ldap" as a lookup source for passwd, shadow, and groups.

That should be it. Once you do that, you should be able to use `kinit` from a Linux shell prompt (for example, `kinit aduser`) and generate a valid Kerberos ticket for the specified Active Directory account.

At this point, any PAM-aware service that is configured to use the stacked system file (such as the system-auth configuration on Red Hat-based distributions) will use Active Directory for authentication. Note, however, that unless you also add the pam\_mkhomedir.so module in the PAM configuration, home directories will have to be created manually for any Active Directory account that may log on to that server. (I generally recommend the use of pam\_mkhomedir.so in this situation.)

[1]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[2]: {{< relref "2005-07-13-linux-ad-integration-direction.md" >}}
[3]: {{< relref "2005-07-18-minor-linux-ad-hiccup-fixed-hopefully.md" >}}
[4]: {{< relref "2005-08-16-strange-ntpd-problem-on-centos-41.md" >}}
[5]: {{< relref "2005-12-19-ntpd-on-centos-42.md" >}}
[gist-1]: https://gist.github.com/scottslowe/67a3f8c36270c7e6376b
[gist-2]: https://gist.github.com/scottslowe/722dc11cd7967c6ea12b
