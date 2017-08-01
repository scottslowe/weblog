---
author: slowe
categories: Information
comments: true
date: 2007-11-27T16:29:27Z
slug: some-notes-on-solaris-ad-integration
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Samba
- Solaris
- UNIX
title: Some Notes on Solaris-AD Integration
url: /2007/11/27/some-notes-on-solaris-ad-integration/
wordpress_id: 585
---

This afternoon, I walked back through my own [instructions for integrating Solaris 10 and Active Directory][1], and I found that the process wasn't as smooth as perhaps I'd believed it to be. As a result of walking back through the process again myself, I've collected some notes. At some point in the near future, these notes will be integrated into a new version of the Solaris-AD integration instructions.

So, without further ado, here are the notes I collected in no particular order:

* The Blastwave Samba package does _not_ create its own `smb.conf` file in `/opt/csw/etc/samba`. This is correctly pointed out in the latest integration instructions, but I wanted to mention it again here. You'll need to manually create the `/opt/csw/etc/samba/smb.conf` file before attempting to join the Solaris server to Active Directory via the `net ads join` command.

* The defaultServerList portion of the `ldapclient manual` command only supports IP addresses. The LDAP client service kept going into maintenance mode when using hostnames. On a hunch, I substituted IP addresses for hostnames, and it worked. Go figure.

* Apparently, you can't use `ldapclient mod` to change an existing attribute map. I had a hunch about resolving a co-existence issue where both Solaris and Linux are both authenticating against Active Directory---more on that particular topic is coming soon as well---and needed to change the attribute maps for the homedirectory and loginshell attributes. I ended up editing the ldap_client_file manually (found in `/var/ldap`; must be made writable using chmod) in order to make the change. If anyone has a more elegant fix, please let me know.

* The `net ads join` command correctly creates a Kerberos keytab with the appropriate entries, but places it in the wrong location. On my test system, it placed the `krb5.keytab` file in the `/etc` directory, and Solaris expected it to be in `/etc/krb5` instead. Until I moved that file, authentication against Active Directory consistently failed.

* It turns out that it's not really necessary to enable the DNS client using `svcadm enable svc:/network/dns/client:default`; from what I've been able to gather, that's there as a dependency only. The `nslookup` and `host` commands seemed to work just fine with this service still disabled.

Again, I'll be incorporating these changes into a future version of the Solaris-AD integration instructions. I hope to have that complete within the next week or two, so stay tuned. In addition, I have information coming to help with the co-existence of multiple UNIX and UNIX-like operating systems all authenticating against the same Active Directory forest, so keep your eyes peeled for that as well.

[1]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
