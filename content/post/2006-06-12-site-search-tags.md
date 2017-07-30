---
author: slowe
categories: General
comments: false
date: 2006-06-12T09:02:34Z
slug: site-search-tags
tags:
- Blogging
- Web
- WordPress
title: Site Search Tags
url: /2006/06/12/site-search-tags/
wordpress_id: 267
---

I've updated the site to use a tagging plugin called [Ultimate Tag Warrior](http://www.neato.co.nz/ultimate-tag-warrior/). It's taken some time to get things working the way I'd like, but it's working now and I'm really pleased with how things have turned out. Read on for more details.

I had been using [ecto](http://ecto.kung-foo.tv/) (my Mac OS X-based blogging client) to add [Technorati](http://www.technorati.com/)-specific tags to my posts. This was all well and good, but these tags were not getting added to my RSS feeds and therefore weren't getting picked up fully by Technorati, even though I was pinging Technorati every time I published an entry.

In searching for an answer, I came across this plug-in. Additional research showed me to how make ecto work more fully with this plugin by placing tags in the keywords for the post; those instructions can be found [here](http://www.robinlu.com/blog/archives/57) and [here](http://www.robinlu.com/blog/archives/86). It was bit unnerving to be editing the `xmlrpc.php` file, but everything seems to work just fine now, and ecto happily places the tags in the keywords as it is supposed to.

In addition, this plugin gives me the ability to automatically generate site search tags (the current type being displayed, which merely show all other posts tagged the same way), Technorati tags, and [del.icio.us](http://del.icio.us/) tags, among others. I haven't enabled those yet, but probably will soon. You can also display related tags (I am). Finally, RSS feeds are exposed for tags as well, so that visitors can follow a feed for articles tagged a certain way. There is a great deal of functionality to this plugin; I heartily recommend it to fellow WordPress users.

I still have a few clean-up items to do (fix-up older blog posts and resolve one niggling issue with a trailing slash when using site search tags), but otherwise it is working perfectly. I hope the new functionality is useful to you!
