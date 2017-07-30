---
author: slowe
categories: Rant
comments: true
date: 2006-11-10T23:21:15Z
slug: full-text-indexing-in-mailapp-broken
tags:
- Macintosh
title: Full-Text Indexing in Mail.app Broken
url: /2006/11/10/full-text-indexing-in-mailapp-broken/
wordpress_id: 360
---

Of course, I've done the requisite Google search, which turned up a few hits. Most of them suggested a rebuild of the mailbox, which I've done. No change. I've also tried forcing Spotlight to re-index the mail data (via `mdimport -f ~/Library/Mail`). Also no change. It's funny because using the Spotlight menu works just fine, and will return mail messages that contain the requested text. It's just the search filter in Mail.app that appears to be affected.

Hmmm...that's interesting. While sitting here typing out this post, I went to verify that the Spotlight filter for Mail was being loaded and used. Although the Mail.mdimporter file exists in `/System/Library/Spotlight`, the `mdimport -L` command doesn't list it. After copying the file over to `/Library/Spotlight`, the `mdimport -L` command does list it. How odd---why would Spotlight pick up importers in one location, but not the other?

OK, so I copied the file over to `/Library/Spotlight`, the `mdimport -L` command shows the Mail Spotlight importer, and I just re-indexed the Mail files with `mdimport -f ~/Library/Mail`. Still no go. What is up with that?

Anyone have any ideas? I'm open to suggestions.

**UPDATE:** Finally fed up with the situation, I took matters into my own hands and fixed the problem. First, I forced Spotlight to re-index the entire hard drive by adding it to the exceptions list, then removing it. After that process was complete, full-text searching in Mail.app worked, but the MailTags stuff stopped working. Fortunately, the [MailTags FAQ](http://www.indev.ca/MTFAQ.html) pointed me in the right direction---I just needed to copy the MailTags Spotlight importer into `~/Library/Spotlight` (instead of `/Library/Spotlight` or `/System/Library/Spotlight`) and force a reimport. Problem solved!
