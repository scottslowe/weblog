---
author: slowe
categories: Rant
comments: false
date: 2005-09-22T11:03:33Z
slug: gprs-vs-bluetooth
tags:
- Interoperability
- Messaging
title: GPRS vs. Bluetooth
url: /2005/09/22/gprs-vs-bluetooth/
wordpress_id: 89
---

As I've continued using my [Treo 650](http://www.palm.com/us/products/smartphones/treo650/), I've run into an apparent conflict between the Treo's GPRS functionality and Bluetooth HotSync support.

Yesterday, I was trying to HotSync my Treo back to my [PowerBook](http://www.apple.com/powerbook/) so that I could install [Chatopus](http://www.chatopus.com/), an IM client for the Treo. (BTW, Chatopus is pretty cool; it's an XMPP/Jabber IM client.) However, every time I tried to HotSync via Bluetooth, I'd get the "Port is in use by another application" error. Reviews of the HotSync log on the laptop showed nothing, which indicated to me that the problem was on the Treo. Just on a whim, I dropped the GPRS connection I was using to retrieve mail in VersaMail and tried again. It worked!

The only conclusion that I can draw is that having a GPRS connection prevents using Bluetooth to HotSync. Note that it doesn't prevent Bluetooth use in general, since I can use my Bluetooth headset without any problems. It just seems to prevent the use of Bluetooth for a HotSync operation.

This does not appear to be a limitation of Palm's HotSync software on the laptop, so I don't think that using something like [The Missing Sync](http://www.missingsync.com/missingsync_palmos.php) (from Mark/Space) will help matters. (I am looking into switching to that software, though.)

If anyone has any additional information on this conflict, please post more information in the comments.
