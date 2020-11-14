---
author: slowe
categories: Explanation
comments: true
date: 2011-10-08T09:00:47Z
slug: formatting-rtfs-with-textsoap
tags:
- Collaboration
- macOS
- Writing
title: Formatting RTFs with TextSoap
url: /2011/10/08/formatting-rtfs-with-textsoap/
wordpress_id: 2446
---

If there's one thing I miss about trying to avoid Microsoft Word and stick with more "open" file formats, it's styles. Yes, styles. See, I'm a long-time Microsoft Word user, and in a previous life I worked in a role where it was my job to provide consistent formatting for some very large documents (300, 500, or more than 700 pages sometimes). In situations like that, styles are an absolute **must**. So, driven by necessity, I learned about styles, how to use them, and eventually grew to rely very heavily on them.

Now that I'm trying eschew Microsoft Word for more lightweight applications and text-based file formats, I'm finding that I really miss styles. So, I set out to try and find a way to provide some of the consistent formatting I had with styles, but in a way that is in alignment with my goals. I built a solution using these tools:

  * [TextMate](http://www.macromates.com/), for creating [MultiMarkdown](http://fletcherpenney.net/multimarkdown/) source files

  * [Pandoc](http://johnmacfarlane.net/pandoc/), for converting the MultiMarkdown sources into RTF

  * [TextSoap](http://www.unmarked.com/), for applying some regex-based formatting to the RTF destination file.

It's not the greatest solution in the world, but it does work (so far). Here's the basic flow of the solution:

1. I create the source file in MultiMarkdown syntax using TextMate. Along the way, I add some user-defined markup in the document for formatting later. Specifically, I've placed "[h1]" at the start of level 1 headings, "[h2]" at the start of level 2 headings, etc. It's almost like another layer of markup within the MultiMarkdown syntax. (Yes, I know this kind of goes against the spirit of Markdown.)

2. I use pandoc to convert the Markdown source document into RTF (a process I've automated using AppleScript and [FastScripts](http://www.red-sweater.com/fastscripts/)). This creates a pretty plain and simple RTF document with my user-defined tags still present. Pandoc supports the idea of templates, but I'm not familiar enough with the syntax of RTF to create an RTF template.

3. TextSoap parses the document and uses the user-defined tags to modify the font family and font size accordingly. I use a regular expression (regex) to find lines with the user-defined tag at the beginning, and apply a certain format to that line. After the formatting has been applied, the user-defined tags are removed. After the file has been processed by TextSoap---which is a really handy tool, by the way; I recommend it for anyone who works with text a lot---then the document has the formatting that I want and I can proceed with generating a PDF or delivering the RTF file as is.

This process is by no means perfect, but it does allow me to generate all my content using Markdown in TextMate, then spin it off to RTF for quick and easy formatting using TextSoap. This splits the content creation and content formatting steps into separate steps, and allows me to focus on "content first, appearance later."

The next evolution in the process will be to use AppleScript to tie the Markdown-to-RTF conversion and the formatting together in a single step (yet another reason to choose TextSoap: it supports and can be driven by AppleScript for automation).

