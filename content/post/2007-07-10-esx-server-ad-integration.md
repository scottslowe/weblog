---
author: slowe
categories: Tutorial
comments: true
date: 2007-07-10T13:20:33Z
slug: esx-server-ad-integration
tags:
- ActiveDirectory
- ESX
- Interoperability
- Kerberos
- LDAP
- SSH
- Virtualization
- VMware
title: ESX Server-AD Integration
url: /2007/07/10/esx-server-ad-integration/
wordpress_id: 488
---

Although much of the administration of servers running [VMware ESX Server 3.0](http://www.vmware.com/products/vi/esx/) will occur in the Windows-based Virtual Infrastructure client connected to a [VirtualCenter](http://www.vmware.com/products/vi/vc/) server, there are times when it is quicker or easier to perform an administrative task directly on the ESX Server itself---either via the command-line interface (CLI) or via the VI client authenticating directly against the ESX Server. The problem with this is that, by default, administrators will have to use different credentials when connecting the VI client to ESX Server directly. In addition, these credentials must be managed separately from Active Directory, and separately on each individual ESX Server. As the number of ESX Servers in a farm grows, this can quickly become an administrative nightmare.

Fortunately, we can fairly easily integrate ESX authentication into Active Directory, so that the same account used in VirtualCenter is also used when logging in to the ESX Server directly. While VMware took the step of automating a portion of this process for us in the `esxcfg-auth` command, it only takes us part of the way.

Let's take a look at what part esxcfg-auth does accomplish for us, then look at how to accomplish the rest of the task.

## Using esxcfg-auth

The `esxcfg-auth` command will help automate a good portion of the work required to integrate ESX Server into Active Directory; specifically, it will automate the Kerberos configuration. To use `esxcfg-auth` to enable AD authentication, use the following command:

	esxcfg-auth --enablead --addomain=example.com --addc=dc1.example.com

Obviously, you'll want to substitute the appropriate values for "example.com" and "dc1.example.com" on the command above. So what does this command do, exactly? Here's the breakdown:

* Modifies the `/etc/krb5.conf` file to use example.com as the default Kerberos realm, and to use dc1.example.com as a KDC for that realm. In this same file, the domain "example.com" is mapped to the realm "EXAMPLE.COM" (keep in mind Kerberos realms are always specified in UPPERCASE).

* The `/etc/pam.d/system-auth` file is modified to use pam_krb5.so for Kerberos authentication.

What does this command _not_ do? Well, for one it doesn't configure the ESX Server for anything other than pure authentication. This means that although users will be forced to authenticate against Active Directory, ESX Server still expects to find the accounts defined in the local `/etc/passwd` file. This means that password management is centralized, but account management is still decentralized. (Some might see this as a security advantage, in that we must manually define an account in order to allow that account to log in to that server.)

To fully centralize account management, we'll need to step outside of the `esxcfg-auth` framework and get our hands dirty. Ready?

## Finishing esxcfg-auth's Work

To fully round out the authentication/account management configuration, Active Directory will have to be made aware of some UNIX-specific attributes. This means extending the schema. If you are running Windows 2000 or Windows Server 2003 pre-R2, this means installing Services for UNIX (SfU) 3.5; for Windows Server 2003 R2 or later, this means installing Identity Management for UNIX.

I'll refer you to this [article for Windows 2003 and Windows Server 2003 pre-R2][1] and this [article for Windows Server 2003 R2][2] or later for more information. (Because the ESX Server service console is based on Red Hat Enterprise Linux, these Linux-AD integration guides are very applicable here.)

Once Active Directory has been configured, the only tasks left for us to do are 1) to configure the nss_ldap libraries; 2) to configure `/etc/nsswitch.conf` to enable LDAP for naming services; and 3) verify time synchronization.

To configure the nss_ldap libraries, you'll need to modify the `/etc/ldap.conf` file as described in Step 5 of the "Prepare Each Linux Server" section of [this article][2] (assuming you are using Windows Server 2003 R2). This sets up the connection to Active Directory via LDAP and configures the attribute maps accordingly.

Please note that you'll need to create an account in Active Directory (a Domain Guest is fine) that the nss_ldap libraries may use for LDAP queries. You'll specify that account information when configuring `/etc/ldap.conf`.

Next, you'll need to configure `/etc/nsswitch.conf`. You only need to modify the passwd, group, and shadow lines, and you only need to add "ldap" at the end of the lines. The lines will end up looking something like this:

	group: files ldap  
	passwd: files ldap  
	shadow: files ldap

At this point, you should be able to test the nss_ldap configuration. Run `getent passwd <AD user account>` and you should get back information about that account's home directory, login shell, UID, and name. If you don't get back any information, go back and double-check your configuration.

To verify time synchronization, have a look at the NTP configuration found in `/etc/ntp.conf`. Make the changes you need here to be sure that both Active Directory (the forest root PDC emulator, specifically) and ESX Server are both synchronizing time and will stay in sync. Otherwise, the Kerberos authentication process will fail. Keep in mind that you may need to adjust the Service Console firewall using `esxcfg-firewall` in order to allow the appropriate traffic outbound. (Thanks to KentA for reminding me about this step!)

Once `getent passwd` is working as expected and time is in sync, then you should be able to SSH into the ESX Server with any appropriately configured AD account (i.e., any AD account that has the "UNIX Attributes" tab completed with valid information). This gives you a similar level of control over who is allowed to login and who isn't; accounts that don't have any UNIX attributes won't be able to authenticate and gain access to the ESX Servers.

In addition, you should be able to configure some level of access control as described [here][3].

## Summary

To summarize, the process for integrating ESX Server into Active Directory looks like this:

1. Use `esxcfg-auth` to set up the Kerberos authentication.

2. Extend the AD schema (if necessary) to include UNIX attributes such as login shell, UNIX home directory, UID, UID number, etc. This is accomplished in different ways depending upon the version of Windows in use.

3. Populate the UNIX attributes for those user accounts that should be allowed to access the ESX Server(s).

4. Configure `/etc/ldap.conf` on the ESX Server to configure LDAP connectivity back to Active Directory for name service lookups.

5. Configure `/etc/nsswitch.conf` to use LDAP for name service lookups.

6. Verify (and configure, if needed) time synchronization via NTP on the ESX Server.

7. Test the configuration using `getent passwd`, `getent group`, or both.

This configuration will centralize not only authentication for the ESX Servers but will also centralize account management in Active Directory as well.

Please feel free to add any corrections or suggestions for improvement in the comments below. Thanks!

[1]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[2]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[3]: {{< relref "2006-09-08-ldap-based-access-control.md" >}}
