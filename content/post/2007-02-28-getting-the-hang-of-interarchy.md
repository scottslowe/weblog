---
author: slowe
categories: Explanation
comments: true
date: 2007-02-28T22:02:01Z
slug: getting-the-hang-of-interarchy
tags:
- FTP
- macOS
- Networking
- SFTP
title: Getting the Hang of Interarchy
url: /2007/02/28/getting-the-hang-of-interarchy/
wordpress_id: 421
---

[Interarchy](http://www.nolobe.com/interarchy/) is a well-respected FTP/SFTP client for [Mac OS X](http://www.apple.com/macosx/), although the feature set now makes calling it an "FTP/SFTP client" a bit of a misnomer. After, when you add in HTTP, WebDAV, and Amazon S3 support, it's not exactly just an FTP/SFTP client any longer. (For simplicity's sake, we'll continue calling it an FTP/SFTP client.) For users of other FTP/SFTP clients, however, Interarchy's interface is different enough to throw users off and prevent them from really being able to take advantage of the application's full functionality.

Now that I've been using it for a couple of weeks, though, I'm starting to get the hang of the application, and beginning to understand (what I think) are the concepts behind the application. In the event that other new users of Interarchy may find it useful, here are my thoughts. I welcome additions, clarifications, or corrections from users more experienced than myself.

## Multiple Windows

One difference that seems to throw new users of Interarchy is the use of multiple windows. This didn't really throw me at first, as I naturally use lots of windows anyway. The idea here is that instead of using a two-paned "Midnight Commander"-type interface, Interarchy presents different windows for different remote locations, just like the Finder can present different windows for different file system locations. In this respect, Interarchy is more Mac-like than most of the other similar applications out there. (Side note: If you don't like multiple windows, you can use multiple tabs in a single window instead.) If you've ever used Linux with [KDE](http://www.kde.org/) or [GNOME](http://www.gnome.org/), then Interarchy's windows with the URL address bar will look very familiar to you.

In addition, Interarchy has windows to report the status of mirror tasks, a transfers window, a transcript window, and of course the default Bookmarks window.

## Bookmarks

Interarchy is built around the idea of _bookmarks_. "Bookmark" isn't the best name for this concept, but I can't think of anything better either. These bookmarks combine a set of basic building blocks (more on that in a moment) to create the desired result. This makes Interarchy's bookmarks much more than the bookmarks of a web browser; indeed, these bookmarks are more like Automator workflows. (Hey, that's a really good analogy.)

You'll see this analogy carried throughout the application. The "Connect to Server..." window, in which you can define "on-the-fly" bookmarks, is itself listed as a bookmark (although it's a special bookmark that uses the "interarchy:" URL type).

## Basic Building Blocks

Bookmarks in Interarchy combine three basic elements: protocol, action, and parameters.

* Protocol: Here you define the network protocol that should be used, such as FTP, SFTP, Amazon S3, HTTP, or WebDAV. It's kind of handy that the application and its bookmarks present a consistent interface regardless of the protocol(s) being used.

* Action: Once the protocol is selected, you must then select the action.  Actions include List (equivalent to a "traditional" connection that shows the contents of the selected server), Download, Upload, Mirror, New Net Disk, New Auto Upload, View, Edit, and others. List is probably the one you'll use the most.

* Parameters: These will vary between actions. For the List action, all you really need is the server name, path, username, and password. For the Upload action, you'll need those four parameters plus a source folder.

Mixing and matching these three building blocks allows you to build bookmarks (custom workflows) to do exactly what you want to do.

Here's an example. I maintain the [web site](http://www.knightdalecog.org/) for my church. It's not much (I'm not an HTML programmer by any stretch of the imagination), but God honors the effort anyway. I have three bookmarks defined to help manage the church web site:

1. A List bookmark, which allows me to navigate the filesystem where the web site's files are stored, edit files, etc.

2. A Mirror Download bookmark, which downloads an exact copy of the web site to my [MacBook Pro](http://www.apple.com/macbookpro/).

3. A Mirror Upload bookmark, which uploads the local copy of the web site to the remote server.

Using these bookmarks, I can quickly and easily download a mirror copy of the web site, edit it locally, then publish it back up to the server with just a few clicks. (Yes, I could probably do this easier with a straight-up Mirror task, but I'm not quite ready to go there yet. I like control.) Other applications have the same kind of synchronization functionality ([Cyberduck](http://cyberduck.ch/) does), but they require you to first establish a connection to the server, then initiate the synchronization. Interarchy allows you to just do the task in one step.

Hopefully, you've found this information at least a little but useful. I'm sure that as I continue to use Interarchy, I'll continue to find new ways of streamlining these types of tasks. Anyone have any tips they care to share?
