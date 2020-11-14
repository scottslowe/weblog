---
author: slowe
categories: Explanation
comments: true
date: 2013-12-20T09:00:00Z
slug: reducing-the-friction-training-a-spam-filter
tags:
- Automation
- macOS
- Personal
- Productivity
- Messaging
title: 'Reducing the Friction: Training a Spam Filter'
url: /2013/12/20/reducing-the-friction-training-a-spam-filter/
wordpress_id: 3391
---

I'm back with another "Reducing the Friction" blog post, this time to talk about training an e-mail spam filter. As you may recall if you read one of the earlier posts (may I suggest [this one][1] and [this one][2]?), I use the phrase "reducing the friction" to talk about streamlining and simplifying commonly-performed tasks so as to allow you to improve your own personal efficiency and/or adopt more efficient habits or workflows.

I recently moved my e-mail services off Google and onto [Fastmail](http://www.fastmail.fm/). (I described the reasons why I made this move in [this post][3].) Fastmail has been _great_---I find their e-mail service to be much faster than what I was seeing with Google. The one drawback, though, has been an increase in spam. Not a huge increase, mind you, but enough to notice. Fastmail recommends that you help train your personal spam filter by moving messages into a folder you designate, and then telling their systems to consider everything in that folder to be spam. While that's not hard, it's also not very streamlined, so I took up the task of making it even easier and faster.

(Note that, as a Mac user, most of my tips focus on Mac applications. If you're a user of another platform, I do apologize---but I can only speak about what I use myself.)

To help make this easier, I came up with a bit of AppleScript:

``` applescript
-- This script moves messages to a spam training folder
-- Set some default values to be used later in the script; not all values may be used
property destMailbox : "Spam Training"
property destAccount : "example.com"

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
            move theMessage to mailbox destMailbox of account destAccount
        end repeat
    end tell
end run
```

You can download this script [from GitHub][gist-1], if that's helpful.

To make this work on your system, all you need to do is just change the two `property` declarations at the top. Set them to the correct values for your system.

As you can tell by the comments in the code, this script was designed to be run from within Apple's Mail app itself. To make that easy, I use a fantastic tool called [FastScripts](http://www.red-sweater.com/fastscripts/) (_highly recommended!_). Using FastScripts, I can easily designate an application-specific shortcut key (I use Ctrl-Cmd-S) to invoke the script from within Apple Mail. Boom! Just like that, you now have a super-easy way to both help speed up processing your e-mail as well as helping train your personal spam filter. (Note: if you are also a FastMail customer, refer to the FastMail help screens while logged in to get more details on marking a folder for spam learning.)

I hope this helps someone out there!

[1]: {{< relref "2013-06-21-reducing-the-friction-processing-e-mail.md" >}}
[2]: {{< relref "2013-07-19-reducing-the-friction-processing-e-mail-part-2.md" >}}
[3]: {{< relref "2013-12-04-divorcing-google.md" >}}
[gist-1]: https://gist.github.com/scottslowe/7990921
