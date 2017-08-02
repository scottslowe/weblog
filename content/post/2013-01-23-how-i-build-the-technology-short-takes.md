---
author: slowe
categories: Explanation
comments: true
date: 2013-01-23T09:00:00Z
slug: how-i-build-the-technology-short-takes
tags:
- Productivity
- Writing
title: How I Build the Technology Short Takes
url: /2013/01/23/how-i-build-the-technology-short-takes/
wordpress_id: 3071
---

Shortly after I published [Technology Short Take #27][1], a reader asked me on Twitter how I managed the information that goes into each Technology Short Take (TST) article. Although I've blogged about my productivity setup before, [that post][2] is now over two years old and horribly out of date. Given that I need to post a more current description of my tools and workflow, I thought I'd take a brief moment to answer the reader's question. Here's how I go about building the Technology Short Take articles.

I've mentioned before that I have three "layers" of tools: a consumption layer, an organization layer, and a creation layer. I'll break down the process according to these layers.

## The Consumption Layer

This is where I go about actually finding the content that gets pulled into a TST post. There's nothing terribly unique here; I have a collection of RSS feeds which I subscribe and I get content from people I follow on Twitter. I will also visit various Usenet newsgroups and certain IRC channels (only on `irc.freenode.net`) from time to time.

If you're interested in seeing the RSS feeds to which I subscribe, here's [an OPML list of all my subscriptions](/public/dl/current-rss-subscriptions.opml).

The majority of the content I consume is web-based, so when I find an article that I want to use in a TST post, I'll save that as a Safari web archive. I wish there was a more platform-independent method, but as yet I haven't found a good solution. Once I've saved a page for future use, then we move into the organization layer.

## The Organization Layer

As content is discovered via any of the various consumption layer tools, I then need to get that content "sucked" into my primary organization layer tool. I use a really, really fancy tool---it's called the file system.

When I save a web page that I'm planning on including in a TST article, I generally save it, by default, to the Desktop. I have a program named [Hazel](http://www.noodlesoft.com/hazel) that watches the Desktop and Downloads folders for web archive files, and automatically moves them to a WebArchives folder. From there, I use a couple of saved Spotlight searches to identify newly-created web archives that don't have a source URL or don't have any OpenMeta tags assigned. For these newly-created web archives, I use the Spotlight comments field to store the source URL, and I use an application named [Tagger](http://hasseg.org/tagger/) to assign OpenMeta tags.

Once a web archive has its source URL and OpenMeta tags assigned, then I have a group of saved Spotlight searches the group files together by topic: virtualization, storage, OpenStack, Open vSwitch, etc. This makes it super easy for me to find web archives---or other files---related to a particular topic. All these saved searches are built on queries involving the OpenMeta tags.

Content will remain here until either a) I use it in a TST article and no longer need it; or b) I use it in a TST article but feel it's worth keeping for future reference. I might keep content for quite a long time before I use it. Case in point: the Q-tools stuff from Dave Gray that eventually found its way into some of my VMUG presentations was something I found in 2009 (it was published in 2008).

## The Creation Layer

After collecting content for a while, a scheduled, recurring [OmniFocus](http://www.omnigroup.com/applications/omnifocus/) action pops up reminding me to write the next TST post. At this point, I go back to my organization layer tools (saved Spotlight searches and content folders) to pull out the various pieces of information that I want to include. I write the post in Markdown using [TextMate](http://macromates.com/), building off a skeleton template that has all the content headers already in place.

Using the saved searches I described above, I'll search through my content to see what I want to include in the TST post. When an item is included in a TST blog post, I'll write my thoughts about the article or post, then grab the source URL from the Spotlight comments to make a hyperlink to the content. If the content is useful and informative, I might keep it around; otherwise, I'll generally delete the saved web archive or bookmark file. I repeat this process, going through all my saved content, until I feel that the TST post is sufficiently full of content.

Then, because it's all written in Markdown, I convert the post to HTML and actually publish it to the site using the excellent [MarsEdit](http://red-sweater.com/marsedit/) application. TextMate makes this incredibly easy with just a few keystrokes.

And that's it! That's the "mystery" behind the Technology Short Take articles. Feel free to post any questions or thoughts you have about my workflow and tools in the comments below. Courteous comments are always welcome.

[1]: {{< relref "2012-12-06-technology-short-take-27.md" >}}
[2]: {{< relref "2010-05-02-my-current-getting-things-done-setup.md" >}}
