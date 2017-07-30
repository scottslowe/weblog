---
author: slowe
categories: Musing
comments: false
date: 2005-07-29T15:43:00Z
slug: cisco-ios-windows-xp
tags:
- Cisco
- Security
- Windows
title: Cisco IOS = Windows XP?
url: /2005/07/29/cisco-ios-windows-xp/
wordpress_id: 60
---

Well, the [Black Hat](http://www.blackhat.com/main.html) conference in Las Vegas certainly generated a great deal of excitement this past week. A security presentation was going to be given about a first-ever exploit for [Cisco](http://www.cisco.com) routers. From what I've been able to decipher, Cisco had been working closely with [ISS](http://www.iss.net), the company whose researcher (Michael Lynn) was supposed to give the presentation, and both companies apparently agreed that the presentation needed further review. Cisco and ISS [jointly filed an injunction](http://www.eweek.com/article2/0,1759,1841232,00.asp) against Michael Lynn when he [quit his job at ISS to give the presentation](http://www.computerworld.com/securitytopics/security/holes/story/0,10801,103515,00.html) anyway; this is after Cisco employees physically removed presentation pages from the books handed out at the conference and destroyed CDs containing information about the presentation.

It appeared as if Lynn was going to give a VoIP presentation instead, but then [proceeded with the original presentation](http://www.eweek.com/article2/0,1759,1841166,00.asp) anyway, even though he knew it would likely result in lawsuits from Cisco and his former employer. In the presentation, he likened Cisco's IOS (Internetwork Operating System) to Microsoft Windows XP, saying "IOS is the Windows XP of the Internet."

Finally, Thursday, a court order was issued and [all parties involved have agreed](http://www.computerworld.com/securitytopics/security/story/0,10801,103561,00.html) to the terms of the court order, which restrict them from further sharing or disseminating any information about the security flaw.

Both Cisco and ISS have taken a real hit from this whole situation, and I can understand why. Cisco looks like it's trying to cover up security vulnerabilities; it was only after all of this that Cisco issued a [security advisory](http://www.cisco.com/warp/public/707/cisco-sa-20050729-ipv6.shtml) discussing the vulnerability. If Michael Lynn's research was accurate, then it is appropriate for people to know so that our networks can be protected. Cisco network equipment running IOS does, indeed, power a large portion of the Internet. _At the same time_, if he violated the law and the terms of his agreement with Cisco by reverse engineering IOS, then he should not have publicized that information. _But then again_, it appears as if Cisco would not have released security information had Mr. Lynn not proceeded with the presentation, so...you see that this issue is sensitive and there are reasonable and understandable concerns on all sides.

To be honest, I don't know what I would have done if I were in the same situation. The only advice that comes to mind is, "If you do what is right, you can't go wrong." But what is _"right"_ in this situation?
