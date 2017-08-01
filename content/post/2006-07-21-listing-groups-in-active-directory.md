---
author: slowe
categories: Education
comments: false
date: 2006-07-21T12:34:58Z
slug: listing-groups-in-active-directory
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Listing Groups in Active Directory
url: /2006/07/21/listing-groups-in-active-directory/
wordpress_id: 305
---

There's nothing really unusual or new about the commands we'll use to perform this task, other than the little tidbit about how to search for specific types of groups; I disclosed that information while discussing how to [enumerate membership in universal groups][1].

As mentioned in that earlier article, the specific information on the values of the "groupType" attribute are fully documented [here](http://www.microsoft.com/technet/scriptcenter/resources/qanda/aug05/hey0817.mspx). Using the values listed there, we sum up the values for the type of group we'd like to find and then place that into a `dsquery *` command. Some examples are listed below.

Finding global security groups:

    dsquery * "dc=example,dc=net" -scope subtree 
    -filter "(&(objectCategory=group)(groupType=-2147483646))" 
    -limit 0

Finding domain local security groups:

    dsquery * "dc=example,dc=net" -scope subtree 
    -filter "(&(objectCategory=group)(groupType=-2147483644))" 
    -limit 0

Finding universal security groups:

    dsquery * "dc=example,dc=net" -scope subtree 
    -filter "(&(objectCategory=group)(groupType=-2147483640))" 
    -limit 0

Note that the base DN (the "dc=example,dc=net" in our samples above) only needs to be enclosed in quotes if there are spaces in the DN; otherwise, it can specified bare (without quotes) on the command line. The LDAP filter, however, must always be enclosed in quotes.

Of course, you could easily take the output from these commands and pipe them to another command (like `dsget group` to enumerate membership) or redirect the output to a text file (using `> somefile.txt` at the end of the command) for future use.

**UPDATE:** I forgot to provide details on how to list distribution groups; the examples above only list security groups. Obviously, most of the query stays the same, and the groupType value changes for each distribution group scope:

Domain local distribution group: groupType=4  
Global distribution group: groupType=2  
Universal distribution group: groupType=8

Substitute these values in one of the full queries above to list distribution groups of the specified scope.

[1]: {{< relref "2006-06-22-enumerating-universal-group-membership.md" >}}
