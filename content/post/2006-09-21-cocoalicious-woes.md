---
author: slowe
categories: Rant
comments: true
date: 2006-09-21T16:36:28Z
slug: cocoalicious-woes
tags:
- Macintosh
- Web
title: Cocoalicious Woes
url: /2006/09/21/cocoalicious-woes/
wordpress_id: 338
---

A quick Google search turned up [this posting](http://weblog.scifihifi.com/2006/08/12/delicious-api-url-switch/) by Buzz Anderson, the developer of Cocoalicious, on his Sci-Fi Hi-Fi weblog. The posting was short and sweet, indicating all that was required was a simple change in the Preferences---exactly what I had found myself. As you review the comments, however, you see that a number of users are reporting that even with the URL change in the preferences, Cocoalicious is still not working. And for some, like me, the change worked initially but stopped working later.

The common error that everyone is seeing appears to be this:

    2006-09-21 16:14:44.961 Cocoalicious[308] PARSE 
    ERROR: NSError â€œError NSXMLParserErrorDomain 5â€ 
    Domain=NSXMLParserErrorDomain Code=5
    2006-09-21 16:18:58.210 Cocoalicious[311] NSError â€œError 
    NSURLErrorDomain -1012â€ Domain=NSURLErrorDomain Code=-1012 
    UserInfo={
        NSErrorFailingURLKey = https://api.del.icio.us/v1/posts/update?; 
        NSErrorFailingURLStringKey = â€œhttps://api.del.icio.us/v1/posts/update?â€; 
    }

Anyone have any idea what might be going on? I have determined, from reviewing the Console logs, that you should _not_ place a trailing slash after the URL in the Cocoalicious preferences; this causes the application to create a URL like this:

    https://api.del.icio.us/v1//posts/update?

Obviously, that won't work. However, even with the trailing slash not present, my installation of Cocoalicious immediately goes offline as soon as I launch it. Trashing the preferences file didn't seem to help, either.

Any insight would be greatly appreciated. I've come to rely upon del.icio.us and using a browser to view my bookmarks is just too cumbersome.

**UPDATE:** Macintosh users also running delimport (a Spotlight importer for del.icio.us bookmarks) should update their version of delimport to version 0.2, available from [the delimport website](http://www.ianhenderson.org/delimport.html). I just updated delimport myself and new del.icio.us bookmarks are now showing up (they weren't before).

**UPDATE 2:** FYI, I tried a couple of other posting clients, like [Postr](http://www.fromconcentratesoftware.com/Postr/) and [SiteTagger](http://www.fromconcentratesoftware.com/SiteTagger/), and also received the same NSURLErrorDomain code as with Cocoalicious. However, [Pukka](http://codesorcery.net/pukka) seems to work just fine.
