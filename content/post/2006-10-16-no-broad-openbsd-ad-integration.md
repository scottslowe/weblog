---
author: slowe
categories: Information
comments: true
date: 2006-10-16T17:04:54Z
slug: no-broad-openbsd-ad-integration
tags:
- ActiveDirectory
- BSD
- Interoperability
- Kerberos
- SSH
- UNIX
title: No Broad OpenBSD-AD Integration
url: /2006/10/16/no-broad-openbsd-ad-integration/
wordpress_id: 347
---

After doing some additional research on the authentication architecture for [OpenBSD](http://www.openbsd.org/), I learned that OpenBSD does not support PAM (Pluggable Authentication Mechanism), nor does OpenBSD support NSS (Name Switch Service). I found this particularly interesting, but not terribly surprising as the OpenBSD leaders have made it very clear that they won't include software that doesn't meet their stringent security and licensing requirements. I suppose that's a good thing, even if it does make certain tasks impossible.

In any case, I did find some veiled references to login\_ldap, which uses the underlying bsd\_auth mechanism employed by OpenBSD. Unfortunately (again), not all the software installed with OpenBSD supports bsd\_auth and therefore also doesn't support login\_ldap.

There is a bright spot here, though, and that's [OpenSSH](http://www.openssh.org/). OpenSSH supports native Kerberos authentication, i.e., passwordless authentication from a Kerberized SSH client to the OpenSSH daemon, which is itself Kerberized. I wrote about [passwordless Kerberos authentication for Linux and Solaris][1] a while ago; it turns out the process is almost identical for OpenBSD.

To enable native Kerberos authentication in OpenSSH, make sure the following commands are present in the `sshd_config` file (typically found at `/etc/ssh`):

    KerberosAuthentication yes
    GSSAPIAuthentication yes
    GSSAPICleanupCredentials yes

Be sure to restart the SSH daemon after making these changes.

Also, configure the `krb5.conf` file (found in OpenBSD at `/etc/kerberosV`--note the capitalization!) appropriately; refer to any of the Kerberos-related articles here for more information on the appropriate configuration. For this test, I also created a keytab (using `ktpass.exe`) and placed it in the `/etc/kerberosV` directory as well. I don't know for sure if that's required. As I have time, I'll do some additional testing and try to find out.

Because there is no NSS support in OpenBSD, you'll need to maintain accounts (but not passwords) in the local files. So, to test this, first be sure to create an account (using `useradd`), create the home directory, and assign appropriate permissions to the home directory. Otherwise, it won't work.

Once the configuration changes have been made, SSHd has been restarted, and a local account created, SSH connections from a Kerberos-enabled client (with a valid Kerberos ticket) should just work without any prompt for password.

Although this doesn't provide the broad integration with Active Directory that some may be seeking, it can at least help with SSH access to the OpenBSD systems, and that's better than nothing.

[1]: {{< relref "2006-08-21-native-kerberos-authentication-with-ssh.md" >}}
