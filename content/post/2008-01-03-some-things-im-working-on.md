---
author: slowe
categories: General
comments: true
date: 2008-01-03T21:51:19Z
slug: some-things-im-working-on
tags:
- ActiveDirectory
- ESX
- iSCSI
- LDAP
- NetApp
- Solaris
- UNIX
- Virtualization
- VMware
title: Some Things I'm Working On
url: /2008/01/03/some-things-im-working-on/
wordpress_id: 599
---

I wanted to take just a brief moment and let everyone know about a few articles that I've got "in the pipeline" for the site. If there is one---or more---of these articles that looks particularly interesting, speak up in the comments.

Here are the articles that are currently under development:

* _An update to the Solaris-AD integration instructions:_ Last time I ran through these instructions I came across a number of discrepancies and little "gotchas." I need to incorporate the workarounds into the integration instructions and publish a new version.

* _A brief blurb on NetApp OSSV:_ NetApp's Open Systems SnapVault (OSSV) is a pretty cool technology, so I want to take a quick look at setting it up. I'm also exploring to see what kind of unique synergies may arise from using OSSV in a VMware environment.

* _Restricting access to ESX Server when using AD integration:_ As reader Scott Garrett points out in a comment [to an earlier article][1], ESX Server's version of sshd doesn't support the UsePAM directive. This prevents us from using group membership in Active Directory to control access to ESX Server's Service Console. Or does it? I have a hunch that there may be at least one workaround for this problem; once I've tested it, I'll document it here.

* _New iSCSI functionality in ESX Server 3.5:_ VMware appears to have refreshed the software iSCSI initiator in ESX Server 3.5, so I want to take a closer look at and discuss here some of this new functionality.

That's it for now, although I'm certainly open to other items that pique reader interest. Feel free to submit your suggestions in the comments. Thanks!

[1]: {{< relref "2007-07-10-esx-server-ad-integration.md" >}}
