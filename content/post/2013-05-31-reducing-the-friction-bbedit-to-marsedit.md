---
author: slowe
categories: Explanation
comments: true
date: 2013-05-31T11:47:15Z
slug: reducing-the-friction-bbedit-to-marsedit
tags:
- Blogging
- macOS
- Productivity
- Writing
- Markdown
title: 'Reducing the Friction: BBEdit to MarsEdit'
url: /2013/05/31/reducing-the-friction-bbedit-to-marsedit/
wordpress_id: 3199
---

In some of the presentations that I give on productivity and efficiency, one of the things I mention is _reducing the friction_; that is, making processes more streamlined so they're easier to perform. In this post, I'm going to describe one way I reduced the friction for producing and publishing blog posts using [BBEdit](http://www.barebones.com/products/bbedit/), [TextSoap](http://www.unmarked.com/textsoap/), [MarsEdit](http://www.read-sweater.com/marsedit/), and some AppleScript.

It's no secret that I've become a huge fan of [Markdown](http://daringfireball.com/markdown/), the human-readable plain text "markup" format created by Jon Gruber. The _vast_ majority of all my content is now created in Markdown, and then converted to RTF (to share with my Office-using co-workers), PDF (for broader publication), or HTML (for publishing online). Until very recently, my blog publishing process looked something like this:

1. Write the blog post in Markdown, using [TextMate](http://macromates.com/).

2. Using a built-in Markdown binary in TextMate, convert the Markdown into HTML.

3. Run the raw HTML through TextSoap (very handy tool) to remove smart quotes and curly apostrophes.

4. Paste the parsed HTML into MarsEdit for publication to my blog.

While it seems complicated, it wasn't terribly complicated---but it wasn't as seamless as it could be. So, I set out to improve the process. The first big change was a switch from TextMate to BBEdit, which is more extensible (and kept up to date by the developer). That change allowed me to do two things:

* Switch from the built-in Markdown support in TextMate to using a separate (and more up-to-date) MultiMarkdown binary maintained by Fletcher Penny.

* Introduce an AppleScript (BBEdit has outstanding support for AppleScript) to automate some portion of the process.

My first pass at automating the process just got me back to where I was before---writing Markdown in BBEdit, converting to HTML, cleaning the HTML with TextSoap, and pasting into MarsEdit. Not too impressive, but acceptable, and a process with which I was familiar. I stuck with that process for a while, primarily because it was a known entity. A couple of days ago, though, I asked myself: Can I do better? Can I be more efficient?

So, my second pass at automating the process is much more comprehensive. The AppleScript I wrote as a result of challenging myself to _reduce the friction_ does the following:

* Takes the Markdown from BBEdit and converts it to HTML.

* Using the HTML produced by the standalone MultiMarkdown binary, it then calls TextSoap to (in the background) clean the HTML according to a custom cleaner I'd created (the custom cleaner, called "Replace HTML Entities," just replaces curly quotes and curly apostrophes, which don't translate well on my site).

* Creates a new, blank blog post in MarsEdit, into which it pastes the cleaned HTML as the body of the post.

I store the script in BBEdit's scripts folder, which means I can invoke the script easily from within BBEdit.

The entire AppleScript is available [right here](https://gist.github.com/scottslowe/5686217) as a GitHub Gist.

Now, I can write my Markdown in BBEdit, invoke the script, and get dropped out to HTML code sitting in a new blog post in MarsEdit. All I need to do to publish the post at that point is supply the metadata (tags, categories, title, excerpt) and click Send to Blog. Done. (I used this process for this post, in fact.) How's that for reduced friction?
