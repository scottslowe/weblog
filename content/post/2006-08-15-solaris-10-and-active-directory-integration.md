---
author: slowe
categories: Tutorial
comments: true
date: 2006-08-15T17:47:12Z
slug: solaris-10-and-active-directory-integration
tags:
- ActiveDirectory
- Kerberos
- LDAP
- Solaris
- Sun
- UNIX
- Interoperability
title: Solaris 10 and Active Directory Integration
url: /2006/08/15/solaris-10-and-active-directory-integration/
wordpress_id: 320
---

As with the procedure for [authenticating Linux against Active Directory][1] and [providing Kerberos-based SSO with Apache][2], there are a few steps to be performed. Some of these steps are performed on the Active Directory side, some of them are performed on the [Solaris 10](http://www.sun.com/software/solaris/) system.

This procedure assumes that you are using [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/default.mspx); if you are using a previous version, the LDAP attribute mapping will need to be modified to match the schema extensions found in Microsoft's Services for Unix (SfU) add-on product.

### Preparing Active Directory (One-Time)

These steps only need to be performed once. Note that if you have performed any of these steps as part of authenticating Linux to Active Directory, they do _not_ need to be performed again. Simply make note of the information used earlier and re-use that information again with Solaris.

1. Install the "Server for NIS" component on at least one Active Directory domain controller (DC), so that the Active Directory schema can be extended to become partially [RFC 2307](http://www.ietf.org/rfc/rfc2307.txt)-compliant. Installing this component will also add a "UNIX Attributes" tab to objects inside the Active Directory Users and Computers MMC console. You may also need to install the Server for NIS administrative tools on your workstation to see the "UNIX Attributes" tab.

2. Use the Schema Management MMC snap-in to index the uid attribute, which is not indexed by default. This will speed up the login process and reduce the overall load on your DCs. (For more information, refer to my updated Linux-Windows Server 2003 R2 integration instructions, linked above.) It may be possible to change the attribute that Solaris is looking for, but I haven't found a way to do that yet.

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

1. Create a user account (**not** a computer account) for each Solaris server. (Note that this is different than the instructions provided for integrating Linux. I have an article planned to discuss this in more detail, but for now just trust me.) I highly suggest using a naming convention that supports a) the service principal(s) involved; and b) the name of the server. Since Solaris will use the HOST service principal, a name like "HOST-solarissrvr" would be good. The password doesn't matter, but do be sure to check the "Password never expires" check box, and after the account is created specify a good description so that you'll remember what this account is for in 6 months.

2. For each account that was created, run the ktpass.exe command to generate a unique keytab for each account. The command will look something like this (substitute the appropriate values where necessary):  

```text
ktpass -princ HOST/fqdn@REALM -mapuser DOMAIN\account
-crypto DES-CBC-MD5 +DesOnly -pass password -ptype KRB5_NT_PRINCIPAL
-out filename
```

Be sure to specify a unique output filename (so that you don't overwrite files; each server/account will needs its own unique file). I suggest using the server's name as the filename, i.e., something like `solarissrvr.keytab`.

Now that each Solaris server has a matching account in Active Directory, and each account has had a keytab generated for it, we're ready to move on to configuring the Solaris servers themselves.

### Configuring Solaris (Each Server)

The following steps need to be performed on each Solaris server that will authenticate against Active Directory.

#### Configuring Kerberos

Solaris keeps its Kerberos configuration in the `/etc/krb5` directory as `krb5.conf`. Edit this file using your editor of choice to look something like the one below. Depending upon how you configured Solaris during the installation, some of this configuration may already be present.

```text
[libdefaults]
   default_realm = EXAMPLE.COM
   dns_lookup_kdc = true
   verify_ap_req_nofail = false

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

Your particular information will need to be supplied here, of course, so you can't simply copy and paste from here to the Solaris configuration file.

Of particular interest here is the "verify\_ap\_req\_nofail = false" parameter. I'm still shaking out some TGT validation/verification errors, and this is currently the only way to make authentication work from Solaris. As soon as I get the validation/verification problems sorted out, I'll post a new version of these instructions.

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

I think it's necessary at this point to restart the LDAP client service:

    svcadm restart svc:/network/ldap/client:default

Use the `svcs -a | grep ldap` command to verify the exact name of the LDAP client service on your Solaris server.

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

### Testing the Configuration

Once all of the configuration steps have been completed, you can test the configuration with the following commands:

* You can use `getent passwd <Name of AD user>` from the Solaris server; this command should return UID number, GID number, UNIX home directory, and login shell.

* You can use `kinit <Name of AD user>` to test Kerberos authentication. A succesful Kerberos test will not return any feedback, and the `klist` command will show a ticket granting ticket (TGT) from the Active Directory DC/KDC.

If either of these tests are unsuccessful, review the log files on the Solaris server and resolve the problems before continuing. Both of these tests will need to be successful in order for authentication to work correctly.

If the tests are successful, then you should now be able to authenticate on a Solaris server using your Active Directory username and password. I tested this using SSH and the X Desktop login.

### How I Tested

I tested this configuration using Solaris 10 x86 6/06 (the June 2006 release) running as a VM under VMware ESX Server 3.0.0. Authentication was performed against a pair of virtual servers (one hosted on ESX 3.0.0, the other on ESX 2.5.3) running Windows Server 2003 R2, Standard Edition.

[1]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[2]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
