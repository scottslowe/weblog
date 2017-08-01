---
author: slowe
categories: Explanation
comments: true
date: 2015-01-08T03:42:00Z
tags:
- Blogging
- Jekyll
- Markdown
title: Converting URLs to Jekyll References
url: /2015/01/08/converting-urls-jekyll-refs/
---

In my post about [the story behind the migration][xref-1], I mentioned that I made extensive use of regular expressions ("regexes") to help reformat portions of the Markdown documents that are used by Jekyll to build this site. In this post, I wanted to briefly share one of the regexes I used (and am still using) to convert URLs to Jekyll references.

First, let me clarify what I mean by _Jekyll references._ Jekyll offers a tag (not to be confused with content tags, more like a function) named `post_url` that will automatically build the correct URL when passed the filename of a content source. For example, if my `_posts` directory had a Markdown file named `2015-01-02-my-first-blog-post-of-2015.md`, then I could use the filename (`2015-01-02-my-first-blog-post-of-2015`) inside a `post_url` tag, and Jekyll would automatically convert that to the appropriate permalink (URL) for that blog post. If the permalink ever changed for whatever reason, whenever the site is regenerated Jekyll would convert that tag to the new permalink. This helps you ensure that every time you update your site (which, when used on GitHub Pages like I'm doing, means every time you push commits to GitHub using `git push origin master`) your permalink references are up to date and correct. Nifty, eh?

&lt;aside&gt;Converting hard-coded URLs for other blog posts to Jekyll references with `post_url` is one of the things that took so much time during the migration.&lt;/aside&gt;

To help simplify this process, I wrote a regex that I used in conjunction with TextSoap that converts a URL pointing to a post on my site into a Jekyll reference.

Here's the regex that matches against URLs to posts on my own site:

	(\(?)http://blog.scottlowe.org/(2[0-9]{3})/([0-9]{2})/([0-9]{2})/(.*)/(\)?)

Using backreferences, the matching text is replaced with this:

	{% post_url $2-$3-$4-$5 %}

This takes a URL like this (this is the URL for the very first post on my site):

	http://blog.scottlowe.org/2005/05/11/welcome/

And turns it into this:

	{% post_url 2005-05-11-welcome %}

The nice thing about this regex is that it's still useful even after the migration. Why? Well, it's a lot easier to find the article I want to reference via an Internet search engine like DuckDuckGo or Google than it is to search through the Markdown source files. So, I find the article I want to reference, copy the URL, then use this regex to turn it into a Jekyll reference in another blog post. Handy!


[xref-1]: {{< relref "2015-01-06-the-story-behind-the-migration.md" >}}
