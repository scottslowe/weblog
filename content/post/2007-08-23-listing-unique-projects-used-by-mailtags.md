---
author: slowe
categories: Tutorial
comments: true
date: 2007-08-23T18:14:05Z
slug: listing-unique-projects-used-by-mailtags
tags:
- CLI
- macOS
- Tagging
- Tags
title: Listing Unique Projects Used by MailTags
url: /2007/08/23/listing-unique-projects-used-by-mailtags/
wordpress_id: 510
---

Spurred to see if I could become more efficient than I am currently, I've been experimenting with some GTD-style applications. As a result of my experiments, I found that I needed a list of all the [MailTags](http://www.indev.ca/MailTags.html) projects I've ever used in any of my mailboxes. (To make a long story short, I want to use the same project names across all applications---Mail, GTD application, Spotlight comments for files, etc.) While you can see a list of projects in the Preferences, that list does not include all projects that might ever have been used.

It only took a few minutes of experimentation in the Terminal to come across the `mdls` command, which lists metadata for a file (or group of files). Perfect! Using `mdls`, I listed the metadata for an e-mail message, and found that it was the kMDItemProjects attribute that was used by MailTags. With that information in hand, I hacked together this quick process for listing all the projects in use across all my mailboxes:

	mdls <mailbox path>/*.emlx | grep kMDItemProjects >> ~/all-projects.txt

Repeat this command for each mailbox (for example, I have a separate archive mailbox for each year). Once you've enumerated the information for all the mailboxes, then parse it down like this:

	sort all-projects.txt > sorted-projects.txt
	uniq sorted-projects.txt > unique-projects.txt

Now the file "unique-projects.txt" contains all the unique project names that are in use across all your mailboxes. Very handy! Now, if only obtaining a unique list of all the MailTags keywords were as easy...
