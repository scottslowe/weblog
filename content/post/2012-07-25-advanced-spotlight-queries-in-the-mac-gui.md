---
author: slowe
categories: Tutorial
comments: true
date: 2012-07-25T09:23:35Z
slug: advanced-spotlight-queries-in-the-mac-gui
tags:
- CLI
- macOS
- Productivity
title: Advanced Spotlight Queries in the Mac GUI
url: /2012/07/25/advanced-spotlight-queries-in-the-mac-gui/
wordpress_id: 2695
---

I mentioned on [Twitter](http://twitter.com/scott_lowe) a couple days ago that I was mulling a switch from [EagleFiler](http://c-command.com/eaglefiler/) to a pure file system-based approach leveraging [OpenMeta](https://code.google.com/p/openmeta/) tags. Long-time readers may recall that it was only September of last year that I [switched to EagleFiler from Yojimbo][1], my previous "anything bucket". My decision to switch away from EagleFiler is not a reflection on the application itself; it's a great application. For me, it just seemed as if EagleFiler was duplicating functionality I could already get with just the file system, so what's the point? Plus, as I increasingly keep data synchronized across multiple systems, EagleFiler wasn't as friendly to my data synchronization solutions as I would have liked. A file system-based approach leveraging OpenMeta tags is perfectly happy with both [Dropbox](http://www.dropbox.com) and [Unison](http://www.cis.upenn.edu/~bcpierce/unison/).

The OpenMeta tags are really the key; using OpenMeta tags, I can set up saved searches that easily let me drill down to only certain subsets of files regardless of where on my computer those files might be stored. However, the problem that I was running into was that the Mac OS X GUI did not provide a way for me to find all _untagged_ files (obviously, the key to making tags work effectively is using tags everywhere). For that, I had to drop out to the OS X command line and use the `mdfind` command, like this:

    mdfind -onlyin ~ "kOMUserTags != '*'"

The `kOMUserTags` is the name of the extended attribute in which OpenMeta tags are stored. This particular query finds files that don't have any OpenMeta tags. Unfortunately, there didn't seem to be any way to bring this query (and the query results) into the GUI.

After some further experimentation, I grew frustrated with how things were progressing (or not progressing, in this instance) and thought I'd try to dig up some documentation on the syntax for `mdfind`. I didn't find the documentation---but I did find [this page](http://hints.macworld.com/article.php?story=20050427030707455), which gave me two important pieces of information:

1. It showed me the syntax to query on both filename as well as extended attributes at the same time.

2. It showed me how to bring these queries into the Finder GUI.

Let's take a look at the first one. To query on both filename _and_ one or more extended attributes, you can't use the `-name` parameter to `mdfind`. Instead, you have to use this syntax:

    mdfind -onlyin ~ "(kMDItemFSName == '*.txt') && (kOMUserTags != '*')"

That query will show you all the text files that don't have any OpenMeta tags applied. Obviously, you could customize the filename specification or the particular extended attributes you wanted to search, but you get the idea.

The second thing I found was how to bring these advanced queries into the Finder GUI. The trick is in enabling "Raw Query" on the pop-up menu in the Spotlight search menu:

1. Open a Spotlight search window by pressing Cmd-F while Finder is active.

2. Click the "Kind" pop-up menu and select "Other".

3. It may take a moment, but in a bit a window will open with all of the various types of Spotlight metadata. Scroll through the list to find Raw, then place a checkmark in the "In Menu" column. (By the way, this is also how to add Tags to the menu for use in Spotlight searches.)

Now, in a Spotlight search window (and that includes new Smart Folders), you can click the pop-up, select Raw Query, and then type your raw `mdfind` query like the one above, and it will be displayed in the Finder. Cool! Using this Raw Query support allows me to build extremely sophisticated Smart Folders.

For example, I've already shown you the query to list all untagged files (very first example in this post) and the query to list all untagged files of a certain type (second example). Here's the query to list files that have one tag but not another tag:

    mdfind -onlyin ~ "(kOMUserTags == 'Tag1') && (kOMUserTags != 'Tag2')"

You could, naturally, combine this with a filename filter to get even more specific. I hope this helps someone else out there fine tune their Spotlight searches as well!

[1]: {{< relref "2011-09-07-switching-to-eaglefiler.md" >}}
