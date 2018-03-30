---
author: slowe
categories: Tutorial
comments: true
date: 2006-08-08T11:50:09Z
slug: linux-active-directory-and-windows-server-2003-r2-revisited
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- LDAP
- Linux
- Security
- Windows
title: Linux, Active Directory, and Windows Server 2003 R2 Revisited
url: /2006/08/08/linux-active-directory-and-windows-server-2003-r2-revisited/
wordpress_id: 315
---

**UPDATE:** A revised version of these instructions is available [here][5].

The integration of (what was formerly called) Services for UNIX into [Windows Server 2003 R2](http://www.microsoft.com/windowsserver2003/) also brought some other changes; most notably, a change in the schema. To accommodate those changes, I've updated my Linux-AD integration instructions (the [previous instructions are here][1] for pre-R2 versions of Windows). If you need to integrate Linux systems for authentication into Active Directory with Windows Server 2003 R2, these instructions should get you there. (Note that a [previous version][2] of these instructions is also available.)

For the most part, these instructions are reasonably similar to the instructions for pre-R2 versions of Windows.

### Preparing Active Directory (One-Time)

Based on what I've seen so far, it appears as if a partial RFC 2307-compliant schema is included by default with Windows Server 2003 R2. This means that it is no longer necessary to extend the schema to include attributes such as uid, gid, login shell, etc. However, while the schema does appear to be present by default (based on explorations using ADSI Edit), you must install the "Server for NIS" component on at least one domain controller in order to be able to actually set those attributes (and it will be necessary to set those attributes using the Active Directory Users and Computers console before logins from Linux will work).

However, to optimize Active Directory logins from Linux systems, it's also necessary to index the uid attribute in Active Directory. By default, most PAM-enabled systems use the uid attribute as the default login attribute (refer to the "pam\_login\_attribute" parameter in the `/etc/ldap.conf` file). Logins will work without having this attribute indexed, but as was discovered in a [recent VAS installation][3], this can introduce delays and drive CPU utilization through the roof. Use the Schema Management MMC snap-in to check the box labeled "Index this attribute in the Active Directory" for the uid attribute. (If you don't want to index the uid attribute, change the value of the pam\_login\_attribute to something like sAMAccountName, which is already indexed.)

Next, create a new global security group that will act as the default group for Linux-enabled users. Be sure to set the values on the "UNIX Attributes" tab for this group. Add the users that will authenticate to this group using both the "Members" tab and the members list on the "UNIX Attributes" tab.

Finally, you'll also need to create an account in Active Directory that will be used to bind to Active Directory for LDAP queries. This account does _not_ need any special privileges; in fact, making the account a member of Domain Guests and _not_ a member of Domain Users is perfectly fine.

Each of these tasks are one-time tasks that must be accomplished before logins from Linux will work. Once they have been completed, you are ready to configure the individual users.

### Preparing Active Directory (Each User)

Each Active Directory account that will authenticate via Linux must be configured with a uid and other UNIX attributes. This is accomplished via the new "UNIX Attributes" tab on the properties dialog box of a user account. Installing the "Server for NIS" component enables this new tab, as mentioned previously.

Each user must be given an NIS domain, but this parameter is ignored in our authentication scheme. Each user must also have a unique uid; I believe that the Server for NIS defaults at a starting uid of 10000, which is pretty safe for most systems. In addition, each member must have a gid (group ID); simply specify the group that was created earlier. Be sure to also specify a login shell (such as `/bin/bash`) and a home directory (such as `/home/slowe`).

After all the user accounts have been configured, then we are ready to perform the additional tasks within Active Directory and on the Linux server that will enable the authentication.

### Preparing Active Directory (Each Linux Server)

Here is where it starts getting tricky. So far, nothing we've done has been unusual or terribly difficult. Things will start getting a bit more complex now.

First off, you'll need to decide if you want to use _TGT validation._ I don't have the space here to fully describe this, but basically it's a check that the Kerberos Key Distribution Center (KDC---in this case, an Active Directory domain controller) is not being spoofed. It's an added level of security that ensures that all hosts involved are indeed who they say they are, which is one of the core principles of the Kerberos authentication system.

#### Without TGT Validation

If you don't care about TGT validation, then ignore this whole section and proceed to "Preparing Each Linux Server", below. Once Linux is properly configured for Kerberos authentication and LDAP lookups, it can authenticate against Active Directory _with no further action required._ You'll note that this is in contrast to many of the instructions out there (including my [original instructions][2]), which state that you must perform additional steps. In my experience, the additional steps are only necessary if you want TGT validation, i.e., if you want the Linux server to verify the identity of the Active Directory domain controller handing out the Kerberos tickets. If you don't care about that, then you're ready to proceed with the next step.

#### With TGT Validation

For each Linux-based server that will be authenticating against Active Directory, follow the steps below.

1. Create a computer account in Active Directory. When creating the computer account, be sure to specify that this account may be used by a pre-Windows 2000-based computer.

2. Use the following command at a command prompt to configure the new computer account:  

``` text
ktpass -princ HOST/fqdn@REALM -mapuser DOMAIN\name$
-crypto DES-CBC-MD5 +DesOnly -pass password -ptype KRB5_NT_SRV_HST
-out filename
```

Of course, you'll need to substitute the appropriate values for "fqdn" (the fully-qualified domain name of the computer), "REALM" (the DNS name of your Active Directory domain _in UPPERCASE_), "DOMAIN" (the NetBIOS name of your Active Directory domain), "name$" (the name of the computer account created, with a dollar sign appended at the end), "password" (the password that will be set for the new computer account), and "filename" (the keytab that will be generated and must be copied over to the Linux computer). Please note (and this is **important**) that the "HOST/fqdn@REALM" portion is _case-sensitive_ and should be typed as shown above. Of course, if you are repeating this process for multiple servers, please be sure to use a unique filename for each keytab generated using `ktpass.exe`. (I use each Linux server's hostname as the filename.)

If this computer account ever gets deleted from Active Directory, then Active Directory users will be unable to authenticate to Linux systems. You'll need to repeat the process---create a new computer account, run `ktpass.exe`, and copy the keytab over to the Linux server (as described below).

### Preparing Each Linux Server

Follow the steps below to configure each Linux server for authentication against Active Directory.

1. Make sure that the appropriate Kerberos libraries, OpenLDAP, pam_krb5, and nss_ldap are installed. If they are not installed, install them.

2. Be sure that time is being properly synchronized between Active Directory and the Linux server in question. Kerberos requires time synchronization. Set up NTP if necessary.

3. Edit the `krb5.conf` file to look something like [this code snippet](https://gist.github.com/scottslowe/2263f0a95bd9e18e0d4f). Note that the line "validate =" should be set to true if you want TGT validation; otherwise, set it to false. Note also that we've commented out the `[realms]` section because we are using DNS to locate the KDCs ("dns\_lookup\_kdc = true"); this requires the presence of the appropriate SRV records in DNS. In a correctly-functioning Active Directory environment, these records will be present. Substitute your actual host names and domain names where appropriate.

4. Edit the `/etc/ldap.conf` file to look something like [this](https://gist.github.com/scottslowe/a7c89505c46a13d95ebe), substituting the appropriate host names, domain names, account names, and distinguished names (DNs) where appropriate.

5. Securely copy the file generated by the `ktpass.exe` command above to the Linux server. You can replace the existing `/etc/krb5.keytab` file _if and only if_ you do not need any of the existing keys stored there. If you haven't put any keys in there, then you probably don't have any and don't need to worry about using `ktutil` to merge the new keys (from the file generated by `ktpass.exe`) with the existing keys. If, however, you _do_ have existing keys you need to maintain, be sure to use `ktutil` to merge/append the new keys to the existing keytab.

6. Configure PAM (this varies according to Linux distributions) to use pam\_krb5 for authentication. Many modern distributions use a stacking mechanism whereby one file can be modified and those changes will applied to all the various PAM-aware services. For example, in Red Hat-based distributions, the `system-auth` file is referenced by most other PAM-aware services. A `system-auth` file that would be found in `/etc/pam.d` might look something like [this sample](https://gist.github.com/scottslowe/0e47e27dd5e515963daf) taken from [CentOS](http://www.centos.org/) 4.3, with a few modifications. Remember that in [Red Hat](http://www.redhat.com/)-based distributions, such as CentOS, running the authconfig program will overwrite all the changes to `/etc/pam.d/system-auth`, so _be careful._

7. Edit the `/etc/nsswitch.conf` file to include "ldap" as a lookup source for passwd, shadow, and groups.

That should be it. Once you do that, you should be able to use `kinit` from a Linux shell prompt (for example, `kinit aduser`) and generate a valid Kerberos ticket for the specified Active Directory account.

At this point, any PAM-aware service that is configured to use the stacked system file (such as the system-auth configuration on Red Hat-based distributions) will use Active Directory for authentication. The SSH daemon is a good one to test. Note, however, that unless you also add the pam\_mkhomedir.so module in the PAM configuration, home directories will have to be created manually (with the correct permissions and ownership set manually as well) for any Active Directory account that may log on to that server. (I generally recommend the use of pam\_mkhomedir.so in this situation.)

### Caveats/Limitations/Disclaimers

I haven't tested this configuration on every possible distribution of Linux. This configuration was tested on CentOS 4.3 running as a virtual machine under ESX Server 3.0, authenticating against a pair of domain controllers running Windows Server 2003 R2 (which were also VMs). It should work without major modifications on most other Linux distributions, and with modifications on various other Unix operating systems. (I plan to test [OpenBSD](http://www.openbsd.org/) 3.9 and possibly Solaris 10 x86 soon.)

Also, even though the "validate = true" setting in `/etc/krb5.conf` implies that the Kerberos TGT must be validated, pam_krb5 appears to bypass the TGT validation if the keytab is not present or not readable. This means that logins will succeed, even if the keytab is not present or not readable. If the computer account in Active Directory is missing, however, logins will fail. I know it's odd; the only possible explanation I can offer is described in a [follow-up posting regarding ESX-AD integration][4].

If anyone finds any errors, discrepancies, or inaccuracies in this article, please let me know and I'll correct them as soon as possible.


[1]: {{< relref "2005-12-22-complete-linux-ad-authentication-details.md" >}}
[2]: {{< relref "2006-04-27-linux-ad-integration-with-windows-server-2003-r2.md" >}}
[3]: {{< relref "2006-07-28-active-directory-and-vas.md" >}}
[4]: {{< relref "2006-05-16-follow-up-on-esx-server-integration.md" >}}
[5]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}