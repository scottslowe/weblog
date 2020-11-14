---
author: slowe
categories: Explanation
comments: true
date: 2007-07-20T12:20:34Z
slug: killing-ads-in-rss-feeds-in-netnewswire
tags:
- macOS
- Web
title: Killing Ads in RSS Feeds in NetNewsWire
url: /2007/07/20/killing-ads-in-rss-feeds-in-netnewswire/
wordpress_id: 491
---

I don't like advertisements in my RSS feeds. I just don't. It's not that I begrudge the authors the ability to monetize their content; that's their choice, and I can certainly understand the need to pay for hosting and bandwidth costs. The day might even come one day when I am faced with the same issues here on this site. Even so, I don't like ads in the feeds. After all, if it's a good site, I am very likely to visit the site anyway, even with full feeds, so that I can comment, view others' comments, or see related posts.

So, when ads started showing up in my RSS feeds from a variety of sources, I looked for a way to kill them. I came across [this article](http://www.ollicle.com/2005/aug/15/feed_ad_block.html) (at least, I believe it was this one---I'm not entirely sure) that suggested the use of userContent.css to block ads in [NetNewsWire](http://www.newsgator.com/individuals/netnewswire/). It works like a champ!

Basically, you have to edit the NetNewsWire stylesheet to include a reference to your ad-blocking code, like this:
    
    @import url(../userContent.css);

Then put the `userContent.css` file of your choice (apparently there are many; I think I pulled down one of the files linked to in that article) in the `~/Library/Application Support/NetNewsWire/Stylesheets` folder, restart NNW, and away you go! No more pesky ads in the RSS feeds.

In addition to ads, the "FeedFlare" functionality offered by [FeedBurner](http://www.feedburner.com/) irks me as well. If I want to [digg](http://www.digg.com/) it, bookmark it with [del.icio.us](http://del.icio.us/), or whatever, I'll do that---I don't need links in the content of the feed to help me. I suppose that functionality is useful to some readers, and probably helps increase readership and visibility of the site, but I still don't like it. Fortunately, that is easily blocked using this same technique. You only need to tweak the `userContent.css` to include the additional URLs used by FeedBurner to add the FeedFlare content. (By the way, this is not a knock against FeedBurner; I use FeedBurner too. I just don't like FeedFlare.)
