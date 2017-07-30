---
author: slowe
categories: General
comments: true
date: 2007-03-05T12:04:10Z
slug: wordpress-upgrade
tags:
- Web
- WordPress
title: WordPress Upgrade
url: /2007/03/05/wordpress-upgrade/
wordpress_id: 423
---

I had been stubbornly clinging to [WordPress](http://www.wordpress.org/) 1.5.2, which worked just fine and, in all reality, provided all the functionality I really needed. I figured it would probably be best to keep up with the newer versions of the software, however, so I decided to upgrade.

To give all credit to the WordPress developers, the upgrade process for WordPress itself was very straightforward and rather quick. The trouble came in having to update or replace certain plugins that weren't compatible with the newer versions of WordPress. Combine that with some [manual hacks](http://www.robinlu.com/blog/archives/57) I'd performed to help [ecto](http://ecto.kung-foo.tv/) work better with the [Ultimate Tag Warrior (UTW) plugin](http://www.neato.co.nz/ultimate-tag-warrior/), and you have yourself a real pain in the neck.

Fortunately, I stumbled across a really helpful nugget to assist in the ecto-UTW issue. The [XML-RPC Plugin for WordPress 2.1](http://blog.circlesixdesign.com/2007/01/26/utw-rpc-10/) incorporates the Robin Lu hacks (linked above), so that ecto can store tags in the keywords field and work properly with UTW. Yay---no more manually editing code!

The rest of the problems surrounded updating the theme templates with new PHP calls to reflect new plugins; for example, you'll note the "Recent Comments" section in the sidebar is different (required a new plugin).

Only a few things remain undone:

* The site search tags don't seem to work just yet. You get a "404 - Page Not Found" error when clicking on a site link tag.

* The site's theme has not been completely updated in all places, so you'll see different layouts in different sections of the site. I searched for a new prebuilt theme I could apply to the site, but couldn't really find any that I liked. I guess I'll just continue to tweak the existing theme.

* There are a few new plugins that I would like to use now that I've upgraded, but those haven't been added to the site or incorporated into the theme just yet.

I anticipate that things should be worked out reasonably quickly, but I appreciate everyone's patience in the meantime.
