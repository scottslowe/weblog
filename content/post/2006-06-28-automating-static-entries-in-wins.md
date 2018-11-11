---
author: slowe
categories: Tutorial
comments: false
date: 2006-06-28T10:50:39Z
slug: automating-static-entries-in-wins
tags:
- Microsoft
- Networking
- Windows
title: Automating Static Entries in WINS
url: /2006/06/28/automating-static-entries-in-wins/
wordpress_id: 283
---

There may be times when large numbers of static (or dynamic, for that matter) entries need to be added to your WINS server. This may be the result of migrating from one WINS server to a new WINS server. In any case, here's the commands needed to do just that.

First, create a simple text file that contains the names and IP addresses of the computers that should be added to the WINS server. These should be separated by a space. Something like this will work well:

	SERVER1 172.16.100.1  
	SERVER2 172.16.100.2  
	SERVER3 172.16.100.3

Let's assume this file is called `static.txt`. From the same directory where `static.txt` is currently saved, execute this command at a command line prompt (the line is broken here for readability, please enter as a single line):

```text
for /f "tokens=1,2" %1 in (static.txt) do 
@netsh wins server add name name=%1 ip=\{\%2}
```

Assuming that the user account under which this command is run has read/write access to the currently-configured WINS server, the output of this command should be something like this:

	***You have Read and Write access to the server srv01.example.net***  
	Command completed successfully.

This will be repeated for each line in the text file `static.txt`. After this command completes, WINS Manager will show the new static entries in the WINS database.

Please note that it should be possible to modify this command line to accept input piped in from another program, but I have not tested this. Enjoy!

**UPDATE:** To direct the requests to a specific WINS server rather than just the default WINS server, add the IP address of the WINS server in the command line like this:

```text
for /f "tokens=1,2" %1 in (static.txt) do 
@netsh wins server 10.10.10.100 add name name=%1 ip=\{\%2}
```

This will direct the name registrations to the specified WINS server.
