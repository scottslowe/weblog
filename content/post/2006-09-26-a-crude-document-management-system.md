---
author: slowe
categories: Information
comments: true
date: 2006-09-26T17:03:24Z
slug: a-crude-document-management-system
tags:
- Macintosh
- Office
- Tagging
- Tags
- Productivity
title: A Crude Document Management System
url: /2006/09/26/a-crude-document-management-system/
wordpress_id: 340
---

My rudimentary "document management system" is comprised of a variety of technologies, some built into [Mac OS X](http://www.apple.com/macosx/) and some added on via separate third-party applications. At the core of the system is [Spotlight](http://www.apple.com/macosx/features/spotlight/), the search engine that is the object of a love-hate relationship with many Mac users. Despite Spotlight's shortcomings, it is still a very useful tool to have. (I will have to admit that I am sure I am still not taking full advantage of Spotlight's abilities.)

On top of Spotlight I've added a number of pieces:

* _Spotlight comments in Finder:_ I place relevant information in the Spotlight comments in Finder so that the Spotlight engine can find and index these comments. Keywords/tags are separated by spaces and surrounded by brackets (i.e., "[Macintosh Security]") and additional comments---such as a brief description of the document---follow immediately thereafter.

* _Project-specific keywords/tags:_ I have project-specific keywords/tags for the major customer projects in which I am involved. For example, if I were handling an Exchange migration for XYZ Widgets, my project-specific keyword would be "Proj:XYZ-ExMig". This makes it incredibly easy to locate documents relating to a specific document.

* _MailTags:_ [MailTags](http://www.indev.ca/MailTags.html), a handy add-on for [Mail](http://www.apple.com/macosx/features/mail/), lets me apply some of the same metadata to my e-mail messages as to my file system. Then, when I assign a particular message to a project (using the same naming convention as my project-specific keywords mentioned above), I can search for both project-related documents and project-related messages with a single Spotlight search.

* _Keywords/Tags in Address Book and iCal:_ While not specifically related to document management _per se_, using the same keywords/tags on [Address Book](http://www.apple.com/macosx/features/addressbook/) contacts and [iCal](http://www.apple.com/macosx/features/ical/) appointments further unifies the structure and makes it easier to see all related resources. In addition, it also makes it easy to use the Spotlight system service to find related documents when viewing a contact or an appointment.

* _Automator workflow for adding Spotlight comments:_ To help streamline the process of adding these comments to files in Finder, I've added an [Automator](http://www.apple.com/macosx/features/automator/) workflow that prompts me for the text to add to the Spotlight comments and then either appends that text to the existing comments or replaces the existing comments with what I specify. This makes it easy to tag large numbers of documents at the same time.

* _Microsoft Office metadata:_ I also fill out the metadata for all Office documents (via File > Properties). This is something I've been doing for years (since the introduction of the "Find Files" command in Word for Windows 2.0, circa 1994-1995), so it's very natural for me. All of the Office applications are configured to automatically prompt me for document properties when I save a new document. In the document properties, I specify the same keywords/tags as in the Spotlight comments as well as author, manager, title, description, etc.

* _Saved searches:_ To help group together documents, I use a small selection of saved searches---aka Smart Folders---based on raw Spotlight queries. Using raw Spotlight queries allows a finer level of control over what items are returned by the search. For example, the following Spotlight query shows me only active project-related documents:

	(kMDItemFinderComment =*Proj*) && (kMDItemFinderComment != *Inactive*)

I'm still fine-tuning my use of raw Spotlight queries, trying to make sure that I stick to indexed metadata so that Spotlight doesn't have to perform a real-time search everytime I open the Smart Folder. I'd appreciate anyone who has some tips there to share them in the comments.

So, based on my own personal workflow and these additional tools, it's easy for me to create a new project-related document (say, a migration plan or a network diagram), attach the associated metadata (either manually or via the Automator workflow), and have easy and quick access to the document afterward, without really having to pay too much attention to where I saved the document.

I do still use regular old folders (directories) in Finder as well; I'm not sure if that's a holdout of the "old school" way of doing things, but I'm just not ready to completely give up traditional folders just yet.

Changes and/or additions that I'm considering adding include:

* _Using Quicksilver's "File Tags" plug-in:_ As my use of Quicksilver has continued to increase (I'm hoping to now start using it to track and access [my del.icio.us bookmarks](http://del.icio.us/slowe), since [Cocoalicious went belly up][1]), I've also explored the use of Quicksilver for tagging. My main concern with Quicksilver's tagging module is the need for a prefix that is added to every tag. Right now I don't use any sort of prefix, and you can't (unfortunately) tell it not to use a prefix. Perhaps I'll use Quicksilver to create a trigger for my Automator workflow...

* _Changing keyword/tag format:_ Partially because of Quicksilver (see previous bullet), I'm thinking of adding some sort of prefix to denote keywords/tags as such, instead of just part of the Spotlight comments or the document's content. The idea is that searching for "@Proj:XYZ-ExMig" would get only files tagged as such, not all items that may have that content inside them. I still haven't quite decided on this one. Anyone have any thoughts? Would it really be worth the effort?

So, there you have it...my homegrown document management system. I'm open to suggestions for improvement or additional tools to make it more effective. What am I missing that could make this better?

[1]: {{< relref "2006-09-21-cocoalicious-woes.md" >}}
