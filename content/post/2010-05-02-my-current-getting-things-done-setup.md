---
author: slowe
categories: Explanation
comments: true
date: 2010-05-02T15:42:08Z
slug: my-current-getting-things-done-setup
tags:
- macOS
- Personal
- Productivity
title: My Current Getting Things Done Setup
url: /2010/05/02/my-current-getting-things-done-setup/
wordpress_id: 1910
---

In early 2008, I started down the path of using David Allen's Getting Things Done (GTD) methodology. I noted the [start of my GTD journey][1] in early February 2008, and then posted [an update later that month][2] after I'd settled in to some of the tools. This weekend I spent some time tightening some AppleScripts I'd written and it made me think again about how I use various applications and tools in my methodology. I thought it might be useful information to share with other Mac users.

There are two primary applications in my setup: [OmniFocus](http://www.omnigroup.com/products/omnifocus/) and [Yojimbo](http://barebones.com/products/yojimbo/). I use OmniFocus for organizing _commitments_--those things that I've committed to do for myself or someone else. This would include projects at work, deadlines for my publisher, tasks to complete at home, calls to make, e-mails to send, or anything else that I need to do. I use Yojimbo for organizing _information_---the URL that I need for an upcoming project, the contents of that e-mail message that gave me some information I have to save, or that JPEG image that has useful information in it. I would say that together, these two form the basis of my GTD system. Let's call them the organization layer.

There are two other "layers" of applications that I use: the consumption layer and the creation layer. The consumption layer are all those applications that help me consume information and generate commitments. That would include the built-in Mac OS X mail client (with the [MailTags add-on](http://indev.ca/MailTags.html))for e-mail, [NetNewsWire](http://www.newsgator.com/individuals/netnewswire/default.aspx) for RSS feeds, [Camino](http://www.caminobrowser.org/) for browsing web sites, [Twitterrific](http://twitterrific.com/) for tweets, [Colloquy](http://colloquy.info/) for IRC, [Adium](http://adium.im/) for instant messaging, and [Unison](http://panic.com/unison/) for Usenet newsgroups (to name a few). At the other end are the applications in the creation layer; these are the applications that help me create new pieces of information or complete my commmitments. Obviously, some the applications I mentioned already also play here; other applications would include [MarsEdit](http://www.red-sweater.com/marsedit/) for creating blog posts, or [Microsoft Office 2008](http://www.microsoft.com/mac/default.mspx) for generating documents.

In my opinion, the only way to be truly successful with GTD is to make it quick, easy, and seamless to get information into your system (whatever that system is). In other words, it has to be quick and easy and non-intrusive to get information from your consumption layer to the organization layer. In my case, I need to be able to get information into OmniFocus and Yojimbo easily. To do that, I turned to AppleScript.

I wrote a series of AppleScripts that do the following things:

* Capture the URL from the frontmost Camino browser window and create a new bookmark in Yojimbo

* Capture the URL from the frontmost Camino browser window and create a new action in the OmniFocus Inbox with the URL in the notes (I also have a Safari version of this script)

* Create a new note in Yojimbo from the content of a selected e-mail message in Mail

* Create a new action in the OmniFocus Inbox that contains a summary of the selected e-mail message from Mail

* Create a bookmark in Yojimbo from the currently selected NetNewsWire RSS headline

* Take the URL from the selected headline in NetNewsWire and create an action in the OmniFocus Inbox with the URL in the notes

* Capture the currently selected tweet in Twitterrific and create a new note in Yojimbo with the contents of the tweet

* Create an action in the OmniFocus Inbox with the text of the currently selected tweet in Twitterrific in the notes of the action

It used to be that I invoked these scripts via Quicksilver, but I recently re-discovered [FastScripts](http://www.red-sweater.com/fastscripts/), by the developer of MarsEdit. FastScripts allows me to bind an application-specific hotkey to these scripts, so that a quick key combination pushes data from NetNewsWire, Mail, or Twitterrific into Yojimbo or OmniFocus. It's quick, easy, and seamless, and allows me to capture information without really requiring a change of focus or without really interrupting my workflow.

Once the information is captured in either Yojimbo or OmniFocus, then I organize the information as needed---assign tags in Yojimbo and place OmniFocus actions into the appropriate projects and contexts. I also have recurring actions in OmniFocus to review the content in Yojimbo. This system also greatly facilitates an "Inbox Zero" approach, since whatever information I need is either a) archived in Mail for long-term reference, b) captured in Yojimbo for use later, or c) pushed into an action in OmniFocus. There's no longer a need to use my Inbox to help remind me of what I need to do, so it's easier to keep my Inbox empty.

While it's tremendously easy to get information from the consumption layer into the organization layer in an automated fashion, it's not quite so easy to automate getting information from the organization layer to the creation layer. (I suspect if it were easy to automate then many of us wouldn't have jobs!) Nevertheless, I did create a couple AppleScripts to automatically post URLs from either Yojimbo or Camino to [Pukka](http://codesorcery.net/pukka) (and thus to [my Delicious.com bookmarks](http://delicious.com/slowe)). NetNewsWire already had that ability, so no AppleScript was needed there. Beyond that, it's a simple matter of using the information stored elsewhere to do my job, create documents, write blog posts, prepare (and deliver) presentations, and so forth---to complete my commitments as noted in OmniFocus. As my commitments are completed, they are marked as completed in OmniFocus and then archived monthly. And, of course, there are the weekly reviews of all my actions, projects, and contexts; this is a key component to making sure that my system is "trusted" and that I can focus on this one task without forgetting about all my other tasks.

So, in a nutshell, that's the state of "Getting Things Done" on my Mac today. I'd love to hear from other Mac users on how they are using various applications and tools to help them be effective; perhaps I can learn a few tricks! Feel free to share in the comments.

[1]: {{< relref "2008-02-06-getting-things-done-on-my-mac.md" >}}
[2]: {{< relref "2008-02-16-getting-things-done-on-my-mac-part-ii.md" >}}
