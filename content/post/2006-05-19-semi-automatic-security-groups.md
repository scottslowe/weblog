---
author: slowe
categories: Tutorial
comments: false
date: 2006-05-19T16:05:14Z
slug: semi-automatic-security-groups
tags:
- ActiveDirectory
- LDAP
- Microsoft
- Windows
title: Semi-Automatic Security Groups
url: /2006/05/19/semi-automatic-security-groups/
wordpress_id: 253
---

One really cool feature of [Exchange Server 2003](http://www.microsoft.com/exchange/default.mspx) is _query-based distribution groups_. This feature allows organizations to define LDAP queries and then dynamically populate distribution groups based on the results of the LDAP query. Unfortunately, there's no built-in equivalent for security groups, but here's a reasonable workaround to create that same kind of functionality.

Using the `dsquery` and `dsmod` tools in Windows Server 2003, we can create a command that issues an LDAP query, then stuffs the result of that LDAP query into an already-existing security group. Here's how it works.

First, we use the `dsquery *` command, which allows us to define a custom LDAP query to find any kind of object within Active Directory. Let's say we are interested in automatically populating departmental security groups based on the department attribute for each user object. To find all the members of the Engineering group in Atlanta, we'd use a command like this (this has been broken into three lines for readability, but should be typed in as a single line):

``` text
dsquery * ou=Users,ou=Atlanta,ou=Locations,dc=example,dc=net 
-filter "(&(objectcategory=person)(objectclass=user)
(department=Engineering))" -limit 1000
```

This command will return the DNs of those user objects in the Locations/Atlanta/Users OU whose department attribute is set to Engineering. We can then pipe that output to the `dsmod` command, like so (again, lines have been broken for readability but this should be entered as a single line):

``` text
dsquery * ou=Users,ou=Atlanta,ou=Locations,dc=example,dc=net 
-filter "(&(objectcategory=person)(objectclass=user)
(department=Engineering))" -limit 1000 | dsmod group "cn=Atlanta 
Engineering Dept,ou=Groups,ou=Atlanta,ou=Locations,
dc=example,dc=net" -addmbr
```

This command takes the output of the `dsquery` command and pipes it to the `dsmod` command, modifying the group named Atlanta Engineering Dept in the Locations/Atlanta/Groups OU. (If you anticipate more than 1,000 users in Atlanta in the Engineering department, adjust the "-limit" parameter accordingly.)

There's a couple of problems, however. Once the command has run once, then subsequent times will generate an error because the users will already be a member of that group. In addition, this command doesn't take into account those users who have left the Engineering group. To fix this, we need a couple more commands.

First, we get the members of the group using the `dsget` command:

	dsget group "cn=Atlanta Engineering Dept,ou=Groups,
	ou=Atlanta,ou=Locations,dc=example,dc=net" -members

This returns a list of the DNs for those users that are currently members of the specified group. We pipe that to the `dsmod` command again to clear the group out:

``` text
dsget group "cn=Atlanta Engineering Dept,ou=Groups,
ou=Atlanta,ou=Locations,dc=example,dc=net" -members | dsmod group 
"cn=Atlanta Engineering Dept,ou=Groups,ou=Atlanta,ou=Locations,
dc=example,dc=net" -rmmbr
```

This command sequence removes all the current members of the group. Run this command before the `dsquery`, and both problems (the error about users already being a member and "stale" members not being removed) are corrected.

By wrapping these commands into a batch file and then scheduling the batch file to run on a regular interval (an interval to be determined by your organization), you have created semi-automatic security groups that will repopulate every time this command runs.

There are some caveats, of course. Since groups memberships are only updated on the access token when a user logs out and logs back in again, the changes to the group membership won't take effect immediately. For this reason, it's probably best to only run these scripts once a day during the off hours. Second, this technique is only effective if your organization is making sure the information in Active Directory is up-to-date (of course).
