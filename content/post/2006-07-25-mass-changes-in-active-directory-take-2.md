---
author: slowe
categories: Tutorial
comments: false
date: 2006-07-25T14:26:06Z
slug: mass-changes-in-active-directory-take-2
tags:
- ActiveDirectory
- Microsoft
title: Mass Changes in Active Directory, Take 2
url: /2006/07/25/mass-changes-in-active-directory-take-2/
wordpress_id: 307
---

In the original article on [how to make mass changes to Active Directory][1], we discussed the use of `csvde` to produce the original output from Active Directory, Log Parser to massage the information into LDIF format, and `ldifde` to import the changes back into Active Directory. Based on some additional testing of this procedure, I made some changes to that article, and I wanted to include additional information here.

For ease of reference, I've arranged this article in the same way as the original article was arranged, and new information is included in each pertinent section.

### Exporting the Data from Active Directory

The use of `csvde` to export data out of Active Directory is great---except when the object name includes a comma in the name. Here's why.

Suppose that all your users are listed in Active Directory as "Lastname, Firstname", such as this:

	Smith, John  
	Public, Joe  
	Normal, Jane

If these users were stored in the built-in Users container, using a tool such as `dsquery` to output their distinguished name would produce these results:

	"cn=Smith\, John,cn=Users,dc=example,dc=net"  
	"cn=Public\, Joe,cn=Users,dc=example,dc=net"  
	"cn=Normal\, Jane,cn=Users,dc=example,dc=net"

The use of the backslash here is to escape the following comma so that it is not treated as a field delimiter. The double quotes are used because there is a space in the name.

Using `csvde`, however, produces these results:

	"cn=Smith\\, John,cn=Users,dc=example,dc=net"  
	"cn=Public\\, Joe,cn=Users,dc=example,dc=net"  
	"cn=Normal\\, Jane,cn=Users,dc=example,dc=net"

_Note the double backslash._ This will cause problems when Log Parser is used to put this information into LDIF format.

To work around this problem, use [AdFind](http://www.joeware.net/win/free/tools/adfind.htm) instead of `csvde`. (Note that you can't use `dsquery` here because `dsquery` can't output in CSV format.) With AdFind, your output is back to normal, with only a single backslash.

(A change made to the original article also notes that we can leave the "header line" intact in the output, so that Log Parser has field names to attach to the data in the output.)

### Manipulating the Data

As noted in the changes to the original article, if we leave the "header line" from the exported output in the file, then we don't have to use generic field names in the template; instead, we can use friendly names such as "dn" or "sAMAccountName".

Also, the command line for Log Parser was updated to show the correct input type (`-i:CSV` instead of `-i:TEXTLINE`). In addition, note the extra `-q:ON` and `-stats:OFF` switches, to reduce the extra output from Log Parser and remove the need to edit the file manually before importing it back into Active Directory.

### Importing the Data Back Into Active Directory

There are no major changes in this section. Only note that in multi-domain environments, it may be necessary to specify a specific domain controller (using the "-s" switch) and provide alternate credentials (using the "-b" switch).

[1]: {{< relref "2006-06-20-mass-changes-in-active-directory.md" >}}
