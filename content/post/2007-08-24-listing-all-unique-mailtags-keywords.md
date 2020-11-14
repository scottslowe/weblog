---
author: slowe
categories: Tutorial
comments: true
date: 2007-08-24T18:48:57Z
slug: listing-all-unique-mailtags-keywords
tags:
- CLI
- macOS
- Tagging
- Tags
title: Listing All Unique MailTags Keywords
url: /2007/08/24/listing-all-unique-mailtags-keywords/
wordpress_id: 511
---

After figuring out yesterday how to [list all the unique MailTags projects][1] in use, today I worked out how to list all the unique [MailTags](http://www.indev.ca/MailTags.html) keywords in use. As it turns out, it wasn't as hard as I had suspected that it would be.

(Note to experienced UNIX hackers: I'm sure that I probably could have worked out a more elegant solution involving more pipes and redirection, but this works for a UNIX apprentice such as me.)

Here are the commands I used to parse down the list of unique keywords (this command is broken across two lines, but should be typed all on one line):

	mdls <path to mailbox> | grep kMDItemKeywords |
	awk '{print $3 $4 $5 $6 $7 $8 $9}' >> outputfile.txt

This uses the `mdls` command to list all the metadata from the messages in the specified directory, pipes that through `grep` to pick out only the lines containing "kMDItemKeywords", then uses `awk` to list all the keywords (which are in a comma-delimited list surrounded by parentheses. You'll want to repeat this command for every mailbox you use (I have different mailboxes to archive messages by year, so I had to repeat this for 2005, 2006, etc.)

Once we have the keywords list, then we munge it:

	cat outputfile.txt | tr ',' '\n' > outputfile2.txt  
	sed s/\(//g < outputfile2.txt > outputfile3.txt  
	sed s/\)//g < outputfile2.txt > outputfile3.txt  
	sort outputfile3.txt > sorted-file.txt  
	uniq sorted-file.txt > unique-file.txt

This replaces commas with a newline (the `tr` command), strips out the parentheses (the next two `sed` commands), then sorts it and returns only the unique values. The end result, stored in `unique-file.txt`, contains a list of unique MailTags keywords used in all your mailboxes.

Useful, eh?

[1]: {{< relref "2007-08-23-listing-unique-projects-used-by-mailtags.md" >}}
