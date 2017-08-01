---
author: slowe
categories: Explanation
comments: true
date: 2007-09-17T09:54:05Z
slug: one-potential-issue-in-ad-integration-scenarios
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Linux
- Microsoft
- Solaris
- UNIX
title: One Potential Issue in AD Integration Scenarios
url: /2007/09/17/one-potential-issue-in-ad-integration-scenarios/
wordpress_id: 546
---

Regular readers of this blog know that I like to work on integrating various systems into Active Directory. I've written a couple of articles on the issue:

[Linux-AD Integration, Version 4][1]  
[Solaris 10-AD Integration, Version 3][2]  
[Active Directory Integration Index][3]

These articles have been pretty successful and from what I understand have helped a fair number of people integrate their non-Windows systems into Active Directory for simplified user management and authentication. Occasionally, though, we run into the odd issue that isn't quite so straightforward to resolve.

For example, I recently had a reader (let's call him Johnny) who was having a difficult time getting the Linux-AD integration to work. The `ldapsearch` and `kinit` commands worked fine, but `getent passwd` or `getent group` failed with no output. The users in Active Directory did indeed have UNIX attributes added to their accounts. There were no firewalls between the non-Windows systems and the Active Directory domain controllers, and there did not appear to be any connectivity issues whatsoever (this further underscored by the fact that `ldapsearch` successfully returned LDAP search results from AD, and `kinit` successfully obtained a Kerberos ticket from AD). We were stumped.

Johnny and I traded e-mails back and forth a few times, until finally Johnny found his error and notified me about what had been happening. As I read the description about the problem, I realized that this may be a problem that is affecting a lot of users, and may, in fact, have stumped some of you out there reading right now. Here's the details.

The method that I suggest using for AD integration uses two parts:

* First, we use Kerberos to obtain a Kerberos ticket from an Active Directory domain controller (also a Kerberos key distribution center, or KDC). This handles the authentication side of things and prevents the password from crossing the wire at any point in time.

* Next, we use LDAP to centrally store account information, such as UID number, GID number, home directory, login shell, etc. This is the part that typically requires schema extensions (although there is a workaround for that) and using this technique ensures that we don't have to manage accounts individually on each Linux server.

This approach doesn't work without both pieces. The Kerberos authentication takes care of the password, but without account information logins still fail. So if Kerberos works but LDAP doesn't, logins will fail. If Kerberos doesn't work but LDAP is fine, logins will fail. So part of troubleshooting this configuration is isolating where the problem lies. In this particular case, `kinit` worked fine---no error was returned and `klist` showed a valid Kerberos ticket. So the problem had to be with LDAP. But where? The `ldapsearch` command worked fine.

The problem lie with the `/etc/ldap.conf` file. See, the nss_ldap libraries (which are responsible for using LDAP---and other sources, as defined in `/etc/nsswitch.conf`---as the backend information database for account information) are controlled by this file, but `ldapsearch` does not use it. Specifically, the error was with the account that is used to bind (or connect) to Active Directory to perform the searches.

There are two ways of specifying this account in `/etc/ldap.conf`. You can use the full DN, which looks something like "cn=Scott Lowe,cn=Users,dc=example,dc=com" or "cn=John Smith,ou=Marketing,ou=Departments,dc=example,dc=com". Alternately, you can use the universal principal name (UPN), which looks something like an e-mail address, such as "slowe@example.com" or "john.smith@example.com". In this particular case, Johnny (our reader with the problem) was using the full DN, but he was using the wrong attribute in the DN. Here's the information he had:

	First Name: John  
	Last Name: Smith  
	Full Name: John Smith  
	Display Name: John Smith  
	UPN: jsmith@example.com  
	SAM Account Name (downlevel logon name): jsmith  
	Object name: jsmith

Which of these do you suppose should be used in the DN? Full name? No. Display name? No. It must be the **_object name_**, in this case "jsmith". You can double-check your object name (or CN) using ADSI Edit or a similar utility. You _could_ use Active Directory Users and Computers, but that's typically the confusing part. In any case, once Johnny fixed the syntax for the bind account then `getent passwd` and `getent group` worked like a champ.

How do we avoid this kind of issue? Simple: just use the UPN instead of the full DN. This syntax works just as well and avoids the potential problem of using the wrong name when building the full DN.

[1]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[2]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
[3]: {{< relref "2007-01-15-active-directory-integration-index.md" >}}
