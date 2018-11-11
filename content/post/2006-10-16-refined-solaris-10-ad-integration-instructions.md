---
author: slowe
categories: Tutorial
comments: true
date: 2006-10-16T11:38:00Z
slug: refined-solaris-10-ad-integration-instructions
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Solaris
- UNIX
title: Refined Solaris 10-AD Integration Instructions
url: /2006/10/16/refined-solaris-10-ad-integration-instructions/
wordpress_id: 346
---

The original instructions are found [here][1]. Note, however, that this post contains all the information from the original post plus a few added points found during the latest run through the steps.

### Assumptions

This procedure assumes that you are using [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx); if you are using a previous version, the LDAP attribute mapping will need to be modified to match the schema extensions found in Microsoft's Services for Unix (SfU) add-on product. This will require changes to the `ldapclient manual` command shown below, which handles the schema/attribute mapping.

### Preparing Active Directory (One-Time)

These steps only need to be performed once. Note that if you have performed any of these steps as part of authenticating Linux or Solaris to Active Directory, they do _not_ need to be performed again. Simply make note of the information used earlier and re-use that information again this time.

1. Install the "Server for NIS" component on at least one Active Directory domain controller (DC), so that the Active Directory schema can be extended to become partially [RFC 2307](http://www.ietf.org/rfc/rfc2307.txt)-compliant. Installing this component will also add a "UNIX Attributes" tab to objects inside the Active Directory Users and Computers MMC console. You may also need to install the Server for NIS administrative tools on your workstation to see the "UNIX Attributes" tab.

2. Use the Schema Management MMC snap-in to index the uid attribute, which is not indexed by default. This will speed up the login process and reduce the overall load on your DCs. (For more information, refer to the [Linux-Windows Server 2003 R2 integration instructions][2].) It may be possible to change the attribute that Solaris is looking for, but I haven't found a way to do that yet.

3. Create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine. I recommend giving this account a simple, short name; this will make specifying the DN of the account later easier to do.

4. Create a global security group in Active Directory Users & Computers and set the UNIX attributes for this group.

Once these one-time steps have been completed, we can proceed to configuring the individual users that will be authenticating to Active Directory from your Solaris server(s).

### Preparing Active Directory (Each User)

Each Active Directory account that will authenticate via Solaris must be configured with a uid and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account (this tab was made visible by the installation of the Server for NIS component). The attributes that must be populated are:

* NIS domain: It's required on this tab in order to populate the other fields, but we won't be using it.

* UID: This is actually the UID number. Each user must have a unique UID; I believe that the Server for NIS defaults at a starting UID of 10000, which is pretty safe for most systems.

* GID: In addition, each member must have a GID (group ID); simply specify the group that was created earlier.

* Login Shell: Specify a login shell (such as `/usr/bin/csh` or `/sbin/sh`) for each user.

* Home Directory: Specify the home directory (such as `/export/home/slowe`) that will be used for this user. Keep in mind that these values may apply across multiple systems and platforms, and the path must be valid on all systems and platforms.

Based on my experience so far, the values for Solaris will often be very different than what might be specified for Linux-based logins. I haven't yet figured out how to reconcile these differences in a multi-platform environment (suggestions are welcome).

After all the user accounts have been configured, then we are ready to perform the additional tasks within Active Directory and on the Solaris server(s) that will enable the authentication.

### Preparing Active Directory (Each Solaris Server)

These steps need to be repeated for each Solaris server that will authenticating via Kerberos to Active Directory.

1. Create a user account (**not** a computer account) for each Solaris server. (Review [this article][3] for more information on the reasoning behind this approach.) I highly suggest using a naming convention that supports a) the service principal(s) involved; and b) the name of the server. Since Solaris will use the host service principal, a name like "host-solarissrvr" would be good. The password doesn't matter, but do be sure to check the "Password never expires" check box, and after the account is created specify a good description so that you'll remember what this account is for in 6 months.

2. For each account that was created, run the `ktpass.exe` command to generate a unique keytab for each account. The command will look something like this (substitute the appropriate values where necessary):  

```text
ktpass -princ host/fqdn@REALM -mapuser DOMAIN\account
-crypto DES-CBC-MD5 +DesOnly -pass password -ptype KRB5_NT_PRINCIPAL
-out filename
```

Be sure to specify a unique output filename (so that you don't overwrite files; each server/account will needs its own unique file). I suggest using the server's name as the filename, i.e., something like `solarissrvr.keytab`.

Now that each Solaris server has a matching account in Active Directory, and each account has had a keytab generated for it, we're almost ready to move on to configuring the Solaris servers themselves. First, though, we need to take care of some name resolution issues.

### Configuring Reverse DNS

On the DNS server handling the reverse lookup zones for the subnet on which the Solaris server is located, add a PTR record for the Solaris server and it's IP address. This will ensure that reverse DNS lookups work as expected. Make sure that each Solaris server that will be authenticating against Active Directory has a reverse lookup record in DNS.

### Configuring Solaris (Each Server)

The following steps need to be performed on each Solaris server that will authenticate against Active Directory.

#### Configuring the hosts file

To enable reliable TGT validation (this ensures that the Kerberos ticket returned by a KDC actually came from the KDC and not a spoofed server), you'll need to edit the hosts file. On Solaris 10, this is found in `/etc/inet/hosts` and is read-only, even for root. Edit this file (changing permissions as necessary) so that the line with the server's IP address looks something like this:

    10.1.1.1        hostname.example.com hostname loghost

What we're doing here is making sure that the server's fully qualified domain name (not just the short hostname) is the first name entry on the line for the server's IP address.

There may or may not be other entries in the file; leave those entries untouched (unless you _know_ you need to modify them).

#### Configuring Kerberos

Solaris keeps its Kerberos configuration in the `/etc/krb5` directory as `krb5.conf`. Edit this file using your editor of choice to look something like the one below. Depending upon how you configured Solaris during the installation, some of this configuration may already be present.

```text
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

You can't simply copy and paste from here to the Solaris configuration file, as you'll need to customize your version for your particular network, hostnames, domain names, etc.

Transfer the keytab generated earlier by the `ktpass.exe` utility (in our example, it was called `solarissrvr.keytab`) to the Solaris server in some secure fashion, like SFTP or SCP. Place it in the `/etc/krb5` directory as `krb5.keytab`, and make sure that only root has permissions to the file.

#### Configuring LDAP

We'll use the native Solaris `ldapclient` utility to configure the LDAP support in Solaris. The command you'll type in looks something like this (please _don't_ copy and paste this, as it contains generic/incorrect information that won't work!):

```text
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

    hosts          files dns

This will help ensure that Solaris is properly configured to use DNS for host name resolution.

I think it's necessary at this point to restart the LDAP client service:
    
    svcadm restart svc:/network/ldap/client:default

Use the `svcs -a | grep ldap` command to verify the exact name of the LDAP client service on your Solaris server.

#### Configuring the DNS Client

You'll also need to make sure that the DNS client is enabled and running. Using `svcs -a | grep dns` will help you identify the correct service, which you can then enable with svcadm:

    svcadm enable svc:/network/dns/client:default

Test DNS resolution using either the `host` or `nslookup` commands.

#### Configuring PAM

The `/etc/pam.conf` file controls the PAM (Pluggable Authentication Mechanism) configuration on Solaris. You'll need to edit the `/etc/pam.conf` file to look something like what's shown below. I've edited away all the non-essential sections, so only change the sections listed below.

```text
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

#### Reboot the Solaris Server

I know this sounds stupid, but even after restarting LDAP and enabling/starting/restarting the DNS client, things still didn't work for me in the lab. However, after rebooting the Solaris server, it worked like a champ. So, just in case, reboot the Solaris server after completing the configuration.

### Testing the Configuration

Once all of the configuration steps have been completed, you can test the configuration with the following commands:

* You can use `getent passwd <Name of AD user>` from the Solaris server; this command should return UID number, GID number, UNIX home directory, and login shell.

* You can use `kinit <Name of AD user>` to test Kerberos authentication. A succesful Kerberos test will not return any feedback, and the `klist` command will show a ticket granting ticket (TGT) from the Active Directory DC/KDC.

If either of these tests are unsuccessful, review the log files on the Solaris server and resolve the problems before continuing. Both of these tests will need to be successful in order for authentication to work correctly.

If the tests are successful, then you should now be able to authenticate on a Solaris server using your Active Directory username and password. I tested this using SSH and the X Desktop login.

[1]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
[2]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[3]: {{< relref "2006-08-21-more-on-kerberos-authentication-against-active-directory.md" >}}
