---
author: slowe
categories: Tutorial
comments: true
date: 2006-09-14T13:02:56Z
slug: a-couple-handy-ad-manipulation-tricks
tags:
- ActiveDirectory
- LDAP
- Microsoft
- UNIX
title: A Couple Handy AD Manipulation Tricks
url: /2006/09/14/a-couple-handy-ad-manipulation-tricks/
wordpress_id: 335
---

First, a couple of disclaimers: if you are an old hand with regexes (regular expressions), you won't find anything terribly groundbreaking here. Conversely, if you are a complete newbie to regexes, some of the examples I provide may leave you confused. (I am neither a veteran nor a newbie...instead I'm somewhere in between.)

Where shall we start? Let's say that you needed to add a prefix to all the names of the security groups in your Active Directory domain. Of course, you _could_ do this by hand, but let's explore something a bit more...automated. (Caveat: If you only have 20 groups in your domain, the setup work for this process may be more involved than just changing the name manually.)

First, export the list of groups out of Active Directory using a tool such as AdFind:

```text
adfind -b ou=Groups,dc=example,dc=net -s subtree  
-f "(objectCategory=Group)" displayName –csv > grp-list-1.csv
```

This produces a list of groups in the Groups OU at the root of the domain example.net in CSV format, listing the full DN and the group's display name. It will look a little something like this:

	"CN=Dallas Users,DC=example,DC=net","Dallas Users"  
	"CN=Sacramento Users,DC=example,DC=net","Sacramento Users"  
	"CN=Lexington Users,DC=example,DC=net","Lexington Users"

Apply this regex to the file (using your regex-capable text editing tool of choice; I used a Win32 port of `sed`):

	s/(\",\")/\1SG-/g

The command line, if you were to use `sed`, would look something like:

	sed -r -e s/(\",\")/\1SG-/g input-file.csv > output-file.csv`

After this command (which would run _very_ fast, even for listings of thousands of groups), the file would look like this:

	"CN=Dallas Users,DC=example,DC=net","SG-Dallas Users"  
	"CN=Sacramento Users,DC=example,DC=net","SG-Sacramento Users"  
	"CN=Lexington Users,DC=example,DC=net","SG-Lexington Users"

Now, all your groups have an "SG-" prefix (for **S**ecurity **G**roup, of course).

You could then take the output file and convert it from CSV to LDIF for importing back into Active Directory, as I described [in this posting][1]. After a couple of steps with Log Parser and LDIFDE, you could have all your security groups updated in just minutes. Handy, don't you think?

sed (or **s**tream **ed**itor) is a Unix application that is exceedingly efficient at making changes to text files. When combined with a regex (regular expression), this gives us a tremendous amount of flexibility and power to quickly make large numbers of changes.

OK, let's run with another example. What if your organization decides its going to split into two different companies, sharing the same infrastructure, and you are charged with the task of stamping every user account with a suffix that denotes the company for which that user works. After separating the users into separate OUs (which would be useful for separate Group Policy object application anyway), export the user list using AdFind:

```text
adfind -b ou=NewCompanyB,dc=example,dc=net -s subtree  
-f "(&(objectCategory=User)(objectClass=User))" name displayName  
–csv > usr-compb-list-1.csv
```

This will create a more complex listing that includes full DN, name, and display name attributes of the users in that OU. With this file, use `sed` to transform it automagically:

	sed -r -e s/(,DC=net\",\")(.*)(\",\")(.*)(\")($)/\1\2 [New 
	Org]\3\4 [New Org]\5\6/g usr-compb-list-1.csv > output-file.csv

You may find it easier to put the regex in a file (say, called `fixnames.sed` or similar) and use this command instead:

	sed -r -f fixnames.sed usr-compb-list-1.csv > output-file.csv

When this command is complete, your `output-file.csv` would look something like this:

	"CN=Smith\, John,OU=NewCompanyB,DC=example,DC=net",  
	"Smith, John [New Org]","Smith, John [New Org]"  
	"CN=Public\, Joan,OU=NewCompanyB,DC=example,DC=net",  
	"Public, Joan [New Org]","Public, Joan [New Org]"

(Each entry would all be on a single line; they've been wrapped here for readability.)

You could then use a variety of methods to get these changes back into Active Directory. If you wanted to modify only the display name (of course, you'd need to adjust the commands above accordingly) you could use `for /f` with `dsmod user` and pipe the correct information to the command to modify the users; that technique is described [here][2] and [here][3]. Or, you could just use Log Parser again to convert from CSV to LDIF using a template file and then import back in with LDIFDE. The real trick here is again the use of `sed` and regexes to quickly and efficiently make bulk changes to a text file, and that's something that is uniquely Unix-like. Nevertheless, it holds great value in other areas and on other platforms as well.

[1]: {{< relref "2006-06-20-mass-changes-in-active-directory.md" >}}
[2]: {{< relref "2006-07-06-mass-renaming-computers.md" >}}
[3]: {{< relref "2006-06-30-bulk-adding-entries-in-dns.md" >}}
