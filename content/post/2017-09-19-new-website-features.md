---
author: slowe
categories: General
comments: true
date: 2017-09-19T07:00:00Z
tags:
- Blogging
- Site
title: New Website Features
url: /2017/09/19/new-website-features/
---

One of the reasons I [migrated this site to Hugo][xref-1] a little over a month ago was that [Hugo][link-3] offered the ability to do things with the site that I couldn't (easily) do with Jekyll (via GitHub Pages). Over the last few days, I've taken advantage of Hugo's flexibility to add a couple new features to the site.<!--more-->

New functionality that I've added includes:

1. _Category- and tag-specific RSS feeds:_ Hugo can easily generate category- and tag-specific RSS feeds, enabling readers to subscribe to the RSS feed for a particular category or tag. On the taxonomy list pages---these are the pages that list all the posts found in a particular category or tag---there's now a small link to the RSS feed for that specific category or tag. (As an example, checkout [the list of posts in the "General" category][link-1].)

2. _(Truly) Related posts:_ The "Related Posts" section at the bottom of posts has returned, thanks to new functionality found in Hugo 0.27 (functionality that was, apparently, inspired in part by my experiences---see [the docs page][link-2]). This section lists 3 posts that are considered by Hugo to be related, based on the category and tags assigned to the posts.

It's not much, I know, but I'm hoping this new functionality makes it easier for readers to find (and stay in touch with) content that is relevant to their specific interests and needs.

If you have feedback for me on what additional functionality you'd find useful on the site, feel free [to hit me up on Twitter][link-4]. Thanks for reading!



[link-1]: /categories/general/
[link-2]: https://gohugo.io/content-management/related/#performance-considerations
[link-3]: https://gohugo.io/
[link-4]: https://twitter.com/scott_lowe
[xref-1]: {{< relref "2017-08-10-information-recent-site-migration.md" >}}
