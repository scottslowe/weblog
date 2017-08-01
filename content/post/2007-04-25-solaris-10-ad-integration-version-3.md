---
author: slowe
categories: Tutorial
comments: true
date: 2007-04-25T14:54:43Z
slug: solaris-10-ad-integration-version-3
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Microsoft
- Solaris
- SSH
- UNIX
title: Solaris 10-AD Integration, Version 3
url: /2007/04/25/solaris-10-ad-integration-version-3/
wordpress_id: 447
---

Thanks to some very helpful individuals in the #solaris channel on irc.freenode.net, I've been able to get ADS support working in [Samba](http://www.samba.org/) on [Solaris 10](http://www.sun.com/software/solaris/), and thus have been able to incorporate the use of Samba in the Solaris 10-AD integration instructions.

To refer to earlier versions of the Solaris 10-AD integration instructions, see [this article][1] or [this article][2]. I would expect that you won't need to refer to those posts, though, and will be able to get most of what you need directly from this post.

## Assumptions

This procedure assumes that you are using [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx); if you are using a previous version, the LDAP attribute mapping will need to be modified to match the schema extensions found in Microsoft's Services for Unix (SfU) add-on product. This will require changes to the `ldapclient manual` command shown below, which handles the schema/attribute mapping. (I only have a single article written that includes pre-R2 attribute mapping, and that's [this Linux-AD article][3]. The schema mapping should be very, very similar between that article and Solaris 10.)

## Preparing Active Directory (One-Time)

These steps only need to be performed once. Note that if you have performed any of these steps as part of authenticating Linux or Solaris to Active Directory, they do _not_ need to be performed again. Simply make note of the information used earlier and re-use that information again this time.

1. Install the "Server for NIS" component on at least one Active Directory domain controller (DC), so that the Active Directory schema can be extended to become partially [RFC 2307](http://www.ietf.org/rfc/rfc2307.txt)-compliant. Installing this component will also add a "UNIX Attributes" tab to objects inside the Active Directory Users and Computers MMC console. You may also need to install the Server for NIS administrative tools on your workstation to see the "UNIX Attributes" tab.

2. Use the Schema Management MMC snap-in to index the uid attribute, which is not indexed by default. This will speed up the login process and reduce the overall load on your DCs. (For more information, refer to the [Linux-Windows Server 2003 R2 integration instructions][4].) It may be possible to change the attribute that Solaris is looking for, but I haven't found a way to do that yet.

3. Create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine. I recommend giving this account a simple, short name; this will make specifying the DN of the account later easier to do.

4. Create a global security group in Active Directory Users & Computers and set the UNIX attributes for this group.

Once these one-time steps have been completed, we can proceed to configuring the individual users that will be authenticating to Active Directory from your Solaris server(s).

## Preparing Active Directory (Each User)

Each Active Directory account that will authenticate via Solaris must be configured with a uid and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account (this tab was made visible by the installation of the Server for NIS component). The attributes that must be populated are:

* NIS domain: It's required on this tab in order to populate the other fields, but we won't be using it.

* UID: This is actually the UID number. Each user must have a unique UID; I believe that the Server for NIS defaults at a starting UID of 10000, which is pretty safe for most systems.

* GID: In addition, each member must have a GID (group ID); simply specify the group that was created earlier.

* Login Shell: Specify a login shell (such as `/usr/bin/csh` or `/sbin/sh`) for each user.

* Home Directory: Specify the home directory (such as `/export/home/slowe`) that will be used for this user. Keep in mind that these values may apply across multiple systems and platforms, and the path must be valid on all systems and platforms.

Based on my experience so far, the values for Solaris will often be very different than what might be specified for Linux-based logins. The only workarounds I've found to address these issues is the clever use of symlinks and the use of [NFS automounts for home directories][5].

After all the user accounts have been configured, then we are ready to perform the additional tasks within Active Directory and on the Solaris server(s) that will enable the authentication.

## Configuring Reverse DNS

On the DNS server handling the reverse lookup zones for the subnet on which the Solaris server is located, add a PTR record for the Solaris server and it's IP address. This will ensure that reverse DNS lookups work as expected. Make sure that each Solaris server that will be authenticating against Active Directory has a reverse lookup record in DNS, and ensure that both forward and reverse lookups work from each of the Solaris server(s).

## Configuring Solaris (Each Server)

The following steps need to be performed on each Solaris server that will authenticate against Active Directory.

### Configuring the hosts file

To enable reliable TGT validation (this ensures that the Kerberos ticket returned by a KDC actually came from the KDC and not a spoofed server), you'll need to edit the hosts file. On Solaris 10, this is found in `/etc/inet/hosts` and is read-only, even for root. Edit this file (changing permissions as necessary) so that the line with the server's IP address looks something like this:

	10.1.1.1 hostname.example.com hostname loghost

What we're doing here is making sure that the server's fully qualified domain name (not just the short hostname) is the first name entry on the line for the server's IP address.

There may or may not be other entries in the file; leave those entries untouched (unless you _know_ you need to modify them).

### Installing Blastwave Packages

_This_ is the key to getting ADS support into Samba on Solaris 10. I won't go into excruciating detail on this since this process is amply covered elsewhere, but here's the basic idea of the process:

* Use the standard `wget` (found in `/usr/sfw/bin`) to download the `pkg-get` file used by Blastwave.

* Use `pkgadd` to install `pkg-get`.

* Configure `pkg-get` to use the unstable packages (makes sure you get the latest builds).

* Use `pkg-get` to install the CSWsamba package and all requisite packages (there were quite a few dependency packages during my testing).

Once the CSWsamba package and related packages are installed, we'll need to configure Samba by creating `/opt/csw/etc/samba/smb.conf` with the following contents:

``` text
workgroup = <NetBIOS name of AD domain> 
security = ads
realm = <DNS name of AD domain>
use kerberos keytab = true
password server = <Space-delimited list of AD DCs>
```

At this point, we are ready to configure Kerberos and then proceed with testing the configuration and join the Active Directory domain.

### Configuring Kerberos

Solaris keeps its Kerberos configuration in the `/etc/krb5` directory as `krb5.conf`. Edit this file using your editor of choice to look something like the one below. Depending upon how you configured Solaris during the installation, some of this configuration may already be present.

``` ini
[libdefaults]
        default_realm = EXAMPLE.COM
        dns_lookup_kdc = true

[realms]
        EXAMPLE.COM = {
        kdc = dc01.example.com
        kdc = dc02.example.com
        admin_server = dc01.example.com
        }

[domain_realm]
        .example.com = EXAMPLE.COM
        .subdomain.example.com = EXAMPLE.COM

[logging]
        default = FILE:/var/krb5/kdc.log
        kdc = FILE:/var/krb5/kdc.log
        kdc_rotate = {
        period = 1d
        version = 10
        }

[appdefaults]
        kinit = {
        renewable = true
        forwardable= true
        }
```

There will also be a file named `cswkrb5.conf` in the `/etc` directory; you can configure this file with the contents of the [libdefaults], [realms], and [domain_realms] sections as listed above. You don't need to include the [logging] or [appdefaults] sections in this file.

Note that you can't simply copy and paste from here to the Solaris configuration files, as you'll need to customize your version for your particular network, hostnames, domain names, etc. If you must copy and paste from here, put it into a text editor first to customize it for your implementation.

### Configuring LDAP

We'll use the native Solaris `ldapclient` utility to configure the LDAP support in Solaris. The command you'll type in looks something like this (please _don't_ copy and paste this, as it contains generic/incorrect information that won't work!):

``` text
ldapclient manual \
-a credentialLevel=proxy \
-a authenticationMethod=simple \
-a proxyDN=cn=proxyuser,cn=Users,dc=example,dc=com \
-a proxyPassword=Password1 \
-a defaultSearchBase=dc=example,dc=com \
-a domainName=example.com \
-a "defaultServerList=172.16.1.10" \
-a attributeMap=group:userpassword=userPassword \
-a attributeMap=group:memberuid=memberUid \
-a attributeMap=group:gidnumber=gidNumber \
-a attributeMap=passwd:gecos=cn \
-a attributeMap=passwd:gidnumber=gidNumber \
-a attributeMap=passwd:uidnumber=uidNumber \
-a attributeMap=passwd:homedirectory=unixHomeDirectory \
-a attributeMap=passwd:loginshell=loginShell \
-a attributeMap=shadow:shadowflag=shadowFlag \
-a attributeMap=shadow:userpassword=userPassword \
-a objectClassMap=group:posixGroup=group \
-a objectClassMap=passwd:posixAccount=user \
-a objectClassMap=shadow:shadowAccount=user \
-a serviceSearchDescriptor=passwd:dc=example,dc=com?sub \
-a serviceSearchDescriptor=group:dc=example,dc=com?sub
```

The easiest way to handle this would probably be to copy it into a blank text file, edit it to include the specific details for your network, and then paste it into a terminal session on the Solaris server.

After this command has been run, Solaris will create the LDAP configuration in `/var/ldap` and will update `/etc/nsswitch.conf` to use LDAP. However, because we only want to use LDAP for specific purposes, we'll need to go back and edit `/etc/nsswitch.conf` again. Just remove "ldap" from all entries in `/etc/nsswitch.conf` except for passwd and group.

While you're editing `/etc/nsswitch.conf`, be sure to add a "dns" entry at the end of the line for hosts:

	hosts files dns

This will help ensure that Solaris is properly configured to use DNS for host name resolution.

I think it's necessary at this point to restart the LDAP client service:

	svcadm restart svc:/network/ldap/client:default

Use the `svcs -a | grep ldap` command to verify the exact name of the LDAP client service on your Solaris server.

### Configuring the DNS Client

You'll also need to make sure that the DNS client is enabled and running. Using `svcs -a | grep dns` will help you identify the correct service, which you can then enable with svcadm:

	svcadm enable svc:/network/dns/client:default

Test DNS resolution using the `nslookup` command. As mentioned previously, be sure to test both forward and reverse lookups.

### Configuring PAM

The `/etc/pam.conf` file controls the PAM (Pluggable Authentication Mechanism) configuration on Solaris. You'll need to edit the `/etc/pam.conf` file to look something like what's shown below. I've edited away all the non-essential sections, so only change the sections listed below.

``` text
# Default definition for Authentication management
#
other   auth requisite          pam_authtok_get.so.1
other   auth required           pam_dhkeys.so.1
other   auth sufficient         pam_krb5.so.1
other   auth required           pam_unix_cred.so.1
other   auth required           pam_unix_auth.so.1
#
# Default definition for Account management
#
other   account requisite       pam_roles.so.1
other   account sufficient      pam_unix_account.so.1
other   account required        pam_ldap.so.1
#
```

With this configuration in place, Solaris will use Kerberos authentication and will retrieve account information via LDAP.

### Reboot the Solaris Server

I know this sounds stupid, but even after restarting LDAP and enabling/starting/restarting the DNS client, things still didn't work for me in the lab. However, after rebooting the Solaris server, it worked like a champ. So, just in case, reboot the Solaris server after completing the configuration.

## Testing the Configuration

Once all of the configuration steps have been completed, you can test the configuration with the following commands:

* You can use `getent passwd <Name of AD user>` from the Solaris server; this command should return UID number, GID number, UNIX home directory, and login shell.

* You can use `kinit <Name of AD user>` to test Kerberos authentication. A succesful Kerberos test will not return any feedback, and the `klist` command will show a ticket granting ticket (TGT) from the Active Directory DC/KDC.

If either of these tests are unsuccessful, review the log files on the Solaris server and resolve the problems before continuing. Both of these tests will need to be successful in order for authentication to work correctly.

If the tests are successful, then you should now be able to join the Solaris server to Active Directory using Samba.

## Joining the Solaris Server to Active Directory

This is the final step. Don't try this step until you've successfully tested the configuration. After this step is completed, you are finished and AD users will be able to login to the Solaris server (assuming the AD users have been properly configured).

To join the Solaris server to Active Directory, follow these steps:

1. Verify the Samba configuration as outlined earlier. Key to the configuration are the "security = ads" and "use kerberos keytab = true" directives.

2. Use `kdestroy` to destroy any existing Kerberos credentials you may have; then run `kinit <Domain administrative account>@AD.DOMAIN.NAME` to get a Kerberos ticket for an account that is a domain administrator account.

3. Run `net ads join` to join the Solaris server to Active Directory. This command will automatically create a computer object in Active Directory and add the appropriate SPNs (service principal names) to the computer object. In addition, it will populate the local Kerberos key table (`/etc/krb5.keytab`, by default) with the correct entries for authentication against Active Directory. You may see an error about a missing userPrincipalName, but this does not appear to affect any functionality.

At this point, all properly configured AD users (those users who have UNIX attributes) should be able to login to the Solaris server using their Active Directory username and password. Of course, this assumes that you've already dealt with home directories (or are automounting home directories).

As with previous instructions, these instructions don't address password management (the ability to change an AD password from Solaris) and don't address how to handle home directories. Hey, I've got to leave a few challenges for others to tackle, right?

## How I Tested

This testing was done using Solaris 10 11/06 (Update 3) running on VMware ESX Server 3.0.1. Active Directory was running on a pair of servers with Windows Server 2003 R2, also virtual machines on ESX Server. Authentication testing was performed using SSH from a client system running Mac OS X.


[1]: {{< relref "2006-10-16-refined-solaris-10-ad-integration-instructions.md" >}}
[2]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
[3]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[4]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[5]: {{< relref "2006-11-21-greater-ad-integration-via-nfs-and-automounts.md" >}}
