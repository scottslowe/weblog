---
author: slowe
categories: General
comments: true
date: 2020-12-01T15:55:00-07:00
tags:
- Blogging
- Site
title: Some Site Updates
url: /2020/12/01/some-site-updates/
---

For the last three years, the site has been largely unchanged with regard to the structure and overall function even while I continue to work to provide quality technical content. However, time was beginning to take its toll, and some "under the hood" work was needed. Over the Thanksgiving holiday, I spent some time updating the site, and there are a few changes I wanted to mention.<!--more-->

1. The site has been updated to use a much more recent version of [Hugo][link-1]. This change is largely invisible to readers, but a couple of the site changes are related to this upgrade. Specifically...
2. Although the main RSS feed for the site (found [here][link-2]) remains a full content feed, I ran into issues getting Hugo to use my custom RSS templates for generating the category and tag feeds (for example, t[he RSS feed for the "Tutorial" category][link-3], or [the RSS feed for the "Kubernetes" tag][link-4]). You'll now find that the category and tag feeds are summary feeds only as opposed to full content feeds. I do intend to restore them to full content feeds as soon as possible.
3. Finally, I've updated the "metadata line" when viewing a single article to include word count and estimated reading time. This isn't much of a change, but it _is_ a change, so I wanted to briefly mention it.

All existing links and cross-references should continue to work with no issues, but if you do happen to come across a link that doesn't work please let me know. You can hit [me on Twitter][link-5], or find me in any one of the various Slack communities I frequent. Thanks!

[link-1]: https://gohugo.io/
[link-2]: /feed.xml
[link-3]: /categories/tutorial/feed.xml
[link-4]: /tags/kubernetes/feed.xml
[link-5]: https://twitter.com/scott_lowe
