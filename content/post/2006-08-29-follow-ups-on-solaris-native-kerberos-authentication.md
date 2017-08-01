---
author: slowe
categories: Information
comments: true
date: 2006-08-29T11:25:39Z
slug: follow-ups-on-solaris-native-kerberos-authentication
tags:
- ActiveDirectory
- Kerberos
- Solaris
- SSH
- UNIX
- Windows
- Interoperability
title: Follow Ups on Solaris, Native Kerberos Authentication
url: /2006/08/29/follow-ups-on-solaris-native-kerberos-authentication/
wordpress_id: 328
---

If you haven't read the previous articles, take a quick moment to review them before continuing:

[Solaris 10 and Active Directory Integration][1]  
[Native Kerberos Authentication with SSH][2]

Initially, problems cropped up with both of these, but it seems as if the problems have been resolved.

### Solaris 10 and Active Directory Integration

The problem, as outlined in the original article, was that TGT validation wasn't working. The only way to make Kerberos authentication (via `pam_krb5`) to work was to disable TGT validation.

Since that time, I have re-enabled TGT validation, and everything seems to work just fine. I believe that the problem was in the `/etc/hosts` file, since I've been seeing a fair number of comments in the various articles dealing with AD integration of non-Windows platforms. I believe that the fix is making sure that the server's fully qualified domain name is listed first in the `/etc/hosts` file, like so:

    127.0.0.1       localhost localhost.localdomain
    10.1.1.1        hostname.example.com hostname
    <...other entries as needed...>

On the line where the server's IP address is listed, make sure that the fully qualified domain name is listed first. I can't be absolutely sure that this is the fix, but this is the only thing I can think of that I did on both Linux and Solaris. I hope to perform some testing later this week and I'll post the results here.

### Native Kerberos Authentication

The problem here was again the Solaris server---or so it seemed. I had (and currently have) it working with a [CentOS][2] server; I can obtain a Kerberos ticket and then transparently authenticate to the CentOS server without getting prompted for a password. With the Solaris server, though, I kept getting a "Server not found in Kerberos database" when attempting to connect. It wasn't until today that I realized that the key difference between the Linux installation and the Solaris installation was that the Solaris server was in a different DNS domain. Since my PowerBook didn't have the equivalent of an `/etc/krb5.conf` to match DNS domain names with Kerberos realms, it was trying to obtain a ticket for a realm that does not exist. More appropriately, it was contacting a Kerberos realm that did not have a matching entry with the right SPN, hence the "Server not found in Kerberos database" error.

To verify this, I edited the `/etc/krb5.conf` file on a Linux server to map DNS domains and Kerberos realms (i.e., mapped "example.net" to "ad.example.net"), obtained a Kerberos TGT using `kinit`, and then attempted to make an SSH connection to the Solaris server. Lo and behold, it worked! I verified this quickly on [Mac OS X](http://www.apple.com/macosx/) using the built-in Kerberos application to properly map DNS domains to Kerberos realms, and it worked as well.

&lt;aside&gt;You are supposed to be able to use Cmd-E in Kerberos.app to edit the domain-realm mapping. However, I found that keystroke doesn't work until a Kerberos preferences file (named `edu.mit.Kerberos`, either in `~/Library/Preferences` or `/Library/Preferences`) is created. This file is simply the equivalent of an `/etc/krb5.conf` file as you might see on any other Unix-related platform. Once that file had been created---even if it was empty---the Cmd-E shortcut and the Edit Realms command on the Edit menu worked as expected.&lt;/aside&gt;

So, the server was not the problem, instead it was the configuration of my PowerBook all along. Go figure! In any case, it is working now. The lesson to be learned: be sure that either your Kerberos realm maps perfectly to Active Directory's DNS name, or be sure client machines are properly configured for domain-realm mappings.

[1]: {{< relref "2006-08-15-solaris-10-and-active-directory-integration.md" >}}
[2]: {{< relref "2006-08-21-native-kerberos-authentication-with-ssh.md" >}}
