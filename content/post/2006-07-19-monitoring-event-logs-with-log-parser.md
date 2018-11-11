---
author: slowe
categories: Education
comments: false
date: 2006-07-19T16:45:42Z
slug: monitoring-event-logs-with-log-parser
tags:
- Microsoft
- Networking
- Windows
title: Monitoring Event Logs with Log Parser
url: /2006/07/19/monitoring-event-logs-with-log-parser/
wordpress_id: 304
---

If you haven't yet downloaded Log Parser 2.2 (the current version), you can get it from [Microsoft's download site](http://www.microsoft.com/downloads/details.aspx?FamilyID=890cd06b-abf8-4c25-91b2-f8d975cf8c07&displaylang=en).

As I've mentioned before, Log Parser is an extraordinarily versatile tool, and the functionality we've seen so far only scratches the surface. In this next example of using Log Parser, we're going to take advantage of the fact that Log Parser possesses the ability to natively parse Windows' event logs. That ability, combined with some SQL query syntax and command-line tricks, will give us an automated method for retrieving warning and error events from a user-defined list of servers and event logs.

Rather than belaboring the Log Parser syntax here, I'll direct you to the [Unofficial Log Parser support site](http://www.logparser.com/). There are numerous resources, both hosted there and linked to from there, that will help you understand the specific syntax. Instead, let's look at the full command and reverse engineer it to understand what's happening.

The full command looks like this:

```text
for /f "tokens=1,2 delims=," %1 in (c:\servers.txt) do 
@logparser -i:EVT "SELECT TimeGenerated,EventID,EventType,
EventTypeName,EventCategory,EventCategoryName,SourceName,
Strings,ComputerName,SID,Message FROM \%1\%2 WHERE 
TimeGenerated > TO_TIMESTAMP(SUB(TO_INT(SYSTEM_TIMESTAMP()),86400)) 
AND EventType IN (1;2) ORDER BY TimeGenerated DESC" -o:CSV 
-q:ON -stats:OFF >> c:\24hr-events.csv
```

Whew! That's a monster of a command. Let's break it down.

1. First, we have the ever-useful `for /f` trick, which here is pulling two parameters from a file called `c:\servers.txt`. Obviously, the path and name of the file are irrelevant; just be sure to put the correct path and name in the command. The contents of this file should contain a list of servers and logs that will be checked by Log Parser, with a single server-event log pair on each line, separated by a comma. So, to check the System log on SERVERA, SERVERB, and SERVERC, the file would look something like this:  

	SERVERA,System  
	SERVERB,System  
	SERVERC,System

Alternately, to review the Application log on SERVERA, the System log on SERVERB, and both Application and System on SERVERC, it would need to look like this:  

	SERVERA,Application  
	SERVERB,System  
	SERVERC,Application  
	SERVERC,System

Simply place all the log names from all the servers you'd like monitored into this text file (in the appropriate format).

2. Next, we have the Log Parser command itself. This command specifies to use the Event log input format (`-i:EVT`) and uses a SQL SELECT statement to choose the specific fields from the event logs, filtering them by EventType (EventTypes 1 and 2 are Error events and Warning events) and by timestamp (only events in the preceding 24 hour period; refer to the Log Parser help file to try to decode the funky SQL math required to do that). The rows returned by the query are sorted according to time, and the output is set to CSV (`-o:CSV`).  The remaining options suppress other output (`-q:ON`) and suppress statistics (`-stats:OFF`). Note the parameter substitution for the source of the query; here is where the server name and event log name will be substituted as they are pulled from the text file specified in the `for /f` clause.

3. Finally, we redirect the output to a filename. It's important to use double redirection symbols here, or else the output file will contain only the results from the last server queried.

To make this command really helpful, though, we need a way to get the results of the Log Parser command into your hands, instead of making you go to a server and look at a text file. This is where Blat comes in.

[Blat](http://www.blat.net/) is a command-line utility for sending SMTP messages. With Blat, sending the output file from the Log Parser command above becomes as simple as this:

```text
blat - -subject "Daily event log report" 
-body "Here is the daily report of warning and error events 
from the servers' event logs." -to your@address -f source@address 
-server your.mail.server -attacht c:\24hr-events.csv
```

Do you remember how to concatenate multiple commands on a single command line? Use the double ampersands! Using double ampersands, we can query the event logs _and_ e-mail ourselves the results all on a single command line:

```text
for /f "tokens=1,2 delims=," %1 in (c:\servers.txt) do 
@logparser -i:EVT "SELECT TimeGenerated,EventID,EventType,
EventTypeName,EventCategory,EventCategoryName,SourceName,
Strings,ComputerName,SID,Message FROM \%1\%2 WHERE 
TimeGenerated > TO_TIMESTAMP(SUB(TO_INT(SYSTEM_TIMESTAMP()),86400)) 
AND EventType IN (1;2) ORDER BY TimeGenerated DESC" -o:CSV 
-q:ON -stats:OFF >> c:\24hr-events.csv &&
blat - -subject "Daily event log report" 
-body "Here is the daily report of warning and error events 
from the servers' event logs." -to your@address -f source@address 
-server your.mail.server -attacht c:\24hr-events.csv
```

Voila! You now have an automated command that will parse your servers' event logs for warning and informational events, output those results into a CSV file (easily manipulated via your choice of spreadsheet program), and e-mail that CSV file to your e-mail address. Can it get any easier?
