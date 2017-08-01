---
author: slowe
categories: Tutorial
comments: true
date: 2006-12-19T11:07:59Z
slug: using-samba-in-linux-ad-integration
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Samba
title: Using Samba in Linux-AD Integration
url: /2006/12/19/using-samba-in-linux-ad-integration/
wordpress_id: 389
---

Suggestions to use [Samba](http://www.samba.org/) in Linux-AD integration scenarios appeared in the comments for the following articles:

[Linux, Active Directory, and Windows Server 2003 R2 Revisited][1]  
[Kerberos-Based SSO with Apache][2]

The idea was that Samba could be used to help automate the process of creating the appropriate service principals in Active Directory. Previously, I had recommended the use of `ktpass.exe` and separate user accounts for each service principal (i.e., HOST/ or host/, HTTP/, etc.) because of the limitations of `ktpass.exe` and adding service principals in Active Directory. However, a number of readers pointed out that Samba's `net ads join` and `net ads keytab` commands could help automate and streamline this process.

Since one of my Linux servers had crashed anyway, I decided to try out the Samba toolset while integrating this new Linux server into my existing Active Directory infrastructure. Here's what I found and the process that I used to successfully integrate the new Linux server into AD with the Samba tools.

1. First, the Kerberos client had to be configured properly. I'll refer you back to any one of the various Linux-AD integration articles I've written for more information on how to setup the `/etc/krb5.conf` file. You should be able to do a successful `kinit username@AD.DOMAIN.NAME` when `/etc/krb5.conf` is configured correctly.

2. Next, Samba must be properly configured. I used the following settings in `/etc/samba/smb.conf`:

    ``` text
    workgroup = <NetBIOS name of AD domain> 
    security = ads
    realm = <DNS name of AD domain>
    use kerberos keytab = true
    password server = <Space-delimited list of AD DCs>
    ```

3. For full Linux-AD integration, you must configure the nss\_ldap client. Again, I'll refer you to any one of the various AD integration articles I've written for more details on a suggested nss\_ldap configuration. When nss\_ldap is correctly configured, you should be able to do a `getent password <username>` and get back a list of properties (including UID, home directory, login shell, etc.) for that username.

4. Use `kdestroy` to kill any Kerberos credentials you may have established during testing, and then run `kinit <administrative account>@AD.DOMAIN.NAME` to get a Kerberos ticket for an administrative user in the AD domain.

5. If the DNS domain of your Linux server will be _different_ than the DNS domain of the AD domain (for example, perhaps your Linux server will be web1.linux.corp.com whereas Active Directory uses ad.corp.com), then create a computer account in Active Directory. If the Linux server's DNS domain will be the same as the DNS domain for AD, then we can have Samba create it for us. (I ran into problems here since the Linux server does use a different DNS domain than Active Directory, and pre-creating the computer account was the only way to make it work.)

6. Run `net ads join` to join the Linux server to Active Directory. As part of this process, it will add various SPNs to the computer account in Active Directory automatically and create the appropriate entries in the local Kerberos keytab (`/etc/krb5.keytab`, by default). No more `ktpass.exe`!

At this point, you can configure PAM appropriately (again, refer to one of the previous integration articles for full details on PAM configuration) and login to the Linux server with an Active Directory account.

I used this process to integrate a new [CentOS](http://www.centos.org/) 4.4 server into Active Directory without any problems whatsoever. I used the Kerberos, LDAP, nsswitch.conf, and PAM configurations from [this Linux-AD integration article][1] within the framework of the steps listed above and ran into only one problem (that was the issue with the differing DNS domains). Otherwise, it worked just fine.

Thanks to those readers who suggested the use of Samba!


[1]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[2]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
