---
author: slowe
categories: Rant
comments: false
date: 2006-01-24T10:12:51Z
slug: again-i-ask-isnt-there-a-better-way
tags:
- Oracle
- Security
title: 'Again I Ask: Isn''t There a Better Way?'
url: /2006/01/24/again-i-ask-isnt-there-a-better-way/
wordpress_id: 165
---

Last summer, I wrote about my [concerns with regards to fourth-generation rootkits][1] and their supposed beneficial intentions. Now that the same approach is being applied to Oracle databases, I ask again: isn't there a better way?

A security researcher recently [announced that he has created a better "rootkit" for Oracle](http://www.eweek.com/article2/0,1759,1914465,00.asp) that improves upon the earlier version unveiled last year at the Black Hat Conference in Amsterdam. This new version makes it more difficult for database administrators and security professionals to locate the rootkit. Supposedly, this is all being done to underscore the vulnerabilities and flaws in the Oracle database (and, to a lesser extent, Microsoft SQL Server, IBM DB/2, and others).

Isn't there a better way? As IT professionals---whether we be security experts, database experts, or networking experts---we ought to be able to find a way to openly discuss security flaws and vulnerabilities without actually creating tools for exploiting them. Now what's going to happen when this "rootkit" (my definition of rootkit is a bit more stringent than the one used in the referenced _eWeek_ article) falls into the wrong hands and is used to steal hundreds of thousands of credit card numbers from a leading financial institution? What if it was _**YOUR**_ financial institution that was compromised using this tool? Would you still be in favor of this approach then?

I suppose that's the real value behind open source software; the flaws and vulnerabilities are out there for anyone to see in the source code itself.

[1]: {{< relref "2005-07-29-theres-got-to-be-a-better-way.md" >}}
