---
author: slowe
categories: Explanation
comments: true
date: 2008-01-09T23:46:08Z
slug: local-logins-refused-in-ad-integration-scenarios
tags:
- ActiveDirectory
- ESX
- Interoperability
- Kerberos
- LDAP
- Virtualization
- VMware
title: Local Logins Refused in AD Integration Scenarios
url: /2008/01/09/local-logins-refused-in-ad-integration-scenarios/
wordpress_id: 603
---

A customer with whom I had worked to fully integrate their ESX Server systems into Active Directory---using [my instructions here][1]---had run into a problem. The problem was that local logins were refused when the Service Console lost network connectivity. Clearly, this was a problem; if the customer couldn't login as root when the network is down, even locally at the console, then we have problems. So I set out today to isolate and fix the problem.

After much trial and error, I had determined what was _not_ the cause the problem:

* The `/etc/pam.d/system-auth` file was not at fault; we tried numerous combinations in system-auth and there was no difference in behavior

* The `/etc/ldap.conf` file was not at fault; we even tried adding a few additional entries (like "bind_policy soft") to help with issues when LDAP was down and not responding

* A lack of DNS resolution was not the problem; the behavior was the same whether DNS was working or not

Finally, I was able to track down [this thread](http://osdir.com/ml/ldap.padl.nss/2006-09/msg00012.html) which discusses the behavior of the nss\_ldap libraries when the LDAP service is not available across the network. In [this specific message](http://osdir.com/ml/ldap.padl.nss/2006-09/msg00014.html) in the thread, it is noted that nss\_ldap will try to contact LDAP to enumerate group membership, _even if LDAP is down._ So the problem was with using LDAP for group membership, and a quick edit to `/etc/nsswitch.conf` to remove LDAP from the group line proved that to be true.

As shown in the message, the only workarounds are:

* Upgrade to v245 of nss\_ldap, which allows the use of the "nss\_initgroups\_ignoreusers" directive; this instructs nss\_ldap to _not_ perform group enumeration for the specified users; or

* Remove the ldap entry from the group line in `/etc/nsswitch.conf`.

Unfortunately, ESX Server 3.0.2 and ESX Server 3.5.0 only supply nss_ldap v207-17, which is too early to support that directive. Of course, we can't really upgrade the library, either, since that's not supported by VMware. So the only real fix for VMware ESX Server AD integration scenarios is to not use Active Directory for group memberships. User accounts can still be managed using Active Directory---and authentication occurs against Active Directory---but groups and group membership will have to be handled locally.

This issue is applicable to other operating systems besides ESX Server, though, and for those operating systems an upgrade of the nss\_ldap library and the use of the "nss\_initgroups\_ignoreuser" directive in `ldap.conf` may be all that is needed to fix an issue with local logins being refused when network connectivity is not present.

**UPDATE:** It appears that local logins will work without network connectivity, even with full Active Directory integration, if you use the Emergency Console. Thanks to Magnus for the update!

[1]: {{< relref "2007-07-10-esx-server-ad-integration.md" >}}
