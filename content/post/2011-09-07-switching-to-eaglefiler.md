---
author: slowe
categories: Information
comments: true
date: 2011-09-07T23:51:09Z
slug: switching-to-eaglefiler
tags:
- macOS
- Personal
- Productivity
- UNIX
title: Switching to EagleFiler
url: /2011/09/07/switching-to-eaglefiler/
wordpress_id: 2413
---

Over the last month or so, I've taken a strong interest in moving a fair number of my files that are predominantly text-based back to "standards-based" formats such as RTF and plain text. I've started using [Markdown](http://daringfireball.net/projects/markdown/syntax) as a means of storing formatting information in plain text files, and then using tools like [Pandoc](http://johnmacfarlane.net/pandoc/index.html) to convert these Markdown files into the desired destination format. I'll likely discuss this in more detail in a future post, but what I wanted to discuss here was the affect of this decision on my software usage.

If you've read any of the posts I've published on [my Getting Things Done setup][1], you'll know that I used an application called Yojimbo as my "anything bucket." [Yojimbo](http://www.barebones.com/products/yojimbo/) is a native Mac OS X application that operated as part of the consumption phase of my workflow and provided a way for me to collect and organize all the various bits of information that pass in front of me. Yojimbo is a pretty handy application, and I made it even more handy with some home-grown AppleScripts that made it easier and faster to get information _into_ and then back _out of_ the application.

However, I recently started examining other applications in the same space as Yojimbo, in an effort to ensure that I was using the most effective tools possible. (Consider this a "sharpening the saw" exercise.) I evaluated [DEVONthink Pro](http://www.devon-technologies.com/products/devonthinkpro/) and [EagleFiler](http://c-command.com/eaglefiler/), testing each of them within my workflow to see if either of them added some value above and beyond what I currently had with Yojimbo. This was occurring at the same time that I started shifting my text-based formats back to plain text, RTF, and Markdown, and so part of the evaluation process was testing how well those applications fit into this new way of managing my text-based data.

What I found, surprisingly, was that EagleFiler was a great fit for this new workflow. One of my long-time complaints of Yojimbo was that I couldn't use my preferred applications ([Skim](http://skim-app.sourceforge.net/) for PDFs or [TextMate](http://macromates.com/) for text-based files), an issue that was even more of a problem now that I was making greater use of TextMate with plain text files and Markdown. I explored ways of using AppleScript to modify Yojimbo's behavior, but it was beyond my simple AppleScript skills. EagleFiler, on the other hand, simply leveraged the default applications I used with Mac OS X. PDFs opened in Skim, text files opened in TextMate (where I could then use TextMate bundles to convert formats between HTML, plain text, and Markdown), and RTF documents opened in [Bean](http://www.bean-osx.com/Bean.html) (which I'd adopted as a lightweight editor over the oh-so-bulky Microsoft Word). This made it a great fit for the new way I was working with documents. In addition, EagleFiler came with some useful capture functionality built-in, eliminating the need for some of my home-grown AppleScripts. Finally, EagleFiler used an "open" library format that stored my items as files in the file system. If, for whatever reason, I ever decided to ditch EagleFiler, all my information would be easily accessible. This was a real attraction for me.

So, after only a week or so of testing, I switched completely away from Yojimbo and started using EagleFiler instead. Thus far, I've been quite pleased with the results. While it seems simple, I like the ability to mark items as unread (something I couldn't do in Yojimbo, so I had to approximate that functionality with certain tags). I still prefer the way Yojimbo displays metadata about bookmarks in the same window (in EagleFiler you have to open the Inspection window), but this has not been a significant problem.

I also anticipate that the use of the file system will make integrating tools like Pandoc into my workflow possible; it didn't seem possible before with Yojimbo. Because EagleFiler's library is file system-based, it should be possible to use AppleScript to manipulate records by manipulating the underlying files in the file system. This will be an area of exploration for me over the next few months as I also refine my Markdown-Pandoc workflows for document generation.

In my opinion, if you're considering an "anything bucket" for your Mac to help keep your information organized, EagleFiler should definitely be on your list of applications to consider.

[1]: {{< relref "2010-05-02-my-current-getting-things-done-setup.md" >}}
