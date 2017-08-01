---
author: slowe
categories: Education
comments: true
date: 2006-07-17T11:42:31Z
slug: listing-services-running-as-a-user-account
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Listing Services Running as a User Account
url: /2006/07/17/listing-services-running-as-a-user-account/
wordpress_id: 300
---

You might want to identify services running as a user account for any number of reasons, including any of the following:

* You are preparing for a migration and need to know what services and/or what computers will be affected when you migrate a particular user account.

* The password for a particular service account needs to be changed, and you need to know which computers will be affected by that change.

* You want to check to make sure that there aren't services running as user accounts out there that you didn't know about.

I'm sure that you can think of more reasons, but that's enough for now. To accomplish this feat, we're going to reach once more into the toolkit of WMIC, this time using the `wmic service` alias to query the service status and configuration of remote systems.

Our base WMIC command is something like this:

    wmic service get Caption,StartName

This command lists all services, with the caption (friendly name) and security context (local system, local service, network service, or user account). That's all well and good, but we only need those services that are running as user accounts. To do that, we'll modify the command to look like this:

    wmic service where (StartName!="LocalSystem" and 
    StartName!="NT AUTHORITY\\LocalService" and 
    StartName!="NT AUTHORITY\\NetworkService") 
    get Caption,StartName

Now the results include only those services running as user accounts, or (if none match), the message "No instances available." Note the double backslashes when checking for services running as LocalService or NetworkService; these are necessary on the command line.

Reusing the `for /f` trick shown [here][1], we can build a more complex command to do the same thing for all computers in an OU:

    for /f "tokens=1" %1 in ('dsquery computer 
    "ou=Workstations,dc=example,dc=net" -o rdn -limit 0') do 
    @wmic /node:%1 /failfast:on service where (StartName!="LocalSystem" 
    and StartName!="NT AUTHORITY\\LocalService" and 
    StartName!="NT AUTHORITY\\NetworkService") 
    get Caption,StartName > c:\temp\svc-list-%1.txt

This command will create a list of files, one for each computer returned by the query, each of which contains a list of services running as user accounts. One new technique shown here is the creation of a text files that have the computer names returned from the Active Directory query embedded in the filename. Note that we've also incorporated the `/failfast:on` switch, to avoid delays due to computers that are turned off, out of the office, or otherwise unreachable.

With these text files, you then have the information you need to know what computers and/or services will be affected by password or account changes, and you can stay on top of new services being added to the network without your knowledge.

**UPDATE:** This weblog entry shows [another way to get this information](http://guy.netguru.co.il/archives/19-Querying-services-and-the-account-they-run-under.html), this time without using WMIC (this may be useful for dealing with older computers that may not support WMIC).

[1]: {{< relref "2006-07-06-remotely-setting-the-dns-suffix-search-order.md" >}}
