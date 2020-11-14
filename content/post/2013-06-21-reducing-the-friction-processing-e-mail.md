---
author: slowe
categories: Explanation
comments: true
date: 2013-06-21T09:00:00Z
slug: reducing-the-friction-processing-e-mail
tags:
- Automation
- macOS
- Personal
- Productivity
title: 'Reducing the Friction: Processing E-Mail'
url: /2013/06/21/reducing-the-friction-processing-e-mail/
wordpress_id: 3217
---

E-mail is the bane of efficiency for a lot of professionals, especially IT professionals. In this Reducing the Friction post, I'd like to share a few tips or tricks that might help making processing e-mail a bit easier.

Remember that I like to talk about _processing_ e-mail instead of _managing_ e-mail. There is a subtle distinction there---one (processing) implies a task that is to be performed, perhaps on a regular basis; the other (managing) implies an ongoing process that doesn't end. (Which one would you rather do?)

In [presentations that I've given on personal efficiency](https://speakerdeck.com/slowe/tips-and-tricks-from-a-wannabe-productivity-geek), I provided a set of four steps for processing e-mail. These aren't necessarily my invention; I've culled them together from a variety of sources online and in print. Here they are:

1. If a message contains a task that you can do in under 2 minutes (replying to the message, doing whatever task is in the message, etc.), then just go ahead and do it.

2. If the task found in the message takes more than 2 minutes, put it into your trusted system. (Personally, I use [OmniFocus](http://www.omnigroup.com/products/omnifocus/), but use whatever system makes sense to you.)

3. If, after performing the task or putting it into your trusted system, you think you need the information in the message, then archive the message according to whatever system you prefer.

4. Finally, if after performing the task or putting it into your trusted system you don't need the message, then delete it.

In looking at these steps, it occurs to me that there are 2 places where we can "reduce the friction," i.e., make repetitive tasks easier, and that's in steps #2 and #3. In this post, I'm going to focus on #3. I'll try to address #2 in a future post.

Obviously, how we tackle automating the task of filing messages away according to some system will depend on the system we use. I use a system whereby all messages are filed away by year. All the messages in 2012 get archived into a 2012 folder (or mailbox), messages from 2013 are filed into a 2013 mailbox, etc. To help simplify this process, I wrote an AppleScript that takes the selected messages and moves them to a folder. Here's the script (click [here](https://gist.github.com/scottslowe/5788136) for an option to download the script):

``` applescript
-- This script moves messages to a mailbox for the current year
-- Set some default values to be used later in the script
property theMailbox : "Archive"

-- Handler called when running script from script menu
on run
  tell application "Mail"
        set theSelectedMessages to selection
        if ((count of theSelectedMessages) < 1) then
            beep
            return
        end if
        repeat with theMessage in theSelectedMessages
            if (read status of theMessage) is false then
                set read status of theMessage to true
            end if
            move theMessage to mailbox theMailbox
        end repeat
    end tell
end run
```

For my particular system, I change the value of the variable `theMailbox` every year to correspond to a mailbox for that year (i.e., 2011, 2012, 2013). Then, to further streamline the process, I bound the AppleScript to a Mail.app-specific keyboard shortcut using [FastScripts](http://www.red-sweater.com/fastscripts/), as I outlined in an earlier Reducing the Friction blog post on [using keyboard shortcuts][1]. Now, the process of filing a message into this year's archive folder is a simple keyboard shortcut away. Super easy!

If you use a system where you file to a few different folders, you could use multiple versions of this script each pointing to a different folder. If you use _lots_ of different folders/mailboxesthen this script probably won't help you. (Sorry.)

I think part of the reason my people use lots of different folders if that they use the folders as a way of categorizing messages---each folder represents a particular category, project, person, group, customer, etc. I found an alternate way of handling this categorization process by using a tool called [MailTags](http://indev.ca/MailTags.html). MailTags is an extremely powerful add-in to Mail.app that allows you to assign keywords, projects, due dates, notes, etc., to mail messages. Further, it allows you to build Smart Mailboxes (saved searches) on any of these properties as well. If you use Mail.app, I **highly** recommend MailTags.

As I said, I archive all messages for a given year into a single mailbox. To help with the categorization of messages (or to help provide additional context to every message), I assign at least one keyword to every message. Where applicable, I also assign a project. You can create rules in Mail.app that will automatically assign keywords and/or projects, and I highly recommend that. However, you'll also want to make it very easy to assign keywords manually. Fortunately, MailTags supports AppleScript, so I was able to write a quick script to do this (click [here](https://gist.github.com/scottslowe/5788104) for an option to download the script):

``` applescript
-- This script assigns MailTags keywords to the selected messages
-- Set some default values to be used later in the script
property assignedKeywords : {"Personal"}

-- Handler called when running script from script menu
on run
  tell application "Mail"
        set theSelectedMessages to selection
        if ((count of theSelectedMessages) < 1) then
            beep
            return
        end if
        using terms from application "MailTagsHelper"
            repeat with theMessage in theSelectedMessages
                set keywords of theMessage to assignedKeywords
            end repeat
        end using terms from
    end tell
end run
```

If you want to assign multiple keywords, just modify the script to change the value of `assignedKeywords` to something like this:

    {"Keyword1", "Keyword2", "Keyword3"}

Naturally, you could modify this script to assign a project (I'll leave that as an exercise for the reader), and you could use FastScripts to create an application-specific keyboard shortcut to run the script. This means that you can easily tag your messages with one keystroke, then file them away with another.

When used in conjunction with a good set of rules, I think these scripts can really make step #3 of the four-step mail processing pipeline a lot easier.

Of course, this is just how _I_ work, and your own workflow might be very different. I encourage you, though, to examine your workflow and see if there are ways you could "reduce the friction." I hope this post provides some ideas to help make that happen.


[1]: {{< relref "2013-06-14-reducing-the-friction-using-keyboard-shortcuts.md" >}}
