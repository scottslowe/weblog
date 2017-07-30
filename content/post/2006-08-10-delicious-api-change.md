---
author: slowe
categories: Explanation
comments: true
date: 2006-08-10T16:44:40Z
slug: delicious-api-change
tags:
- Collaboration
- Macintosh
- Web
title: del.icio.us API Change
url: /2006/08/10/delicious-api-change/
wordpress_id: 317
---

Fortunately, the fix for Cocoalicious is really straightforward; simply go into the preferences, change the API URI to "https://api.del.icio.us/v1/", click OK, then exit and restart the application. All should be well after that. (At least, it worked for me.)

However, this also means that if you have any scripts or WordPress plug-ins, you may have to modify those as well. I have a plug-in that lists recent del.icio.us posts in the sidebar, and I'll need to see if that plug-in (or the plug-in's configuration) needs to be updated.

(Funny how a "little" thing like a URI can have a ripple effect like this.)
