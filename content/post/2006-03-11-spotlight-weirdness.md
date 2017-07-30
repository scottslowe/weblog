---
author: slowe
categories: Information
comments: false
date: 2006-03-11T23:17:33Z
slug: spotlight-weirdness
tags:
- Macintosh
- OSS
title: Spotlight Weirdness
url: /2006/03/11/spotlight-weirdness/
wordpress_id: 197
---

One of the key features in Tiger that I was looking forward to was [Spotlight](http://www.apple.com/macosx/features/spotlight/). I know, I know---lots of users have complained about Spotlight's background indexing and its impact on performance, and there's a lot of chatter on various forums and in the newsgroups about disabling Spotlight. I, on the other hand, was interested in having all my stuff indexed by Spotlight so that I could take advantage of the indexes (indices?) in native Mac applications such as Mail (via Smart Mailboxes), Finder (via Smart Folders), Address Book (via Smart Groups), so on and so forth.

However, Spotlight was exhibiting some strange weirdness. I don't know if it was related to Spotlight having already built the index prior to applications being installed, or if it was due to the fact that some of the Spotlight importers weren't being "picked up" by Spotlight. In either case, the search results weren't quite right.

So, in true geek fashion, I set out to find out why. My first stop? The terminal, to trot out some Unix commands that manipulate Spotlight. First, I looked up some information on `mdutil`, and found that I can reset the Spotlight index using this command:

    mdutil -E <path/to/volume>

I didn't want Spotlight indexing my external Firewire hard drive or my iPod, so I added them as exclusions using the Spotlight preference pane in System Preferences, then issued the following commands:

    mdutil -E /Volumes/Maxtor
    mdutil -E /Volumes/iPod
    mdutil -E /

This reset the Spotlight index. However, I still wasn't convinced that all was well. I turned next to `mdimport`, another Unix command, and found that you can force Spotlight to tell you which importers were installed and recognized. Using `mdimport -L`, I found that only the importers found in `/System/Library/Spotlight` or in an application bundle were actually being recognized. However, there were some importers found in `/Library/Spotlight` that were not being recognized. I copied one of these importers over to `/System/Library/Spotlight`, and `mdimport -L` showed it as being recognized. I copied the remaining importers over. Another problem resolved---now all the importers were being recognized.

However, these importers hadn't been used when the index was being rebuilt, and now i needed a way to tell Spotlight to update itself with these new importers. Another look at the man page for `mdimport` showed another switch that would be useful. So I ran these commands to fix the problem:

    mdimport -r <path/to/importer>

I gave Spotlight a little bit of time to update itself, then issued a quick Spotlight query for some text I knew would be buried inside a Microsoft Office document. The results included the document(s) I expected, which told me that things appeared to be working much better.

The only thing not working (not yet, anyway), was the [del.icious Spotlight Importer](http://ianhenderson.org/delimport.html) (to search [del.icio.us](http://del.icio.us/) bookmarks via Spotlight). Still need to work on that one a bit.
