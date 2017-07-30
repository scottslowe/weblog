---
author: slowe
categories: Information
comments: true
date: 2008-04-11T12:43:11Z
slug: ad-integration-tip-dealing-with-more-than-1000-users
tags:
- ActiveDirectory
- Interoperability
- LDAP
- Linux
- Microsoft
- Windows
title: 'AD Integration Tip: Dealing With More Than 1,000 Users'
url: /2008/04/11/ad-integration-tip-dealing-with-more-than-1000-users/
wordpress_id: 681
---

Reader Scott Merrill pointed out something to me in an e-mail regarding a Registry change that might be necessary in some Active Directory integration scenarios:

>Finally, I would like to share one registry change that we've found to be necessary in our AD integration. By default, the MS LDAP server only returns 1,000 results. As a university department with more than 1000 active students, this limitation has caused us some frustration.  
>
>This KB article shows how to increase the number of results returned in a query: [http://support.microsoft.com/kb/315071](http://support.microsoft.com/kb/315071)  
>
>We recently set MaxPageSize to 5,000. I don't know if this will
introduce additional problems down the road, but at least it lets me fully enumerate all our AD users from a Linux machine with `getent passwd`.

If you have an Active Directory domain with more than 1,000 users in the DN specified in your LDAP configuration, then this is a Registry change you'll want to investigate. Otherwise, you could find that your UNIX/Linux servers aren't able to fully enumerate all the users in the domain.

Thanks, Scott!
