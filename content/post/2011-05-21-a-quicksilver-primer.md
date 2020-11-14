---
author: slowe
categories: Education
comments: true
date: 2011-05-21T22:15:29Z
slug: a-quicksilver-primer
tags:
- Apple
- macOS
title: A Quicksilver Primer
url: /2011/05/21/a-quicksilver-primer/
wordpress_id: 2311
---

Last week, I came the closest I've ever come to leaving [Quicksilver](http://qsapp.com) and switching to an alternative application. It's quite an honor to the almost-replacement, [Alfred](http://www.alfredapp.com), that I even got as close as I did. In the end, though, Quicksilver won out again. During the process, a number of people asked me about Quicksilver, why I use it, and how I use it. After revisiting the reasons for using Quicksilver as part of the evaluation against Alfred, I thought now might be a good time to some answers to these questions.

I've formatted this post in a sort of "question and answer"-style layout. This helps me provide some structure around the discussion and hopefully makes it a bit easier for readers to follow.

**First and foremost: what _is_ Quicksilver, exactly?**

This might actually be the most difficult question to answer. Quicksilver is many things. Yes, it is an application launcher. But it's so much more than just an application launcher. Referring to it just as an application launcher limits it. Quicksilver provides the ability to find a wide variety of objects and perform an action on those objects.  So you can use Quicksilver to find an application and perform the action of opening (launching) that application, among other things.

**So what examples can you give about what sorts of objects and actions I can work with via Quicksilver?**

Quicksilver leverages a modular architecture that supports plug-ins which add functionality to the core application. This is not uncommon; there are a number of Mac OS X applications that provide this sort of functionality. Depending upon the plug-ins that are installed, the sorts of objects that Quicksilver can manipulate include:

* Contact objects from the Mac OS X Address Book

* Files or folders out of the Finder

* Snippets of text from an application, either via the Clipboard or by just being selected in the application

* Songs and playlists in iTunes

* Bookmarks from a web browser like Safari or [Camino](http://www.caminobrowser.org/)

Once you have an object selected, you can then select your action. For example, if you have a contact selected, you can choose to compose a new e-mail to that contact. If you have a file selected, you can choose to send that file via e-mail as an attachment. If you have some text selected and it's a URL, you can choose to open it in your web browser. If it's a bookmark from a web browser (or other applications that use bookmarks and have a Quicksilver plug-in, like [Cyberduck](http://cyberduck.ch/)), you can open the bookmark in the appropriate application. If it's a snippet of text, you can choose to display it in large text on your screen. If the selected object is a song in iTunes, you can choose to play it.

**That sounds really complicated. How hard is it to use Quicksilver?**

Once you understand the basic premise of object/action, Quicksilver is pretty easy to use. You invoke Quicksilver---which by default appears as a partially transparent bezel on your screen---via a customizable hotkey, then simply start typing the name of the object you're seeking. Quicksilver will find it, matching not only on consecutive letters but on letters anywhere in the name. Once you find your object, press Tab and then start typing to select your action. Quicksilver "learns" via your selections which objects you mean, associating certain keystroke combinations with certain objects. This makes Quicksilver's matching more accurate over time. This matching behavior was one of the reasons why I stayed with Quicksilver vs. switching to Alfred. Even though Alfred supported non-consecutive matching, it still wasn't as flexible (for me) as Quicksilver.

**This functionality sounds very similar to what other applications offer. What other features made you stick with Quicksilver?**

Two features really stand out to me. First, there's the "comma trick." Let's say you have two applications you need to launch. Start typing characters until Quicksilver matches the first app, then press comma, and start typing until Quicksilver matches the next app. By using the comma to separate objects, you can invoke several objects at the same time. This is _extremely_ useful. Need to open several applications at once, or open several documents at once? The comma trick can help. The second very useful feature is referred to proxy objects. Let's say you have some text selected in an application, and you want to take that text into Quicksilver and do something with it. Just press Cmd-Esc, and Quicksilver is invoked with the selected text/file/object already selected. Find a file in the Finder, press Cmd-Esc, press Tab, start typing "email", press Tab, start typing the name of the contact you'd like to send this file to via e-mail. Congratulations, you've just starting composing a new e-mail message to a contact in Address Book with a file attached without ever taking your hands off the keyboard.

**So, Scott, how do you use Quicksilver on a day-to-day basis?**

Let's see...Quicksilver has become such a part of my routine it's hard to imagine using a computer without it. Here are some of the ways I use Quicksilver every day:

* I use Quicksilver to access my Camino bookmarks. This makes it easy for me to jump to any website in my bookmarks by simply invoking Quicksilver (I use Option-Space) and then typing a few characters to match the bookmark.

* I use proxy objects to open URLs in text. I simply select the text with the URL, press Cmd-Esc, then press Enter (because the default action for text recognized as a URL is Open). My default browser opens and navigates to that URL.

* I search Google from Quicksilver. I created a custom web search object (called Search Google) that I invoke, then Tab over twice and enter the search terms followed by Enter. A new browser window opens to the search results from Google.

* I launch applications, lots of times using the comma trick.

* I look up details about contacts in my Address Book. Once you have a contact object selected, pressing the right arrow key "opens" the details for the contact object so that individual fields become objects that can be manipulated. I can then select a contact's e-mail address and compose a new e-mail to that address, for example.

There's a few examples; hopefully that helps.

If you're interested in Quicksilver, have a look at [the Quicksilver wiki](http://qsapp.com/wiki/Main_Page). There's some useful information there. In the meantime, if you think Quicksilver might be something that will help you become more efficient, [download it](http://qsapp.com/download.php) and give it a try.

If you have additional information to share (perhaps you're an existing Quicksilver user), please feel free to speak up in the comments. Feedback, dialog, and contributions are always welcome.
