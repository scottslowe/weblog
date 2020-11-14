---
author: slowe
categories: Information
comments: true
date: 2011-05-03T09:00:00Z
slug: applescripts-for-yojimbo-camino-and-skim
tags:
- Apple
- Collaboration
- macOS
title: AppleScripts for Yojimbo, Camino, and Skim
url: /2011/05/03/applescripts-for-yojimbo-camino-and-skim/
wordpress_id: 2285
---

I'm not sure this post will be useful to many readers, but I do know that there are a fair number of readers who are also Mac users, and some have expressed interest in AppleScript and how I use it to "glue" some of my daily applications together for a more seamless workflow. If you're one of those readers, then read on. (If you're not, you can still keep reading. I might sway your mind.)

I use [Yojimbo](http://www.barebones.com/products/yojimbo/) as my "catch all" for any and all sorts or types of information that I find during the day: URLs, text snippets, PDF files, etc. Mostly it's URLs (what Yojimbo calls bookmarks), as I keep the majority of my PDF files in my Dropbox. Over the last month or so, I'd found two little things about Yojimbo that bothered me and interrupted my workflow:

1. There is no easy way to open a group of bookmarks, especially from the keyboard.

2. There is no easy way to open a PDF in Skim, my preferred PDF viewer, instead of Preview.

Fortunately, I was able to use AppleScript to fix both of these problems. First I wrote an AppleScript that takes the selected bookmarks in Yojimbo (it ignores selected items that aren't bookmarks) and opens them in [Camino](http://www.caminobrowser.org/), my browser of choice. You could, of course, easily modify the script to open the URLs in Safari. I built a foreground/background option into the script so that you can open the URLs but leave Yojimbo as the active application, if you so preferred.

Next I wrote an AppleScript that grabs the selected PDF archives in Yojimbo (it ignores anything that's not a PDF archive item), exports them to a temporary folder on my laptop, and opens them in [Skim](http://skim-app.sourceforge.net/). As with the other script, I built a foreground/background option into this script as well, so you can control whether Skim will become the active, foreground application or not.

To make both of these scripts easy to use and easy to access, I stored them in `~/Library/Scripts/Applications/Yojimbo`. This makes them easily accessible from the menu bar or via a keyboard shortcut using FastScripts. These two new scripts join an earlier script I wrote that allows me to easily post a bookmark stored in Yojimbo to Delicious.com via the Mac application [Pukka](http://codesorcery.net/pukka).

If you'd like a copy of the scripts to use for yourself or to modify for your own purposes, click [here][1]. Just remember that I am in no way liable for anything that may happen as a result of using any scripts or code that I post on my site, including increased productivity and greater enjoyment of your computing experience.

[1]: /public/dl/yojimbo-skim-camino.zip
