---
author: slowe
categories: General
comments: true
date: 2016-02-07T00:00:00Z
tags:
- Blogging
- Site
title: Brief Site Update
url: /2016/02/07/brief-site-update/
---

As many of you may already know, GitHub recently [upgraded GitHub Pages to use Jekyll 3.0][link-3], and along with that upgrade comes a few (relatively) minor changes to the site as a result. Here are the details of what's changed here.

The most notable difference is a change in the site's feed URL. Whereas the feed _used_ to be available at `http://blog.scottlowe.org/feed/`, changes in how Jekyll 3.0 handles permalinks mean that URL is no longer sustainable. Thus, the feed is now available at the following URLs:

Served directly from this site: [http://blog.scottlowe.org/feed.xml][link-1]  
Served via Google FeedBurner: [http://feeds.scottlowe.org/slowe/content/feed/][link-2]

I've updated the site's 404 page to indicate the change in feed URL, but this won't (unfortunately) fix feed readers that are still pointing to the old URL; you'll have to update your feed reader manually to point to one of the new URLs. I sincerely apologize for the inconvenience.

The second change is still underway and will (hopefully) be completely transparent to most users. With the Jekyll upgrade, GitHub Pages no longer supports the use of the Pygments code highlighting engine. However, the replacement---rouge---is supposed to be completely compatible with Pygments, so when I convert the site over to the new highlighting engine it _should_ be business as usual for readers.

If you have any questions, or if you run into _any_ problems with the site, please let me know. You're welcome to drop me an e-mail (address is on my About page) or [hit me up on Twitter][link-4].



[link-1]: /feed.xml
[link-2]: http://feeds.scottlowe.org/slowe/content/feed/
[link-3]: https://github.com/blog/2100-github-pages-now-faster-and-simpler-with-jekyll-3-0
[link-4]: https://twitter.com/scott_lowe
