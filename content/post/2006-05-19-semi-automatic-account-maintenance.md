---
author: slowe
categories: Tutorial
comments: false
date: 2006-05-19T16:21:20Z
slug: semi-automatic-account-maintenance
tags:
- ActiveDirectory
- Microsoft
- Windows
title: Semi-Automatic Account Maintenance
url: /2006/05/19/semi-automatic-account-maintenance/
wordpress_id: 254
---

Continuing in my "semi-automatic" theme, here's some information on using command-line utilities to help automate account maintenance. By combining `dsquery` and a third-party replacement for `dsmove` (since dsmove has some problems), we can streamline account maintenance policies for Active Directory.

First, the problem with `dsmove`. The `dsmove.exe` utility is supposed to be able to take DNs on standard input (stdin) and move them (with or without a rename operation at the same time) to a new location in Active Directory. Unfortunately, it doesn't work; for some reason, `dsmove` won't accept the output of `dsquery`, even though that output works with `dsmod` and `dsrm`. I found numerous references to this same problem ([here's one](http://www.codecomments.com/archive305-2004-4-163477.html)) via a [Google search](http://www.google.com/search?hl=en&q=dsmove+pipe+dsquery&btnG=Google+Search), so I know I'm not alone.

Fortunately, there's a free third-party replacement that steps up to the plate to fill in for `dsmove`, and it's called [`AdMod`](http://www.joeware.net/win/free/tools/admod.htm). `AdMod` does more than just move objects; it can also modify objects as well. For our purposes, however, we're just going to use it to move objects.

We'll start out with the `dsquery` command again, this time to find inactive accounts:

	dsquery user -inactive 4

This will find all the user accounts have have been inactive (not logged into) for more than 4 weeks. Pipe this into the `dsmod` command to automatically disable them:

	dsquery user -inactive 4 | dsmod user -disabled yes

This ensures that any account that has not been used in more than 4 weeks will be automatically disabled. Now, we can bring in `AdMod` to help us keep those disabled accounts manageable:

	dsquery user -disabled | admod -move "ou=Disabled 
	Accounts,dc=example,dc=net" -safety 100

This will automatically gather all the disabled accounts and move them into the Disabled Accounts OU automatically. Note the "-safety 100" parameter; this means that if more than 100 objects will be affected, the command won't proceed. This can be replaced with the "-unsafe" parameter if this fail-safe isn't necessary.

So, put this into a batch file, schedule it to run once a week, and it will take care of those inactive accounts that are no longer being used. (This will make those security guys pretty happy.)
