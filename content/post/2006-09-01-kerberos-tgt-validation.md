---
author: slowe
categories: Explanation
comments: true
date: 2006-09-01T17:26:23Z
slug: kerberos-tgt-validation
tags:
- ActiveDirectory
- CentOS
- Interoperability
- Kerberos
- Linux
- Microsoft
- Solaris
- Windows
title: Kerberos TGT Validation
url: /2006/09/01/kerberos-tgt-validation/
wordpress_id: 329
---

I performed some testing with both [CentOS](http://www.centos.org/) 4.3 and [Solaris](http://www.sun.com/software/solaris/) 10, two of the platforms for which I've penned instructions on how to integrate authentication with Active Directory (using Kerberos and LDAP). I was hoping that I would see the same behavior on both platforms, but my testing showed otherwise.

First, let's recap briefly. Unless the setting "verify_ap_req_nofail = false" is in the `krb5.conf` file (in the `/etc` directory on CentOS, in the `/etc/krb5` directory on Solaris), the Kerberos libraries will attempt to perform TGT validation. The purpose behind TGT validation is to ensure that the KDC is indeed who it claims to be; this prevents spoofing of the KDC and helps protect the KDC's status as a "trusted third party" to the authentication process.

As a number of people have begun attempting to use the instructions that I created ([here are the Linux instructions][1] and [here are the Solaris instructions][2]), TGT validation has come up a number of times as something that users are having a hard time with, and thus far I have been unable to offer any real guidance. Hopefully, with this testing, I will have gotten closer to a concrete answer regarding TGT validation.

There are a number of things that you should check when running into TGT validation problems:

* First, ensure that the key version number (kvno) for the key table on the Linux server matches what is listed in Active Directory. The msDS-KeyVersionNumber attribute in Active Directory shows the kvno; the `klist -k` command on Linux/Solaris will show that information for the key table entries. I have seen references, but have not yet had the opportunity to personally test, that not specifying the password on the `ktpass.exe` command line (when generating the keytab entries) causes a mismatch between the keytab entry and Active Directory.

* There is a hotfix available from Microsoft for Windows Server 2003 SP1 and later that addresses a problem with generating key table entries for computer accounts. This [Microsoft support article](http://support.microsoft.com/kb/919557/EN-US/) describes the problem and the hotfix. Either use this hotfix, or switch to using user accounts instead of computer accounts.

* Make sure your key table entries are generated with the proper case. I initially suspected that some parts of the SPN may need to be uppercase (aside from the REALM, which is _always_ uppercase), but I think that is only the case when using GSSAPI/SPNEGO authentication with IIS or Apache, as described [here][3]. In that specific case, the "HTTP" must be uppercase. In all other instances of the Kerberos authentication I've tested, using lowercase is fine. In fact, the OpenSSH client (at least, version 4.2p1 on Mac OS X 10.4.7) will automatically request a ticket with a lowercase SPN if GSSAPI authentication is enabled. (This would be enabled if you were trying to achieve [passwordless, Kerberized SSH logins][4].)

In some instances, however, people have tried all these things and _still_ get TGT validation errors. What then?

Strangely enough, check the `/etc/hosts` file.  Your `/etc/hosts` file should look something like this:

    #
    # Internet host table
    #
    127.0.0.1       localhost
    192.168.2.109   vserver01.example.net vserver01

If your `/etc/hosts` file looks like this instead, you'll need to fix the order of the hostnames:

    #
    # Internet host table
    #
    127.0.0.1       localhost
    192.168.2.109   vserver01 vserver01.example.net

See the difference? It's in the order of the names for the server's IP address. Yes, the order of the names---single label hostname vs. fully qualified domain name---will make a difference. On CentOS, switching the names so that the single label hostname is first causes passwordless Kerberized SSH logins to fail, presumably due to TGT validation problems. (Due to time constraints, I haven't been able to dial up the logging on the CentOS server to verify this for certain, but I will soon, and will post an update to this article with those findings.)

I was hoping that Solaris would show the same results, since my first foray with Kerberos integration with Solaris showed some TGT validation problems that later mysteriously and unexplicably went away. However, Solaris doesn't seem to care about the order of the names in the `/etc/hosts` file, so this isn't the missing link I was hoping it would be.

Hopefully, this new information will help some of you out there that are still struggling with TGT validation. If anyone else has found any tricks or useful tips, please be sure to post them in the comments.

[1]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[2]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
[3]: {{< relref "2006-08-10-kerberos-based-sso-with-apache.md" >}}
[4]: {{< relref "2006-08-21-native-kerberos-authentication-with-ssh.md" >}}
