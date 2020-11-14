---
author: slowe
categories: Explanation
comments: true
date: 2011-06-23T15:40:05Z
slug: using-gnu-screen-with-ssh
tags:
- CLI
- macOS
- UNIX
title: Using GNU Screen with SSH
url: /2011/06/23/using-gnu-screen-with-ssh/
wordpress_id: 2322
---

This post is probably old news to experienced UNIX sys admins, but I thought the information might be useful to less knowledgeable folks like me. I also hope that the resulting conversation will help uncover even more knowledge we can all put to good use.

I've messed around with the `screen` utility off and on for a while. One thing I'd never quite figured out, though, was how using `screen` helped with SSH sessions. I kept seeing references to using `screen` to help keep things running when you needed to disconnect from an SSH session. That seems like a useful feature, so I decided to dig into it and see what I could figure out.

In the end, what I figured out was this:

* I needed to install `screen` on the remote host(s). In my case, the remote hosts were OpenBSD (I removed the secret back doors), so a quick `pkg_add` corrected that issue.

* I had to recreate my `.screenrc` file on the remote host(s). Fortunately, my `.screenrc` is very simple---it only enables the ability to use the iTerm2/Terminal scrollbar to scroll back and increases the scrollback buffer---so that was no big deal.

With these changes in place, you can then use this command to connect to a remote host:

	ssh -t <server.domain.name> screen -R

On the first connection, this command will create a new `screen` session. When you're done with this SSH session and want to disconnect, just detach from the `screen` session (typically using Ctrl-a d). That also disconnects the SSH session, but here's the kicker: your `screen` session is still running---as are any processes you had running in that session.

When you go to reconnect, use the same command again and it will reconnect you to your existing `screen` session and you'll be right back where you left off. Pretty handy!

&lt;aside&gt;By the way, the `-t` in the SSH command is necessary; without it, you'll get a "Must be connected to a terminal" error message and it won't work properly.&lt;aside&gt;

I'm sure this barely scratches the surface of the useful tricks one could perform using `screen`, so I challenge any and all readers to submit other useful tricks in the comments below. Or, if there is a better way of doing what I'm discussing in this article, please speak up!
