---
author: slowe
categories: News
comments: false
date: 2006-06-03T15:47:17Z
slug: pn-agent-filter
tags:
- Networking
- Citrix
title: PN Agent Filter
url: /2006/06/03/pn-agent-filter/
wordpress_id: 258
---

Via [thincomputing.net](http://www.thincomputing.net/comment.php?comment.news.2052), I was alerted to the [PN Agent Filter](http://www.thomaskoetzing.de/index.php?option=com_content&task=view&id=69&Itemid=108), a mechanism for hiding applications from the Program Neighborhood Agent.

Apparently (I haven't yet tested it myself), it hides applications that have a "#" placed at the beginning of the published application. Seems straightforward enough, but the real question I have is why? If the PN Agent only shows those applications the user is authorized to see (based on the permissions of the published application), why do you need to hide applications? The only possible scenario I can come up with is that applications _should_ appear in an application set in the full Program Neighborhood client, but _should not_ appear in the PN Agent. In that case, a utility like this would be helpful. But how often would this really be the case?
