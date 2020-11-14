---
author: slowe
categories: Review
comments: true
date: 2010-10-02T11:23:38Z
slug: revisiting-evernote
tags:
- macOS
- Productivity
- Web
title: Revisiting Evernote
url: /2010/10/02/revisiting-evernote/
wordpress_id: 2127
---

About two years ago, I [took a look at Evernote][1] (here's the [main Evernote web site](http://www.evernote.com)), which at that time was still in beta. While I was intrigued with the idea of Evernote, at that time I struggled with getting data into Evernote. The Web Clipper didn't seem intuitive to me, and I wrestled with how best to use Evernote within my fledgling productivity system.

Since that time, I settled on the use of [OmniFocus](http://www.omnigroup.com/products/omnifocus/) for organizing commitments and [Yojimbo](http://www.barebones.com/products/yojimbo/) for organizing information (more on how I use these two applications is found in [this update][2] on my Getting Things Done setup). Using AppleScript as the glue between the consumption, organization, and creation layers has been tremendously useful for me. While I still have plenty of room to grow and improve, I feel like the system I've built really helps me stay productive, in part because it's transparent (i.e., it doesn't get in my way).

When I first evaluated Evernote, I wasn't too familiar with AppleScript and I believe that the Mac version of Evernote had very little or no AppleScript support. With recent releases of the Mac Evernote application, their AppleScript support has improved dramatically, and so I thought I should revisit Evernote. Now that I could use AppleScript to help ease the process of getting information into Evernote, perhaps it would be a good fit into my workflow. In addition, I'd gain the ability to have access to my notes from my Mac, my iPhone, my iPad, and any web browser. Just as OmniFocus is available from any of my devices, so too would my information be available.

I was right about the AppleScript part; I was able to relatively easily adapt the scripts I'd written for Yojimbo to work with Evernote. Combined with [FastScripts](http://www.red-sweater.com/fastscripts/), this made capturing a URL from [Camino](http://www.caminobrowser.org) or [NetNewsWire](http://netnewswireapp.com/mac/) into Evernote as simple as pressing a hotkey. (If anyone is interested in the scripts themselves, let me know and I'll make them available.)

Unfortunately, I now find that my system is no longer as transparent as it used to be. The system is now getting in my way. I'll grant you that some of this could be due to the switch from Yojimbo to Evernote. It takes time to grow accustomed to any change, and this is no different. The question then becomes: is it worth the effort to sustain the change? What benefits will switching to Evernote get me, and what challenges will it introduce? I've done a little bit of thinking about this, and here's where things stand currently:

* First, everything in Evernote is a note. This means that I have to take extra steps to separate out different data types in the event that I need to view or act upon only certain data types. Yojimbo, on the other hand, has separate data types for notes, bookmarks, images, etc. Is this a big deal? Not a huge deal, but it does introduce a small amount of additional work if I stick with Evernote.

* Second, Evernote's UI is terribly clunky compared to Yojimbo's. Anytime you do anything with tags, the Tags area in the left-hand pane of the Evernote window expands---even if you don't want it to. Searching for items by tag means using Evernote's extended search syntax, which is buried at the end of the user's guide (you'll need to use something like "tag:ToRead" to find all items tagged "ToRead"). Evernote lacks Tag Explorer-like functionality. There's no Smart Collections (or Smart Folders) functionality in Evernote, although you can use saved searches; unfortunately, Evernote doesn't provide a UI for creating saved searches. All in all, it makes working with Evernote more difficult than performing a comparable task in Yojimbo (in my opinion).

* Third, for Evernote to treat everything as a note, it's note functionality is surprisingly simplistic. If you use fonts and formatting in your Evernote notes, the iOS versions of Evernote can't edit them. (To be fair, this is an Apple iOS limitation.) Even when I attempted to convert notes back to the equivalent of plain text using the Simplify Formatting command, some of the formatting remained, and there did not appear to be any way of correcting that behavior. Even more irritating, converting these notes back to plain text equivalent wasn't detected as a change by the Evernote client, which meant that the updated note wasn't synced up Evernote's online service. In fact, unless I actually edited the note (for example, by adding a character and then removing the character), Evernote wouldn't even save the changes to plain text equivalence.

* Yojimbo lacks the ability to sync data across multiple platforms. Heck, Yojimbo is a Mac-only application---it doesn't have apps for any other platforms, much less the ability to synchronize the data. Keeping data in sync across devices and platforms is, of course, one of Evernote's key features. So, how much is the ability to sync and access data across multiple applications worth? How much of an advantage will this truly offer? I've seen the benefits of having my commitments available on multiple platforms via OmniFocus, and I'm seeing the benefits of keeping my RSS feed synchronized via Google Reader (using NetNewsWire on my Mac and NewsRack on my iPhone and iPad). Will the same benefit hold true for notes?

All things considered, it seems as if I'm finding one potential advantage to Evernote (syncing data across devices) and three known drawbacks (lack of multiple data types, note functionality issues, and an unintuitive user interface). I just can't decide if having information like URLs and brief notes available across devices is really as worthwhile as it's made out to be. I'd love to hear feedback from readers about their viewpoint---has Evernote syncing really been useful? Speak up in the comments below. Thanks!

[1]: {{< relref "2008-07-07-evernote.md" >}}
[2]: {{< relref "2010-05-02-my-current-getting-things-done-setup.md" >}}
