---
author: slowe
categories: Tutorial
comments: true
date: 2012-04-03T22:02:40Z
slug: killing-ads-in-rss-feeds-in-vienna
tags:
- macOS
- Web
title: Killing Ads in RSS Feeds in Vienna
url: /2012/04/03/killing-ads-in-rss-feeds-in-vienna/
wordpress_id: 2574
---

Almost five years ago (in mid-2007) I wrote about how to [kill ads in RSS feeds in NetNewsWire][1]. That technique has been a lifesaver for me, as I rely heavily upon RSS feeds to stay up-to-date with information and trends.

Recently, though, I started experimenting with [Vienna](http://www.vienna-rss.org/), the open source RSS reader for Mac OS X. One of the first things I noticed was the ads in the RSS feeds in Vienna, so I started searching for a way---if one existed---to bring the same ad-blocking functionality to Vienna.

Fortunately, there _is_ a way.

Here are the general steps to follow. First, create a Styles folder in `~/Library/Application Support/Vienna`. This is where you'll store any custom styles and the CSS code that will block the ads.

Next, copy your ad-blocking `userContent.css` into the newly-created Styles folder. You can get ad-blocking code from a variety of sources; I got mine [here](http://www.ollicle.com/2005/aug/15/feed_ad_block.html). If the filename is something _other_ than `userContent.css`, make a note of the filename as you'll need it later.

Next, download a user-contributed style from [here](http://www.vienna-rss.org/?page_id=76). Unzip the downloaded file and remove the ".viennastyle" extension. Removing the extension will cause it to revert to being a normal folder.

Copy this now-normal folder (from which you removed the ".viennastyle" extension) into the `~/Library/Application Support/Vienna/Styles` folder you created earlier. You'll note a CSS and an HTML file in that folder.

After you've copied the now-normal folder into the correct location, open the folder for the new style and edit the `stylesheet.css` file to include this content at the top of the file (replace `userContent.css` with the filename noted earlier, where applicable):

    @import url(../userContent.css);

Once all this is done, save your changes, close all files, and restart Vienna. When you re-open Vienna, select the new style that you just installed.

And that should be it! It's really a pretty easy process, once you understand that you have to use a customer-contributed style (you can't do this, as far as I know, with any of the built-in stylesheets).

[1]: {{< relref "2007-07-20-killing-ads-in-rss-feeds-in-netnewswire.md" >}}
