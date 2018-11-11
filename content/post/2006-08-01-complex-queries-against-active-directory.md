---
author: slowe
categories: Education
comments: true
date: 2006-08-01T18:57:35Z
slug: complex-queries-against-active-directory
tags:
- ActiveDirectory
- Microsoft
title: Complex Queries Against Active Directory
url: /2006/08/01/complex-queries-against-active-directory/
wordpress_id: 311
---

For example, consider a situation where you may need to compare two different attributes in Active Directory and list those accounts where the two attributes aren't the same. One excellent example is the situation where an organization has standardized on UPN (User Principal Name; looks like an [RFC 822](http://www.ietf.org/rfc/rfc0822.txt)-compliant e-mail address) logons, and each user's UPN is supposed to match that user's e-mail address. How do you go about finding those accounts where the UPN and primary e-mail address don't match?

It may be possible to do this with `dsquery`, but I couldn't find any way to make it happen. After trying for a while, I turned again to [Log Parser](http://www.logparser.com/) (download [here](http://www.microsoft.com/downloads/details.aspx?FamilyID=890cd06b-abf8-4c25-91b2-f8d975cf8c07&displaylang=en)), Microsoft's Swiss Army Knife of log parsing and data manipulation utilities. Log Parser is particularly helpful in this case because it offers a input format specifically for Active Directory, capable of making native Active Directory queries with all the flexibility of Log Parser's SQL-like functionality.

Here's a query that does exactly what we just mentioned---finds all accounts where the UPN prefix (the part before the "@" symbol) doesn't match the user name portion of the e-mail address (the part before the "@" symbol):

```text
logparser -i:ads -o:csv -stats:off "SELECT cn, 
TO_LOWERCASE(SUBSTR(mail,0,LAST_INDEX_OF(mail,'@'))) as mailPrefix, 
TO_LOWERCASE(SUBSTR(userPrincipalName,0,LAST_INDEX_OF 
(userPrincipalName,'@'))) as uPNPrefix 
FROM 'ldap://server.domain.com/cn=Users,dc=domain,dc=com' 
WHERE NOT (mailPrefix = uPNPrefix)" -objClass:user >> output.csv
```

Of course, this is just one example; I'm sure that you can think up many other examples where this kind of complex query functionality would be useful in managing Active Directory. Why don't you list some ideas for useful queries in the comments below, and then I'll post another article with all the appropriate Log Parser commands.

**UPDATE:** Another fairly complicated query came to me today: trying to locate user accounts that aren't named "Lastname, Firstname." The only way I could think to do this was using this Log Parser query:

```text
logparser -i:ads -o:csv -stats:off "SELECT cn FROM <br></br>
'ldap://server.domain.com/cn=Users,dc=domain,dc=com' WHERE <br></br>
cn NOT LIKE '%,%'" -objClass:user
```

This will return all users that do not contain a comma in their name, thus finding any accounts not named "Lastname, Firstname."
