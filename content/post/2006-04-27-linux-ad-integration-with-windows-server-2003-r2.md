---
author: slowe
categories: Tutorial
comments: true
date: 2006-04-27T11:20:10Z
slug: linux-ad-integration-with-windows-server-2003-r2
tags:
- Microsoft
- Interoperability
- ActiveDirectory
- CentOS
- Kerberos
- LDAP
- Linux
- Networking
- OSS
- RedHat
- Windows
title: Linux-AD Integration With Windows Server 2003 R2
url: /2006/04/27/linux-ad-integration-with-windows-server-2003-r2/
wordpress_id: 235
---

The integration of (what was formerly called) Services for UNIX into [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/) also brought some other changes. To accommodate those changes, I've updated my Linux-AD integration instructions (the [previous instructions are here][1] for pre-R2 versions of Windows). If you need to integrate Linux systems for authentication into Active Directory with Windows Server 2003 R2, these instructions should get you there.

Overall, the instructions are very similar to the instructions for pre-R2 versions of Windows.

### Preparing Active Directory (One-Time)

Based on what I've seen so far, it appears as if a partial RFC 2307-compliant schema is included by default with Windows Server 2003 R2. This means that it is no longer necessary to extend the schema to include attributes such as uid, gid, login shell, etc. However, while the schema does appear to be present by default, you must install the "Server for NIS" component on at least one domain controller in order to be able to actually set those attributes (and it will be necessary to set the attributes before logins from Linux will work).

You'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine.

### Preparing Active Directory (Each User)

Each Active Directory account that will authenticate via Linux must be configured with a UID and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account. Installing the "Server for NIS" component enables this, as mentioned previously.

After all the user accounts have been configured, then we are ready to perform the additional tasks within Active Directory and on the Linux server that will enable the authentication.

### Preparing Active Directory (Each Server)

For each Linux-based server that will be authenticating against Active Directory, follow the steps below.

1. Create a computer account in Active Directory. When creating the computer account, be sure to specify that this account may be used by a pre-Windows 2000-based computer.

2. Use the following command at a command prompt to configure the new computer account:  

``` text
ktpass -princ host/fqdn@REALM -mapuser DOMAIN\name$
-crypto DES-CBC-MD5 -pass password -ptype KRB5_NT_PRINCIPAL
-out filename
```

Of course, you'll need to substitute the appropriate values for "fqdn" (the fully-qualified domain name of the computer), "REALM" (the DNS name of your Active Directory domain in UPPERCASE), "DOMAIN" (the NetBIOS name of your Active Directory domain), "password" (the password that will be set for the new computer account), and "filename" (the keytab that will be generated and must be copied over to the Linux computer).

If you need to rebuild the Linux server for whatever reason, you'll need to delete the computer account you created and repeat this process.

### Preparing Each Linux Server

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

This configuration was tested on Red Hat Linux 9.0 as well as CentOS 4.3.

**UPDATE:** An [updated version][2] of these instructions has been posted.

[1]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[2]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[gist-1]: https://gist.github.com/scottslowe/67a3f8c36270c7e6376b
[gist-2]: https://gist.github.com/scottslowe/a7c89505c46a13d95ebe
