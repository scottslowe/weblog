---
author: slowe
categories: Rant
comments: false
date: 2006-06-06T17:24:36Z
slug: citrix-access-essentials-unsupported-features
tags:
- Networking
- Citrix
title: Citrix Access Essentials Unsupported Features
url: /2006/06/06/citrix-access-essentials-unsupported-features/
wordpress_id: 262
---

While I'm not a Citrix expert, I have designed, implemented, and supported Citrix environments for a while now. I was therefore a bit surprised when I read an [article about some of the unsupported features](http://www.thincomputing.net/comment.php?comment.news.2034) in [Citrix Access Essentials](http://www.citrix.com/lang/English/ms/ms_24616.asp).

The article references a [Citrix Knowledge Base document](http://knowledgebase.citrix.com/kb/entry.jspa?externalID=CTX107097) that describes some of the unsupported features in Citrix Access Essentials. Citrix Access Essentials, in the event you aren't aware, is an SMB bundling of Citrix products in an effort to make their solutions more competitive in the smaller environments.

It's fully understandable that vendors will remove certain pieces of functionality from their products when creating "SMB" versions; there are simply some things that smaller business don't need their software to do. In this instance, no one would expect support for multiple servers in a farm, load balancing of published applications across multiple servers, or similar features. Small businesses just don't need that.

What _did_ catch my eye, though, was this note about Program Neighborhood and Program Neighborhood Agent:

>Connections via Program Neighborhood or Program Neighborhood Agent are not supported. Only connections through Web Interface are supported.

Huh? No support for Program Neighborhood? This completely caught me off-guard. This was _not_ the kind of functionality that I expected Citrix to remove from Access Essentials. This substantially differentiates Access Essentials from other Presentation Server-based solutions---not just by limiting certain enterprise-oriented features, but instead by removing common functionality. The commonality of being able to use Program Neighborhood to connect to just about any server running MetaFrame or Presentation Server is now broken, since Access Essentials only supports the web interface.

Otherwise, the list of unsupported features is pretty much what I expected it to be. If you work with Citrix Presentation Server in any sort of design, implementation, or support role, I encourage you to have a look at the full [Citrix Knowledge Base document](http://knowledgebase.citrix.com/kb/entry.jspa?externalID=CTX107097).
