---
author: slowe
categories: Tutorial
comments: true
date: 2006-08-21T16:15:01Z
slug: native-kerberos-authentication-with-ssh
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- Linux
- Networking
- Solaris
- SSH
- UNIX
title: Native Kerberos Authentication with SSH
url: /2006/08/21/native-kerberos-authentication-with-ssh/
wordpress_id: 325
---

First, a quick disclaimer: I have only tested this in a very limited configuration. Namely, using [OpenSSH](http://www.openssh.org/) 4.2p1 on [Mac OS X](http://www.apple.com/macosx/) (as reported by `ssh -V`) to connect to OpenSSH 3.9p1 on [CentOS](http://www.centos.org/) 4.3 (again, as reported by `ssh -V`). I have been trying to get it to work with the SSH server in [Solaris 10](http://www.sun.com/software/solaris/) but have been unsuccessful thus far (more on that in a moment).

## Configuring the SSH Server

First off, you'll need to make sure that the OpenSSH server's Kerberos configuration (in `/etc/krb5.conf`) is correct and works, and that the server's keytab (typically `/etc/krb5.keytab`) contains an entry for "host/fqdn@REALM" (case-sensitive). I won't go into details on how this is done again; instead, I'll refer you to any one of the recent Kerberos-related articles (like [this one][1], [this one][2], or [even this one][3]). Just be sure that you can issue a `kinit -k host/fqdn@REALM` and get back a Kerberos ticket without having specify a password. (This tells you that the keytab is working as expected.)

Next, configure the `/etc/ssh/sshd_config` file (the system-wide SSH daemon configuration file) to include the following lines (note that these lines may already be present, just commented out; other lines may be present, but with different values):

    KerberosAuthentication yes
    GSSAPIAuthentication yes
    GSSAPICleanupCredentials yes
    UsePAM no

After these changes are made, you'll need to restart the SSH daemon.

## Configuring the SSH Client

Because the OpenSSH client configuration does not include GSSAPI authentication by default, you'll most likely need to modify your SSH client configuration. Edit the global client-side configuration file; on Mac OS X it's found as `/etc/ssh_config`. Change it to include the following lines:

    Host *.example.com
      GSSAPIAuthentication yes
      GSSAPIDelegateCredentials yes

This limits GSSAPI authentication to only those hosts in the example.com domain. Modify the domain to be the appropriate domain for your network.

## Testing the Configuration

Obtain a valid Kerberos ticket from Active Directory. On Mac OS X, you can use the excellent Kerberos.app that's bundled with the system to obtain a ticket. You can also just use `kinit username` from the command line.

Once you have a ticket, you should be able to simply `ssh fqdn.of.server` and you will get logged in, without getting prompted for a password. If you get prompted for a password, go back and double-check your keytab, your SSH daemon configuration, and the time configuration on your OpenSSH server. Because Kerberos requires time synchronization, differences of greater than 5 minutes will cause the authentication to fail.

## Future Configurations

As I mentioned earlier, I have also been trying to make this work with the SSH server bundled with Solaris 10 (which, as I understand it, is not OpenSSH). So far, I have been unsuccessful in this effort, even though the `pam_krb5` integration (having the keyboard-interactive login checked/authenticated via Kerberos) is working just fine. Sun's SSH server is supposed to include GSSAPI authentication enabled by default, but for some reason my client is throwing a "Server not found in Kerberos database" error (seen when running `ssh -vvv full.server.name`). I'm not yet sure what's going on there, but I intend to continue to research the problem and try to find a solution. Solaris gurus out there, I'm open for suggestions.

**UPDATE:** The problems with Kerberos SSH logins to Solaris was a client-side issue; read more about that [here][4].

[1]: {{< relref "2006-08-08-linux-active-directory-and-windows-server-2003-r2-revisited.md" >}}
[2]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
[3]: {{< relref "2006-08-21-more-on-kerberos-authentication-against-active-directory.md" >}}
[4]: {{< relref "2006-08-29-follow-ups-on-solaris-native-kerberos-authentication.md" >}}
