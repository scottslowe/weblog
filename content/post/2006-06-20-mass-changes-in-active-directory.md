---
author: slowe
categories: Tutorial
comments: false
date: 2006-06-20T17:39:33Z
slug: mass-changes-in-active-directory
tags:
- ActiveDirectory
- LDAP
- Microsoft
title: Mass Changes in Active Directory
url: /2006/06/20/mass-changes-in-active-directory/
wordpress_id: 272
---

I'd previously published information on making bulk changes in Active Directory, but those changes previously involved changing one attribute to the same value for all the accounts. For example, earlier I described [how to make mass password changes][1] using `dsquery` and `dsmod`. But what about those situations where simple piping of output doesn't work, like when multiple attributes need to be changed? Here's one technique.

I needed to create this process for a project I was working on. In this project, we needed to be able to set the User Principal Name (UPN) in Active Directory. We couldn't just use `dsquery` and `dsmod` here, since `dsmod` can only accept a DN on standard input; how would we get the UPN value into the command? There was no way to pipe both values from `dsquery` to dsmod. A more elegant solution was going to be required.

After a fair amount of trial and error, I finally found this solution. Hopefully, it will prove useful to someone.

### Exporting the Data from Active Directory

First, we need to get some raw data to work with. In this scenario, we're going to set the UPN for a group of user accounts in the same OU to match their primary e-mail address.

To get the information we need to accomplish that, we'll first use `csvde` to export information from Active Directory in CSV (comma-separated values) format:

	csvde -f c:\output.csv
	-d "ou=Users,ou=Atlanta,ou=Locations,dc=example,dc=net"
	-r "(objectclass=user)" -l dn,mail

Be sure to type this command all on a single line, not wrapped as it is displayed here. This exports only the DN and mail attributes (as specified by the `-l` switch) for users in the Locations/Atlanta/Users OU to a file named `output.csv`.

Now we have the raw data we need, so we move on to the next step.

### Manipulating the Data

The problem we face is that we can only use `csvde` to add new objects, not to modify existing objects. That's not what we need, so we need to convert the CSV data we have into something else. I experimented with a number of CSV-to-LDIF converters, especially [this one from Novell](http://www.novell.com/coolsolutions/tools/13658.html), but couldn't get them to work correctly. Finally, I found [Log Parser](http://www.microsoft.com/technet/scriptcenter/tools/logparser/default.mspx).

If you've never heard of Log Parser, take a break right now and visit the [unofficial Log Parser support site](http://www.logparser.com/) to find out more about the program and what it can do.

Done now? OK, good. We're going to use Log Parser to read in our CSV output and place the fields into a template file that we will create. That template file can then be fed to `ldifde` for import into Active Directory.

Here's the template file that I used:

```text
<LPBODY>  
dn: %FIELD_3%  
changetype: modify  
replace: userPrincipalName  
userPrincipalName: %FIELD_4%  
-  
</LPBODY>`
```

In this template, `%FIELD_3%` and `%FIELD_4%` represent the third and fourth fields in the CSV file. This confused me at first, since the CSV output has only two fields, but the difference is in how Log Parser generates the output. Save this file (make note of the filename!) and then we're ready to proceed. For the purposes of this article, I'll assume we used the name `template.tpl`. By the way, if you leave the header line in the CSVDE output, you can use the "friendly" field name in the template instead of the more generic `%FIELD_3%`.

Use this command to convert our CSV output into LDIF format for modifying Active Directory:

```text
type c:\output.csv |
logparser "SELECT * FROM STDIN"
-i:CSV -o:tpl -tpl:c:\template.tpl -q:on -stats:off >
c:\output.ldf
```

This command selects all fields from standard input (which is being piped to Log Parser by the type command) and places them into the template file (using the placeholders described earlier), then redirecting the output to a file named `output.ldf`.

The results of the file will look something like this:

```text
dn: CN=Bob Smith,OU=Users,OU=Atlanta,OU=Locations,DC=example,DC=net  
changetype: modify  
replace: userPrincipalName  
userPrincipalName: Bob.Smith@atlanta.example.net  
-
```

The dash is important, by the way. Refer to [this Microsoft article](http://www.microsoft.com/technet/prodtechnol/windows2000serv/technologies/activedirectory/howto/bulkstep.mspx) for more information and examples on using `ldifde` to modify Active Directory.

### Importing the Data Back Into Active Directory

Now, with our freshly created `output.ldf` file ready, we can import the data back into Active Directory to make the desired changes:

	ldifde -i -f c:\output.ldf

This will import the LDIF file back into Active Directory and make the requested changes. Be sure to use the `-j` switch if logs of the changes are needed; otherwise, no logging is performed.

While this is a fairly simple example, the procedure easily lends itself to making multiple changes to large numbers of user accounts. In this procedure, Log Parser is the key; this is what allows us to take information from Active Directory (obtained using `dsquery`, `csvde`, or other utilities) and manipulate it so as to be importable back into Active Directory.

Now that I've discovered Log Parser, I hope to be able to find more ways to use this extremely powerful tool. Look for more Log Parser-related articles soon.

**UPDATE:** I modified the article to properly render the percent signs above, as well as removing the reference to delete the header line (leaving the header line in the `csvde` output allows you to specify friendly field names in the Log Parser template file). In addition, I added the switches to Log Parser to use quiet output and not display statistics; this keeps us from having to edit the Log Parser output before importing it back into Active Directory. Finally, I published a [follow-up article][2] that provides some additional information as well.

[1]: {{< relref "2006-05-16-mass-password-changes-in-ad-revisited.md" >}}
[2]: {{< relref "2006-07-25-mass-changes-in-active-directory-take-2.md" >}}
