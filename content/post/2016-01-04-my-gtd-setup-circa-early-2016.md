---
author: slowe
categories: Information
comments: true
date: 2016-01-04T00:00:00Z
tags:
- Personal
- Organization
- macOS
- Productivity
title: My Getting Things Done Setup, Circa Early 2016
url: /2016/01/04/my-gtd-setup-circa-early-2016/
---

Almost six years ago I shared [my (then) current Getting Things Done (GTD) setup][xref-1], in which I described how I used various tools, techniques, and applications to try to maximize my productivity. I'd been toying with updating that post, but I wasn't sure that anyone would find it useful. However, a recent e-mail from a reader indicated that there probably _is_ some interest; with that in mind, then, here's an update on my GTD-like setup, circa early 2016.

Before I dive into the details, a couple quick notes:

* First, I call this a "GTD-like" setup because it doens't necessarily strongly adhere to _all_ the tenets of Getting Things Done. I've adapted the system to fit my particular role and responsibilities, which is something I strongly encourage every reader to also do.
* Although I've previously discussed [moving away from OS X][xref-2] (and this is something that I continue to evaluate and explore), this is---for now---a decidedly Mac-specific system. It's probably possible to emulate a similar system on other platforms, but I leave that as an exercise for interested readers.

If you read the 2010 post, you may recall that I think of my workflow as having three "layers" of applications:

* The "consumption" layer: This is where information to be processed enters the system (i.e., is consumed). Applications in this layer are things like Apple Mail (for e-mail, obviously, but with [the invaluable MailTags plug-in][link-5]), [NetNewsWire][link-4] (for RSS feeds; I run an older version that still supports AppleScript), [Textual][link-1] (for IRC), [Tweetbot][link-2] (for Twitter), [Adium][link-3] (for instant messaging), and Safari (for web content).
* The "organization" layer: This is where things get organized. I say "things" here because there are two distinct types of items: commitments and information. I believe this distinction is necessary---an incoming e-mail from your boss may result in a commitment (something you need to do) or may just be information (that you need to save and make available). These things need to be handled differently. I'll discuss this in a bit more detail shortly.
* The "creation" layer: This is where I create output based on input arriving via the consumption layer, directed/assisted/guided by commitments and information in the organization layer. Applications I use here include [Microsoft Office 2011][link-7] (when I have to exchange documents with other people; I haven't upgraded to the latest version), [OmniGraffle][link-8], or [Sublime Text 3][link-6] (for writing Markdown, ASCIIDOC, reStructured Text, etc.).

There is also a class of applications and tools that don't fall into these broad categories, but rather serve as "helpers." This would include things like [TextExpander][link-9], [OmniOutliner][link-10], [MindManager][link-11], [FastScripts][link-12], etc.

The consumption and creation layers are pretty straightforward, so I won't talk too much directly about them, with one exception: e-mail. It's critical that you take control of your e-mail, and---as I've said before---[stop _managing_ e-mail and start _processing_ e-mail][xref-4]. In other words, an e-mail message generally isn't anything important in and of itself. Rather, it's what the e-mail message generates: an action (commitment) or some information. _That's_ what really matters, not the e-mail message itself. OK, enough about that...

Instead, let's focus on the organization layer. As I did in 2010, I still use [OmniFocus][link-13] to manage my commitments. (I did [try a plain-text based system][xref-3] as well as re-visit Things, but moved back to OmniFocus.) To make getting commitments into OmniFocus (hereafter just OF) easier, I still use a set of AppleScripts bound to a hotkey (using FastScripts) that will take information from the current application---an e-mail message in Apple Mail, or the current web page in Safari, for example---and create an action in OF. Obviously, some of the scripts I mentioned in [the 2010 article][xref-1] are no longer applicable: Camino died a long time ago, I ditched Yojimbo (more on that in a moment), Twitterrific killed its AppleScript support (and I, in turn, moved to a different client), etc. However, the basic idea is still the same: use OF to keep track of all the various projects and actions (commitments), and make it as easy (frictionless) as possible to get stuff into OF.

Managing information is a bit more challenging in that there are many different _types_ of information. For e-mail messages, I tag them (using MailTags) and file them away (using AppleScripts and hotkeys to streamline the process). For files, I use the file system. (Amazing concept, eh?) In 2010, I used a tool called [Yojimbo][link-14]. Yojimbo had a few quirks that I couldn't work around, so I [switched to EagleFiler][xref-5]. EagleFiler was nice in that it leveraged the underlying file system, which then caused me to ask: why couldn't I just leverage the file system? That led me to a file system-based approach using a defined naming convention (based on the ISO 8601 date format) and [OpenMeta][link-17] tags, which I described a bit [here][xref-6]. When OS X Mavericks came out and I finally upgraded, I moved away from OpenMeta tags to native Finder tags, but the underlying approach remained the same: use the defined structure and naming convention, along with tags, to store information, then use Spotlight and/or [LaunchBar][link-16] (a recent addition to my portfolio) to find the information I need later. This is a process I'm still optimizing through the use of tools like [Hazel][link-18] (to automatically apply tags based on predefined conditions or triggers).

For note-based information, I tried (multiple times) to use a tool like Evernote, but it just didn't/doesn't fit into my workflow. Instead, I use a series of text files stored in a [Dropbox][link-20] folder (using my naming convention and tags, of course). For URLs, I capture the URL as a `.webloc` file (using an AppleScript bound to a hotkey, of course!), which can be opened on OS X or iOS (and, as it turns out, is an XML file so it's not that hard to extract the URL on other OSes as well). Since it's then a file, then it falls back under the guidelines for organizing files (I store these on Dropbox as well).

There you have it---an update on my organizational/productivity setup as of early 2016. If you have any questions or feedback, I'd love to hear them; feel free to e-mail me (my address is on this site) or [hit me up on Twitter][link-19]. Thanks for reading, and I hope you find something useful here!


[link-1]: https://www.codeux.com/textual/
[link-2]: http://tapbots.com/tweetbot/mac/
[link-3]: https://adium.im/
[link-4]: http://netnewswireapp.com/mac/
[link-5]: http://indev.ca/MailTags.html
[link-6]: http://www.sublimetext.com/
[link-7]: https://products.office.com/en-us/mac/microsoft-office-for-mac
[link-8]: https://www.omnigroup.com/omnigraffle/
[link-9]: https://smilesoftware.com/TextExpander
[link-10]: https://www.omnigroup.com/omnioutliner/
[link-11]: https://www.mindjet.com/mindmanager/
[link-12]: https://red-sweater.com/fastscripts/
[link-13]: https://www.omnigroup.com/omnifocus/
[link-14]: http://www.barebones.com/products/Yojimbo/
[link-15]: http://c-command.com/eaglefiler/
[link-16]: https://www.obdev.at/products/launchbar/index.html
[link-17]: https://code.google.com/p/openmeta/
[link-18]: http://www.noodlesoft.com/hazel.php
[link-19]: https://twitter.com/scott_lowe
[link-20]: https://www.dropbox.com/
[xref-1]: {{< relref "2010-05-02-my-current-getting-things-done-setup.md" >}}
[xref-2]: {{< relref "2012-11-30-why-i-might-leave-os-x.md" >}}
[xref-3]: {{< relref "2015-04-06-plain-text-productivity-experiment.md" >}}
[xref-4]: {{< relref "2013-06-21-reducing-the-friction-processing-e-mail.md" >}}
[xref-5]: {{< relref "2011-09-07-switching-to-eaglefiler.md" >}}
[xref-6]: {{< relref "2012-07-25-advanced-spotlight-queries-in-the-mac-gui.md" >}}
