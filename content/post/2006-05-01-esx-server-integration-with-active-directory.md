---
author: slowe
categories: Tutorial
comments: false
date: 2006-05-01T19:46:27Z
slug: esx-server-integration-with-active-directory
tags:
- ActiveDirectory
- ESX
- Kerberos
- VMware
- Windows
- Interoperability
title: ESX Server Integration with Active Directory
url: /2006/05/01/esx-server-integration-with-active-directory/
wordpress_id: 238
---

Having successfully mapped out the steps for Linux/Unix-based hosts to authenticate against Active Directory on Windows Server 2003 R2 (get the [complete details][1]), I now turned my sights toward integrating authentication on [ESX Server](http://www.vmware.com/products/esx/) 2.5.3 with Active Directory as well.

Using instructions found in [this technical white paper](http://www.vmware.com/pdf/esx_authentication_AD.pdf) from VMware's web site, I started out by modifying the `/etc/krb5.conf` file, which controls the operation of the Kerberos libraries in the Console Operating System (COS).  The contents of the `/etc/krb5.conf` file should look something like this:

```text
[logging]
 default = FILE:/var/log/krb5libs.log
 kdc = FILE:/var/log/krb5kdc.log
 admin_server = FILE:/var/log/kadmind.log

[libdefaults]
 ticket_lifetime = 24000
 default_realm = EXAMPLE.NET
 dns_lookup_realm = false
 dns_lookup_kdc = false

[realms]
 EXAMPLE.NET = {
  kdc = addc01.example.net:88
  admin_server = addc01.example.net:749
  default_domain = example.net
 }

[domain_realm]
 .example.net = EXAMPLE.NET
 example.net = EXAMPLE.NET

[kdc]
 profile = /var/kerberos/krb5kdc/kdc.conf

[pam]
 debug = false
 ticket_lifetime = 36000
 renew_lifetime = 36000
 forwardable = true
 krb4_convert = false
```

As you may already be aware, you can change the "dns_lookup_realm" and "dns_lookup_kdc" directives to true and omit the "[realms]" section, assuming that your Active Directory DNS infrastructure has the properly registered SRV records for the domain controllers.

The [VMware](http://www.vmware.com/) white paper then instructed to modify the `/var/kerberos/krb5kdc/kdc.conf` file, but I had never performed any edits to that file in my earlier experiments, so I decided to forgo that step. It is my understanding that this file controls the behavior of a Key Distribution Center (KDC), which the COS is not (in this instance, the KDC is the Active Directory domain controller).

Next, I edited the `vmware-authd` file in `/etc/pam.d`, which connects the vmware-authd daemon to the PAM modules for authentication. After the edits, the `/etc/pam.d/vmware-authd` file looked like this:

    #%PAM-1.0
    auth       sufficient   /lib/security/pam_unix_auth.so shadow nullok
    auth       required     /lib/security/pam_krb5.so use_first_pass
    account    required     /lib/security/pam_unix_acct.so

Finally, still following the instructions, I created a user account on the ESX Server (using the `useradd` command there in the COS) that matched the username of an account in Active Directory. At this point, I was ready to test connectivity.

(Those of you that have read the other articles on Kerberos/LDAP integration of Linux into Active Directory will note that I did not create a computer object, use `ktpass.exe` to create a keytab, nor did I configure `ldap.conf` with the attribute mapping. I can't explain why I don't need a computer object or a keytab yet---rest assured I will get the bottom of that---but the LDAP pieces aren't necessary because we are relying on the presence of the accounts locally and only using Kerberos for authentication. This has some advantages and some disadvantages, which I'll discuss in more detail later.)

The first test (performing by trying to log into the VMware ESX Server MUI using the account created above) failed; `/var/log/messages` indicated a problem with host resolution. That problem was easily resolved with a quick edit of `/etc/resolv.conf`.

The next test also failed; again, `/var/log/messages` held the answer: too great of a time skew between the ESX Server and the Active Directory domain controller. The `date` command fixed that right up, and we were ready to test again.

The third time was the charm. Using an account that existed locally (but for which no password had been set) as well as in Active Directory, I was able to log in to the MUI using the Active Directory password. A quick test with another Active Directory account that did _not_ have a matching local account failed (as expected), indicating that it was working as expected.

I learned a couple of useful tidbits from this experiment. First, it seems viable that organizations may wish to use Kerberos for authentication to their Linux-based hosts but _not_ use LDAP for account information, instead requiring that local accounts exist on each system (like in this situation). This bears the advantage that the organization has more granular control over which specific Linux/Unix hosts may be used (i.e., no logins will succeed if a local account does not exist); that granularity does not exist if using LDAP for account information. However, the corresponding disadvantage of this approach is that local accounts must be managed on each separate host.

Second, I learned that a computer object (and the whole `ktpass.exe` command to generate the keytab) may not be necessary; I'm going to go back and perform some additional testing to see if that is the case. More information will be posted here as soon as it is available.

[1]: {{< relref "2006-04-27-linux-ad-integration-with-windows-server-2003-r2.md" >}}
