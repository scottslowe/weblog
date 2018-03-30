---
author: slowe
categories: Tutorial
comments: true
date: 2007-07-09T12:13:03Z
slug: linux-ad-integration-with-windows-server-2008
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Samba
- Windows
title: Linux-AD Integration with Windows Server 2008
url: /2007/07/09/linux-ad-integration-with-windows-server-2008/
wordpress_id: 487
---

In the event that your organization is considering a migration later this year (or next?) to [Windows Server 2008](http://www.microsoft.com/windowsserver2008/default.mspx) (formerly "Longhorn"), here are some instructions for integrating Linux login requests against Active Directory on Windows Server 2008. These instructions are based on [Linux-AD Integration, Version 4][1] and utilize Kerberos, LDAP, and Samba.

When this process is complete, AD users can be enabled for use on Linux systems on the network and login to those Linux systems using the same username and password as throughout the rest of Active Directory.

If you are looking for information on using Linux with a previous version of Windows before Windows Server 2008, please refer back to my [AD integration index][3] and select the appropriate article. The only significant changes in the process involve the mapping of the LDAP attributes; otherwise, the procedure is very similar between the two versions of Windows.

## Preparing Active Directory (One-Time)

The process of installing and configuring Windows Server 2008 is beyond the scope of this article (although I may touch on that in the near future in a separate article). Therefore, I won't provide detailed instructions on how to perform some of these tasks, but instead provide a high-level overview.

### Enable Editing/Display of UNIX Attributes

In order to store UNIX attributes in Active Directory, the schema must be extended. To extend the schema, first install Active Directory (add the Active Directory Domain Services role to an installed server, then use the Active Directory Installation Wizard to setup Active Directory) and then add the "Identity Management for UNIX" role service (this can be done in Server Manager).

Once that role service has been installed, then the AD schema now includes a partially RFC 2307-compliant set of UNIX attributes, such as UID, UID number, GID number, login shell, etc. (Note that it may be that these attributes are already included in the schema for Windows Server 2008; I did not check the schema before installing the Identity Management for UNIX role service. With [Windows Server 2003 R2](http://www.microsoft.com/windowsserver/default.mspx), the schema was present at the time of installation, but the attributes were not visible until installing the UNIX identity services.)

At this point a new tab labeled "UNIX Attributes" will appear in the properties dialog box for users and groups in Active Directory. You'll use this tab to edit the UNIX-specific attributes that are required for logins to Linux-based systems.

### Create an LDAP Bind Account

You'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine. This helps minimize any potential security risks as a result of this account. Just be sure that you know the account's user principal name (UPN) and password.

## Prepare Active Directory (Each User)

Each Active Directory account that will authenticate via Linux must be configured with a UID and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account.

After all the user accounts have been configured, then we are ready to configure Active Directory objects for each of the Linux server(s) that we'll be integrating with AD.

## Prepare Active Directory (Each Server)

Prior to using Samba to join Linux computers to Active Directory and generate a keytab automatically, we had to use the `ktpass.exe` utility on Windows to generate a keytab. Due to some current [Samba-Windows Server 2008 interoperability issues][4], we can't use Samba. That means we'll be back to using `ktpass.exe` to map service principals onto accounts in Active Directory. Unfortunately, you'll need to first disable User Account Control (UAC) on your server, since [UAC interferes with `ktpass.exe`][5]. (Nice, huh?)

Once you've disabled UAC (and rebooted your server), then you can map the service principal names (SPNs) using the following procedure. First, create a compute account (or a user account; either will work) with the name of the Linux server.

Next, use the following command to map the needed SPN onto this account (backslashes indicate line continuation):

	ktpass.exe -princ HOST/server.fqdn@REALM.COM \  
	-mapuser DOMAIN\AccountName$ -crypto all \  
	-pass Password123 -ptype KRB5_NT_PRINCIPAL \  
	-out filename.keytab

Finally, copy the file generated by this command (`filename.keytab`) to the Linux server (using SCP or SFTP is a good option) and merge it with the existing keytab (if it exists) using `ktutil`. If there is no existing keytab, simply copy the file to `/etc/krb5.keytab` and you should be good to go.

Now that Active Directory has computer objects (and, more importantly, SPNs) for the Linux servers and the AD users have been enabled for UNIX (by populating the UNIX attributes), we're ready to start configuring the Linux server(s) directly.

## Prepare Each Linux Server

Follow the steps below to configure the Linux server for authentication against Active Directory. (Note that this configuration was tested on a system running CentOS---a variation of Red Hat Enterprise Linux---version 4.3.)

1. Edit the `/etc/hosts` file and ensure that the server's fully-qualified domain name is listed first after its IP address.

2. Make sure that the appropriate Kerberos libraries, OpenLDAP, pam_krb5, and nss_ldap are installed. If they are not installed, install them.

3. Be sure that time is being properly synchronized between Active Directory and the Linux server in question. Kerberos requires time synchronization. Configure the NTP daemon if necessary.

4. Edit the `/etc/krb5.conf` file to look something like [this](https://gist.github.com/scottslowe/67a3f8c36270c7e6376b), substituting your actual host names and domain names where appropriate.

5. Edit the `/etc/ldap.conf` file to look something like [this](https://gist.github.com/scottslowe/7ad13c8839a546b760df), substituting the appropriate host names, domain names, account names, and distinguished names (DNs) where appropriate.

6. Configure PAM (this varies according to Linux distributions) to use pam_krb5 for authentication. Many modern distributions use a stacking mechanism whereby one file can be modified and those changes will applied to all the various PAM-aware services. For example, in Red Hat-based distributions, the system-auth file is referenced by most other PAM-aware services. Click [here](https://gist.github.com/scottslowe/0e47e27dd5e515963daf) to see a properly edited `/etc/pam.d/system-auth` file taken from CentOS 4.4.

7. Edit the `/etc/nsswitch.conf file` to include "ldap" as a lookup source for passwd, shadow, and groups.

At this point we are now ready to test our configuration and, if successful, to perform the final step: to join the Linux server to Active Directory for authentication.

## Test the Configuration

To test the Kerberos authentication, use the `kinit` command, as in `kinit <AD username>@<AD domain DNS name>`. This should return no errors. A `klist` at that point should then show that you have retrieved a TGT (ticket granting ticket) from the AD domain controller. If this fails, go back and troubleshoot the Kerberos configuration. In particular, if you are seeing references to failed TGT validation, check to make sure that both your Linux servers and AD domain controllers have reverse lookup (PTR) records in DNS and that the Linux server's `/etc/hosts` file listed the FQDN of the server first instead of just the nodename.

&lt;aside&gt;Some readers and some other articles have suggested the use of the AD domain DNS name in the `/etc/krb5.conf` file instead of an AD domain controller specifically; I recommend against this. First, I believe it may contribute to TGT validation errors; second, it is possible to list multiple KDCs (AD DCs) in the configuration. Since the only major reason to use the AD domain DNS name instead of the DNS name of one or more DCs would be fault tolerance, then it doesn't really gain anything.&lt;/aside&gt;

To test the LDAP lookups, use the `getent` command, as in `getent passwd <AD username>`. This should return a listing of the account information from Active Directory. If this does not work, users will _not_ be able to login, even if Kerberos is working fine. If you run into errors or failures here, go back and double-check the LDAP configuration. One common source of errors is the name of the LDAP bind account, so be sure that is correct.

At this point, SSH logins to the Linux system using an account present in Active Directory (one which has had its UNIX attributes specified properly) should be successful. This will be true as long as you used the `ktpass.exe` command earlier to map the SPN onto the computer object in Active Directory. Even if you _didn't_ copy the keytab over to the Linux server, logins will work. Why? Because the PAM Kerberos configuration, by default, does not require a client keytab, and does not attempt to validate the tickets granted by the TGT. This means that as long as the SPN(s) are mapped to the accounts in AD, the keytab is not necessarily required.

(Note, however, that not using a keytab and/or not requiring a keytab does leave the Linux server open to potentially spoofed Kerberos tickets from a fake KDC. In addition, "native" Kerberos authentication---i.e., using a Kerberos ticket to authenticate instead of typing in a password---won't work without a keytab.)

## Deal with Home Directories

Unlike Windows systems, home directories are required on Linux-based systems. As a result, we _must_ provide home directories for each AD user that will log in to a Linux-based system. We basically have three options here:

* Manually create home directories and set ownership/permissions properly before users will be able to log in.

* Use the pam\_mkhomedir.so PAM module to automatically create local home directories "on the fly" as users log in. To do this, you would add an entry for pam\_mkhomedir.so in the session portion of the PAM configuration file.

* Use the automounter to automatically mount home directories from a network server. This process is fairly complicated (too involved to include the information here), so I'll refer you to this article on [using NFS and automounts for home directories][2]. This has the added benefit of providing a foundation for unified home directories across both Windows and Linux systems.

Once you've settled on and implemented a system for dealing with home directories, you are finished! UNIX-enabled users in Active Directory can now login to Linux-based systems with their Active Directory username and password.

What's not addressed in this article? Password management. In this configuration, users will most likely _not_ be able to change their password from the Linux servers and have that change properly reflected in Active Directory. In addition, "native" Kerberos authentication using Kerberos tickets won't work unless the keytab is present. In my testing, I ran into a number of issues with the keytab and TGT validation, but I'm not sure if those are errors in my process or the result of the beta status of Windows Server 2008.

I welcome your corrections, additions, or suggestions in the comments below.


[1]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[2]: {{< relref "2006-11-21-greater-ad-integration-via-nfs-and-automounts.md" >}}
[3]: {{< relref "2007-01-15-active-directory-integration-index.md" >}}
[4]: {{< relref "2007-07-09-samba-and-windows-server-2008-interoperability.md" >}}
[5]: {{< relref "2007-07-09-uac-and-ktpassexe.md" >}}
