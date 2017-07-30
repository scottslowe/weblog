---
author: slowe
categories: Education
comments: true
date: 2006-11-15T17:55:25Z
slug: report-of-logons-from-security-logs
tags:
- ActiveDirectory
- Microsoft
- Security
- Windows
title: Report of Logons from Security Logs
url: /2006/11/15/report-of-logons-from-security-logs/
wordpress_id: 363
---

As a system administrator, no doubt you've had the occasion where you've needed to review the security event log on a server or a domain controller to retrieve information about when a particular user logged in or logged out. It's a time consuming and laborious process. Or it was, until now.

Try this Log Parser command on for size:

    logparser -i:evt -o:csv "SELECT TimeGenerated AS LogonDate,
    EXTRACT_TOKEN(Strings, 0, '|') AS Account INTO filename.csv FROM 
    \Server1\Security, \Server2\Security WHERE EventID NOT IN 
    (541;542;543) AND EventType = 8 AND EventCategory = 2

Whoa! That's quite a command. Let's break it down just a bit:

* The "-i:evt" and "-o:csv" options specify the event log input format and the CSV output format, respectively.

* The TimeGenerated field is selected and presented as LogonDate, and the EXTRACT_TOKEN function is used to extract the username from the event text and present it as Account.

* The results from the query are placed into the file named `filename.csv`.

* Next, we specify the servers and event logs we'd like to include in the query. Here you can specify as many servers as you'd like (within reason, of course), just tacking additional servers on in front of the WHERE clause separated by commas. In the example above, we're looking only at the Security logs on Server1 and Server2.

* Finally, we restrict the results with a WHERE clause that specifies only events with a type of 8 and a category of 2 that are not event codes 541, 542, or 543 should be returned.

That's pretty cool, but it returns "logon" events by computer accounts as well, including the local computer account. To filter that down to only user accounts, we'll trot out our favorite text filtering tool and a simple regex:

    grep -v -E \$$ filename.csv > filename2.csv

This command returns all the lines that don't end in a dollar sign (as a computer account in Active Directory would end in a dollar sign). Make note that your particular version of `grep` may use slightly different parameters or syntax.

Just to give you a slight idea of how useful this technique can be, I managed to parse over 600,000 security event entries in less than a minute and a half with this command. Beats the heck out of searching through them manually, doesn't it?
