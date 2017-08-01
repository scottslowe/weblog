---
author: slowe
categories: Information
comments: true
date: 2007-11-01T23:19:36Z
slug: lm-and-ntlm-authentication-in-ad-integration
tags:
- ActiveDirectory
- Interoperability
- Kerberos
- LDAP
- Linux
- Windows
title: LM and NTLM Authentication in AD Integration
url: /2007/11/01/lm-and-ntlm-authentication-in-ad-integration/
wordpress_id: 568
---

I've received some feedback from a reader who alerted me to some sort of interaction between the Local Security Policy on the Windows side and Linux servers authenticating to Active Directory via Kerberos/LDAP/Samba. I haven't quite been able to get to the root issue yet, but here's the high level overview.

The reader was seeing strange delays at the end of a Linux logon process that seemingly could not be explained. After jumping through all the hoops, another administrator within the organization changed the Local Security Policy setting that governed the use of LM and NTLM authentication, and the delays disappeared.

The policy had been set to allow both LM and NTLM authentication; when changed to allow only NTLM authentication, the delays disappeared immediately. The Linux server in question did have Samba installed, so apparently Samba was timing out trying the LM authentication; this caused the delays. Of course, this is all just speculation, as we don't know _exactly_ why the policy change eliminated the delay.

In any case, since I've been pushing the use of Samba in my [latest integration instructions][1] (Solaris version [here][2]), I thought it might be prudent to mention this feedback. In the event you start seeing some strange delays in your Linux authentication requests, check the Local Security Policy and see if LM authentication is being permitted. That might just be your culprit.

[1]: {{< relref "2007-01-15-linux-ad-integration-version-4.md" >}}
[2]: {{< relref "2007-04-25-solaris-10-ad-integration-version-3.md" >}}
