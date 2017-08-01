---
author: slowe
categories: General
comments: false
date: 2006-07-15T12:31:53Z
slug: tags-rss-and-other-site-changes
tags:
- Blogging
- Collaboration
- Tagging
- Tags
- Web
title: Tags, RSS, and Other Site Changes
url: /2006/07/15/tags-rss-and-other-site-changes/
wordpress_id: 298
---

I've been wrestling with [Technorati](http://www.technorati.com/) not indexing my weblog for quite a while now. It appears that ever since I [added Ultimate Tag Warrior to my weblog][1] and moved the tags into the keywords portion of the weblog (instead of the posting body), Technorati stopped indexing the site. It appears that the tags aren't being added to the RSS and Atom feeds in the content, and therefore Technorati won't properly index the site.

I could go back to putting the tags in the body of the posting, but I really don't feel like it, and I like having them in the keywords portion of the posting. Since the Technorati links weren't returning any posts on my weblog, I've just removed the Technorati search links on the category and tag pages. Perhaps that's childish, or selfish, or whatever, but the links just don't seem useful any more.

In addition, I've added links to search [del.icio.us](http://del.icio.us/) for related bookmarks when browsing the category pages, and I've added a link for subscribing to a category's RSS feed. Visitors now use the exact same links for both tags and categories to find related bookmarks and subscribe to the RSS feed.

Just for easy reference, each category's RSS feed is available at the following URL:

    http://blog.scottlowe.org/<category name>/feed

In addition, each tag's RSS feed is available at the following URL:

    http://blog.scottlowe.org/<tag name>/feed

By default, these URLs will return an RSS 2.0 feed. To access an RSS 0.92 feed, add "/rss" to the end of each of these URLs. There is no Atom feed for categories and tags just yet. There is an Atom 1.0 feed for the overall weblog, though.

Since each category's RSS feed is available from the category page, I've removed the links to the RSS feeds in the sidebar.

**UPDATE:** I also added the plugin [Tags in the Head](http://www.maxpower.ca/wordpress-plugin-tags-in-the-head/2006/04/17/) to add tags as meta keywords in the header of each page. This is a pretty handy plugin that also ties in nicely with [Ultimate Tag Warrior](http://www.neato.co.nz/ultimate-tag-warrior/).

[1]: {{< relref "2006-06-12-site-search-tags.md" >}}
