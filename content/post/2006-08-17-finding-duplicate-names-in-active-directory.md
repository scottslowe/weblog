---
author: slowe
categories: Education
comments: true
date: 2006-08-17T11:43:39Z
slug: finding-duplicate-names-in-active-directory
tags:
- ActiveDirectory
- Microsoft
title: Finding Duplicate Names in Active Directory
url: /2006/08/17/finding-duplicate-names-in-active-directory/
wordpress_id: 323
---

To use this procedure, you'll need access to the Directory Service command line tools (these come installed automatically with [Windows Server 2003](http://www.microsoft.com/windowsserver2003/default.mspx)) and Microsoft [Log Parser](http://www.microsoft.com/downloads/details.aspx?FamilyID=890cd06b-abf8-4c25-91b2-f8d975cf8c07&DisplayLang=en). With these two tools in hand, let's proceed.

First, we'll need to obtain a list of all the CNs in Active Directory for every user and/or every contact, regardless of their container. It may be possible to do this with Log Parser (using the ADS input format), but I couldn't figure out how. Instead, I turned to the Directory Service command line tool `dsquery`. Here's the command to use:

```text
dsquery * "dc=example,dc=com" -scope subtree
-filter "(|(objectCategory=user)(objectCategory=contact))"
-limit 0 > output-file.txt
```

This creates a file (`output-file.txt`) with a list of the CNs for every user object and contact in your Active Directory domain (obviously, you'll need to substitute the correct values in the query statement above---unless your domain is called example.com).

Using this file, then we use Log Parser to list only those CNs that occur more than once in this file. This will identify those objects that have the same name in the domain:

```text
logparser -i:TEXTLINE -stats:off -o:NAT -rtp:-1
"SELECT Text AS objName, COUNT(*) FROM output-file.txt
GROUP BY Text HAVING COUNT(*) > 1" > duplicates.txt
```

This produces a text file that lists each CN which is found more than once in the input file, along with a count of how many times it was found. Use this file to go to Active Directory Users and Computers, find the duplicate objects, and rename them as needed. You can then repeat the process until you don't find any more duplicate names.

While this may seem like overkill for smaller Active Directory installations, this is certainly very applicable in larger organizations, particularly those with decentralized IT operations. Think about it---would _you_ want to manually search through 15,000 objects to find the duplicates?
