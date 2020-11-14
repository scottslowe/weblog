---
author: slowe
categories: Explanation
comments: true
date: 2013-06-07T09:00:00Z
slug: reducing-the-friction-automating-file-management
tags:
- macOS
- Productivity
title: 'Reducing the Friction: Automating File Management'
url: /2013/06/07/reducing-the-friction-automating-file-management/
wordpress_id: 3206
---

One of the key things you can do to help improve your efficiency is "reduce the friction"---make common tasks easier, simpler, faster. (This isn't my phrase or idea, by the way, but I've forgotten exactly where I first saw it.) In [my last Reducing the Friction post][1], I talked about how I use AppleScript to help automate part of my blog publishing process. In this post, I'll share how I help automate some common file management tasks.

The tool that I use here is [Hazel](http://www.noodlesoft.com/hazel.php), a nifty rules-based file management tool. Hazel uses a set of "if-then" rules to automatically perform tasks on a file based on a set of conditions. I use Hazel for a number of things, but the primary use case I'll highlight here is automatically archiving files. I use this same technique in two different ways:

1. To automatically archive files I no longer need/want

2. To automatically archive published blog posts

The basic rule is the same in both instances, although the trigger is different. Here's a screenshot of the Hazel rule for the first usage, archiving files I no longer need/want:

![Hazel rule screenshot](/public/img/2013-06-07-rtf-archive-file-rule-01.png)

Let me break that down real quick:

* The rule condition states that a file must have been assigned the blue color label in the Finder (which, in my system, is what I use to mark a file as no longer needed/wanted) in order for the rule to be fired.

* Once the rule is fired, then Hazel will move the file to the Archive-Pending folder and clear the color label from the file.

So, to archive files to my Archive-Pending folder (which I periodically move off my laptop to my home NAS for longer-term storage), I simply assign it the blue color label and Hazel does the rest. By the way, you'll note that this rule is attached to the ~/Documents folder.

I use the same basic rule, with one small tweak, for the second usage I listed (automatically archiving published blog posts):

![Hazel rule screenshot](/public/img/2013-06-07-rtf-archive-file-rule-02.png)

As you can see, the rule actions are the same (move the file to the Archive-Pending folder and clear the color label), but the trigger is different. In this particular case, I use the green color label, which I've named "Finished" in my Finder preferences, to trigger the rule action. This rule is attached to my Blog-Drafts folder.

So, when I'm done with a blog post, I mark the Markdown file with the Finished (green) label, and Hazel automatically moves the post to my Archive-Pending folder. This helps ensure that the Blog-Drafts folder only contains unpublished/in-progress drafts, and also helps ensure that I have an archive of all my blog posts (an archive that is _separate_ from backups of the WordPress database). This second reason, by the way, is one of the reasons I use the publishing process I use instead of just using the WordPress editor (although that is a perfectly fine way of doing it, if that works for you).

So what's next? Hazel has the ability to watch a file's activity (based on last opened or last modified dates), so in theory I could have Hazel start automatically moving "old" or "inactive" files to the Archive-Pending folder as well. I'm a bit hesitant to do that right now, but it is an option I might explore in the future.

I'd love to hear any other thoughts readers might have on "reducing the friction" through automating parts of the file management process (either with Hazel or with other tools). Feel free to add your thoughts, ideas, or suggestions in the comments below.

[1]: {{< relref "2013-05-31-reducing-the-friction-bbedit-to-marsedit.md" >}}
