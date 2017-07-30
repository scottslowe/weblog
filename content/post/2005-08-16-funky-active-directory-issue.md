---
author: slowe
categories: Explanation
comments: false
date: 2005-08-16T23:17:43Z
slug: funky-active-directory-issue
tags:
- ActiveDirectory
- Microsoft
- Windows
- Interoperability
- Security
title: Funky Active Directory Issue
url: /2005/08/16/funky-active-directory-issue/
wordpress_id: 78
---

I came across a funky Active Directory issue today. User objects stored in an OU on which permissions had been assigned were "losing" their permissions and their inheritance from the parent OU. For example, if the group "Support Team" had been granted a set of permissions on the OU "Sales", then the user object "Bob Jones" stored in that OU would lose those permissions regularly. Even when the permissions were reassigned, they would disappear again.

It turns out that if the user object is a member, directly or indirectly, of a "protected group" (such as Server Operators, Backup Operators, or Administrators), then Active Directory automatically removes any inherited permissions, resets those permissions to the default, and turns off inheritance for those objects. The idea is to prevent possible elevation of privileges. This behavior is described in [this KB article](http://support.microsoft.com/default.aspx?scid=kb;en-us;817433).

The fix? Well, you can muck around in Active Directory and play with the adminSDHolder object, or you can just not worry about delegations on those user objects, or you can remove those user objects from the protected group in question. In my particular case, the user objects needed to be taken out of the protected group anyway, so that was a natural fit.

Keep this in mind when planning AD delegations and the placement of accounts that will be members of a protected group.
