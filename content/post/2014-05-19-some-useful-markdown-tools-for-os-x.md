---
author: slowe
categories: Information
comments: true
date: 2014-05-19T09:00:00Z
slug: some-useful-markdown-tools-for-os-x
tags:
- macOS
- Productivity
- Writing
- Markdown
title: Some Useful Markdown Tools for OS X
url: /2014/05/19/some-useful-markdown-tools-for-os-x/
wordpress_id: 3454
---

I don't think that it is any secret that I've become a big fan of [Markdown](http://daringfireball.com/markdown/) (MultiMarkdown to be precise; more on that in a moment) over the last couple of years. Likewise, it's not really a secret that I'm an OS X user, although my "fanboi" status for OS X has waned a bit since the introduction of Lion and Mountain Lion (neither of which are as good, in my opinion, as Snow Leopard, but that's an entirely different discussion---the jury is still out on Mavericks). In this post, I want to share with you a few Markdown tools for OS X that I've been using and that (hopefully) you'll find helpful as well.

## BBEdit

Because Markdown is just plain text, a good text editor is essential. For a long time, I was a diehard [TextMate](http://macromates.com/) fan. It was lean, it was fast, and the syntax highlighting for Markdown was really good. Over time, though, as TextMate faded and waned, I found myself in a position of needing to find a new text editor. (Yes, I looked at the TextMate 2.0 alpha releases. I didn't really like where the project was headed, though I'm sure that it's perfectly fine for many folks.) I played with a variety of text editors, and finally settled on [BBEdit](http://www.barebones.com/products/bbedit/), from Bare Bones Software.

One of the key things that attracted me to BBEdit was its extensibility. (This included fairly extensive AppleScript support.) For example, once I'd wrapped my head around how BBEdit works (which meant unwrapping my head from how TextMate worked), it was relatively trivial to incorporate additional AppleScripts and even UNIX scripts into my workflow (here's [a non-Markdown example][1]). I'm still uncovering tips and tricks for maximizing my use of BBEdit, but if you're in the market for a good text editor to use with Markdown, I'd recommend giving BBEdit a serious look.

## MultiMarkdown

It didn't take me too long to stop relying on built-in Markdown processors in my text editors and switch to [the standalone MultiMarkdown processor](http://fletcherpenney.net/multimarkdown/) from MultiMarkdown's creator, Fletcher Penny. In addition to a few syntax additions, MultiMarkdown (MMD) adds more output options---not just HTML, but also OPML and ODF. (ODF can then be easily transformed into RTF or DOC/DOCX using a built-in tool from OS X named `textutil`.)

Because of BBEdit's extensibility---specifically, because of the ability to integrate UNIX scripts into the application---it was a piece of cake to use BBEdit for writing the Markdown, then leverage the standalone MMD processor to convert to the final output format.

One other thing I really like about MultiMarkdown is the ability to add metadata to the document. This allows you to embed keywords, author name, document title, project, affiliation, dates, etc., into the document itself. This has two benefits:

  1. It makes it easier to find the right original documents, since each document now carries additional information that you can use in Spotlight.

  2. When converting to HTML, this information gets embedded into the HTML header.

To make embedding the Markdown metadata easier, I use a [Typinator](http://www.ergonis.com/products/typinator/) snippet.

## QuickLook Generator

Available [here](http://fletcherpenney.net/multimarkdown/download/), this QuickLook generator allows you to preview Markdown/MultiMarkdown files using OS X's QuickLook functionality. This might not seem like a big deal, but I've actually found it quite useful.

## Pandoc

[Pandoc](http://johnmacfarlane.net/pandoc/), from John McFarlane, is a treasure trove of document conversion functionality. It supports an impressive list of both input and output formats. While the standalone MultiMarkdown processor handles most of what I need, having Pandoc in my pocket when I need some additional conversion options is very powerful. For example, I can (and sometimes do) use Pandoc to handle automatic conversions from Markdown/MultiMarkdown directly to Rich Text Format (RTF) or DOC/DOCX. Pandoc also allows you to specify custom templates, so that you could create custom RTF templates that would control how the final RTF document is formatted. All in all, a _very_ powerful tool.

## Marked

[Marked](http://marked2app.com/) is a relatively new addition to my Markdown toolset, although it's been around for a while. The premise behind Marked is simple: preview any Markdown (or MultiMarkdown) document. It turns out this functionality is quite useful (more so than I thought it would be, to be honest). Brett Terpstra, the developer behind Marked, has added features like readability statistics, keyword highlighting, and repetitive word visualization to make Marked even more useful. Another feature I'm finding very handy is the ability to easily export from Marked in PDF, something that is possible using other tools but not nearly as easy. Now, if only I could control Marked via AppleScript, so I could automate that processthat would be ideal.

I hope this information is useful to someone. I've found that using Markdown via some of these tools has really helped my productivity; perhaps you will find the same thing. As always, feel free to post any courteous comments, corrections, or clarifications below.

[1]: {{< relref "2013-11-11-making-json-output-more-readable-with-bbedit.md" >}}
