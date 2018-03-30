---
author: slowe
categories: Tutorial
comments: true
date: 2007-01-15T14:22:31Z
slug: linux-ad-integration-version-4
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Microsoft
- Samba
- SSH
title: Linux-AD Integration, Version 4
url: /2007/01/15/linux-ad-integration-version-4/
wordpress_id: 400
---

This procedure allows Linux-based systems to authenticate against Active Directory. This configuration uses Kerberos for authentication, LDAP for account information, and [Samba](http://www.samba.org/) to help automate the process along the way. When this process is complete, AD users can be enabled for use on Linux systems on the network and login to those Linux systems using the same username and password as throughout the rest of Active Directory.

These instructions are designed for use with [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/). If you are looking for information on using Linux with a previous version of Windows, please refer back to [this article][1]. The only significant changes in the process involve the mapping of the LDAP attributes; otherwise, the procedure is very similar between the two versions of Windows.

## Preparing Active Directory (One-Time)

### Enable Editing/Display of UNIX Attributes

Based on my research, it appears that the partially RFC 2307-compliant schema is installed with Windows Server 2003 R2; this means that the schema does not need to be extended to include UNIX-specific attributes such as uid, gid, login shell, etc. However, while the attributes are there in the schema, there is no way to edit those attributes, and these attributes _must_ be populated correctly in order for this process to work.

The easiest way to enable the editing of these attributes is to install the "Server for NIS" component on at least one domain controller. This will cause a new tab, labeled "UNIX Attributes," to appear in the properties dialog box for users and groups. You'll use this tab to edit the UNIX-specific attributes that are required for logins to Linux-based systems. Please note that due to the way this tab is displayed, you'll need Schema Admin privileges in order to install the "Server for NIS" component on your domain controller. (More information on this issue is available [here][2].)

You could just as well use LDP, LDIF files, ADSI Edit, or any number of other methods to display and edit these attributes. To make this process as seamless as possible, however, you'll want to integrate the management of these attributes into Active Directory Users and Computers using the method described above.

### Create an LDAP Bind Account

You'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine. This helps minimize any potential security risks as a result of this account.

## Prepare Active Directory (Each User)

Each Active Directory account that will authenticate via Linux must be configured with a UID and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account. (Installing the "Server for NIS" component enables this, as mentioned previously.)

After all the user accounts have been configured, then we are ready to configure the Linux server(s) for authentication against Active Directory.

## Prepare Each Linux Server

Follow the steps below to configure the Linux server for authentication against Active Directory.

1. Edit the `/etc/hosts` file and ensure that the server's fully-qualified domain name is listed first after its IP address.

2. Make sure that the appropriate Kerberos libraries, OpenLDAP, pam\_krb5, and nss\_ldap are installed. If they are not installed, install them.

3. Be sure that time is being properly synchronized between Active Directory and the Linux server in question. Kerberos requires time synchronization. Configure the NTP daemon if necessary.

4. Edit the `/etc/krb5.conf` file to look something like [this](https://gist.github.com/scottslowe/67a3f8c36270c7e6376b), substituting your actual host names and domain names where appropriate.

5. Edit the `/etc/ldap.conf` file to look something like [this](https://gist.github.com/scottslowe/7ad13c8839a546b760df), substituting the appropriate host names, domain names, account names, and distinguished names (DNs) where appropriate. (**Note:** These schema mappings assume that you are using the newer schema extensions provided by Windows Server 2003 R2. If you are using SFU 3.5 instead, you will need to use the schema mappings described [here][1].)

6. Configure PAM (this varies according to Linux distributions) to use `pam_krb5` for authentication. Many modern distributions use a stacking mechanism whereby one file can be modified and those changes will applied to all the various PAM-aware services. For example, in Red Hat-based distributions, the `system-auth` file is referenced by most other PAM-aware services. [Here's](https://gist.github.com/scottslowe/0e47e27dd5e515963daf) a properly edited `/etc/pam.d/system-auth` file taken from CentOS 4.4 (some lines have been wrapped for readability; do not wrap them when editing the file).

7. Edit the `/etc/nsswitch.conf` file to include "ldap" as a lookup source for passwd, shadow, and groups.

At this point we are now ready to test our configuration and, if successful, to perform the final step: to join the Linux server to Active Directory for authentication.

## Test the Configuration

To test the Kerberos authentication, use the `kinit` command, as in `kinit <AD username>@<AD domain DNS name>`; this should return no errors. A `klist` at that point should then show that you have retrieved a TGT (ticket granting ticket) from the AD domain controller. If this fails, go back and troubleshoot the Kerberos configuration. In particular, if you are seeing references to failed TGT validation, check to make sure that both your Linux servers and AD domain controllers have reverse lookup (PTR) records in DNS and that the Linux server's `/etc/hosts` file listed the FQDN of the server first instead of just the nodename.

&lt;aside&gt;Some readers and some other articles have suggested the use of the AD domain DNS name in the `/etc/krb5.conf` file instead of an AD domain controller specifically; I recommend against this. First, I believe it may contribute to TGT validation errors; second, it is possible to list multiple KDCs (AD DCs) in the configuration. Since the only major reason to use the AD domain DNS name instead of the DNS name of one or more DCs would be fault tolerance, then it doesn't really gain anything.&lt;/aside&gt;

To test the LDAP lookups, use the `getent` command, as in `getent passwd <AD username>`; this should return a listing of the account information from Active Directory. If this does not work, users will _not_ be able to login, even if Kerberos is working fine. If you run into errors or failures here, go back and double-check the LDAP configuration. One common source of errors is the name of the LDAP bind account, so be sure that is correct.

## Join the Linux Server to Active Directory

This is the final step. Don't try this step until you've successfully tested the configuration. After this step is completed, you are finished and AD users will be able to login to Linux-based systems (assuming the AD users have been properly configured for Linux logins).

To join the Linux server to Active Directory, follow these steps:

1. Verify the Samba configuration. Be sure the following settings are specified in `/etc/samba/smb.conf`:  

	``` text
    workgroup = <NetBIOS name of AD domain> 
    security = ads
    realm = <DNS name of AD domain>
    use kerberos keytab = true
    password server = <Space-delimited list of AD DCs>
    ```

2. Use `kdestroy` to destroy any existing Kerberos credentials you may have; then run `kinit <Domain administrative account>@AD.DOMAIN.NAME` to get a Kerberos ticket for an account that is a domain administrator account.

3. Run `net ads join` to join the Linux server to Active Directory. This command will automatically create a computer object in Active Directory and add the appropriate SPNs (service principal names) to the computer object. In addition, it will populate the local Kerberos key table (`/etc/krb5.keytab`, by default) with the correct entries for authentication against Active Directory.

Only one small detail remains: how to deal with home directories for users logging into Linux systems.

## Deal with Home Directories

Unlike Windows systems, home directories are required on Linux-based systems. As a result, we _must_ provide home directories for each AD user that will log in to a Linux-based system. We basically have two options here:

* Use the `pam_mkhomedir.so` PAM module to automatically create local home directories "on the fly" as users log in. To do this, you would add an entry for `pam_mkhomedir.so` in the session portion of the PAM configuration file.

* Use the automounter to automatically mount home directories from a network server. This process is fairly complicated (too involved to include the information here), so I'll refer you to this article on [using NFS and automounts for home directories][3]. This has the added benefit of providing a foundation for unified home directories across both Windows and Linux systems.

(There is a third option as well: manually create home directories before users can log in.)

Once you've settled on and implemented a system for dealing with home directories, you are finished! UNIX-enabled users in Active Directory can now login to Linux-based systems with their Active Directory username and password.

What's not addressed in this article? Password management. In this configuration, users will most likely _not_ be able to change their password from the Linux servers and have that change properly reflected in Active Directory. I'll try to work on that for version 5 of the instructions.

I hope you find this information helpful. As always, feel free to post corrections, additions, or suggestions in the comments below.


[1]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[2]: {{< relref "2006-11-28-unix-attributes-tab-and-nispropdll.md" >}}
[3]: {{< relref "2006-11-21-greater-ad-integration-via-nfs-and-automounts.md" >}}
