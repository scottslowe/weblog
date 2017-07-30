---
author: slowe
categories: Tutorial
comments: false
date: 2006-07-06T11:27:21Z
slug: mass-renaming-computers
tags:
- ActiveDirectory
- Microsoft
- Networking
- Windows
title: Mass Renaming Computers
url: /2006/07/06/mass-renaming-computers/
wordpress_id: 292
---

I hate to continue beating the `for /f` stick, but it sure is a handy command. This time we're going to use it to rename large numbers of computers at a time, such as when your company changes the standard naming convention and then asks you to go back and rename all the computers that don't match the new convention. This is a big deal because not only must the computers be renamed, but the matching computer accounts in Active Directory must also be renamed at the same time. Renaming the computer manually (from the Network Identification tab of the properties of My Computer) renames the Active Directory account, but who wants to do it all by hand?

First, you'll need to prepare a comma-separated list of the old computer name (the current computer name) and the matching new computer name. We're using CSV (comma separated values) here because a number of the directory service command-line utilities, as well as other tools, produce CSV in their output. Building our solution here to accept CSV means that you can later chain pieces together for an even more automated solution.

Once the CSV file is in place, the command looks something like this:

    for /f "tokens=1,2 delims=," %1 in (file.csv) do 
    @echo %1 >> results.txt &&
    @netdom renamecomputer %1 /newname:%2 /ud:DomainAdmin /pd:AdminPass 
    /uo:LocalAdmin /po:AdminPass /force /reboot >> results.txt

Please note that you can run this command across domains with a trust relationship in place; the "/ud" switch assumes the computer's current domain unless told otherwise, and the "/uo" parameter assumes a local account unless specified otherwise.

You'll note we're doing a couple of new things here. First, we're echoing the old name of the computer to a text file; this is for troubleshooting purposes. Then, we run the `netdom` command, substituting the appropriate parameters from the text file, and redirecting its output to the same text file as well. The two commands are concatenated together on the same line using the double ampersands.

The resulting output (results.txt in the example above) looks like this:

    pcname1
    The command completed successfully.
    
    pcname2
    The command completed successfully.

This makes it very easy to go back and double-check to see which computers were successfully renamed and which computers had problems.

There are a couple of important caveats to this solution, however:

1. You must either embed the passwords for the domain administrator and local administrator accounts in the command line, or you must enter the passwords for each computer to be renamed. When renaming hundreds of computers, typing in the passwords is obviously not an option.

2. Every computer must have the same local account and password as specified in the command line. One could work around this by embedding the local username and password into the source text file, but this would most likely be more complicated than just standardizing usernames and passwords across workstations.

3. This command is not intelligent enough to stop trying if the remote computer is unavailable, so lengthy delays will be introduced for those remote computers that are, for whatever reason, unreachable.

Even with these limitations and drawbacks, however, the process can be very useful when there are significant numbers of computers that must be renamed.
