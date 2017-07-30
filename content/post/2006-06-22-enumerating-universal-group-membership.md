---
author: slowe
categories: Education
comments: false
date: 2006-06-22T14:43:23Z
slug: enumerating-universal-group-membership
tags:
- ActiveDirectory
- Microsoft
title: Enumerating Universal Group Membership
url: /2006/06/22/enumerating-universal-group-membership/
wordpress_id: 273
---

It's a fairly well-known fact that universal group membership in Active Directory is replicated among all Global Catalog (GC) servers. That is, when the membership of a universal group changes, that change must be replicated to all GC servers in the forest. In Windows 2000, a change to universal group membership replicated the entire membership again; in Windows Server 2003, only the changes are replicated. Even though Windows Server 2003 reduces the load for replicating universal group membership, it's still considered a best practice to keep universal group membership fairly static and to use global groups instead of users. But how does an administrator check that? In large organizations, it's easy to lose control of universal groups and their memberships, especially when delegations have been performed to allow another group to handle group memberships. Fortunately, the directory service command line tools provide the functionality necessary to make this a relatively easy task even in large distributed enterprises.

There are two components here. First, we must find all the universal groups. That in itself can be a fairly daunting task since the number of groups in an Active Directory deployment grows very large as the size of the company increases. Considering that groups, particularly security groups, are the fundamental building block of Windows security, it's easy to see how organizations can quickly end up with hundreds, perhaps thousands, of groups. So we must first find a way to sift through all the groups and find only the universal groups.

Second, we need to enumerate the membership of those universal groups so that we can see who or what has been placed into the membership of the groups. If the members are only other groups, all is well; if there are users as members, that needs to be documented and noted. There very well may be valid business reasons why a user and not a group is placed into a universal group---that's fine. Just document it for future reference. What we want to avoid is allowing the membership of the universal group to become too dynamic, as that will have an adverse impact on Active Directory replication.

### Finding all the Universal Groups

To find all the universal groups, we turn to the `dsquery *` command. The `dsquery group` command won't work here because it has no way of specifying to list/find only universal groups. Instead, we'll use a generic LDAP query with the `dsquery *` command to find universal groups.

But how exactly do we identify universal groups? Kudos to Microsoft's Scripting Guy for providing the [method to identify universal groups](http://www.microsoft.com/technet/scriptcenter/resources/qanda/aug05/hey0817.mspx). Based on that information (follow the link and read the article real quick, then come back), we come up with this command:
    
    dsquery * "dc=example,dc=net" -scope subtree 
    -filter "(&(objectCategory=group)(groupType=-2147483640))"

This command will return the DN of all universal groups. Note that it may be necessary to add a `-limit` parameter to allow the `dsquery` statement to return _all_ of the universal groups (the default is 100, if I recall correctly).

### Enumerating Membership in Universal Groups

Now that we have the DNs for the universal groups, we can use the `dsget group` command to show the membership for those groups, like so:
    
    dsquery * "dc=example,dc=net" -scope subtree 
    -filter "(&(objectCategory=group)(groupType=-2147483640))" |
    dsget group -members

This will return the membership of each of the universal groups. It may be helpful to redirect the output to a text file for future storage or additional manipulation.

One problem with this method is that it doesn't list the universal group name along with the membership. I haven't quite figured out how to do that with the directory service command-line tools just yet...but give me some time.
